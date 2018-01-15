# disruptor

[TOC]

## introduction



> it seems like Java's BlockingQueue. Like a queue the purpose of the disruptor is to move data between threads within the same process.
>
> however there are some key features that the disruptor provides that distinguish it from a queue. they are:
>
> - multicast events to consumers, with consumer dependency graph.
> - pre-allocate memory for events.
> - optionally lock-free

## core Concepts

- **RingBuffer**: the ring buffer is often considered the main aspect of the disruptor, however from 3.0 onwards the ring buffer is only responsible for the storing and updating of the data(events) that move through the disruptor. and for some advanced use cases can be completely replaced by the user.
- **Sequence**: the disruptor uses Sequences as a means to identify where a particular components is up to.Each Consumer(EventProcessor) maintains a sequence as does the disruptor itself. the majority of the concurrent code relies on the movement of these sequence values, hence the sequence supports many of the current features of an AtomicLong. In fact the only real difference between the 2 is that the sequence contains additional funcionality to prevent false sharing between sequences and other values.
- **Sequencer**: real core of the disruptor. the 2 implementations (single producer, multi producer) of this interface implements all of the concurent algorithms use for fast, correct passing of data between producers and consumers.
- **Sequence Barrier**: the sequence barrier is produced by the sequencer and contains references to the main published Sequence from the Sequencer and the sequences of any dependent consumer. it contains the logic to determin if there are any events available for the consumer to process.
- **Wait Stragegy**: the wait stragety determins how a consumer will wait for events to be placed into the disruptor by a producer.
- **Event**: the unit of data passed from producer to consumer. there is no specific code representation of the event as it defined entirely by the user.
- **EventProcessor**: the main event loop for handling events from the disruptor and has ownership of consumer's sequence.there is a single representation called `BatchEventProcessor` that contains an efficient implementation of the eventloop and will call back onto a used supplied implementation of the EventHandler interface
- **EventHandler**: an interface that is implemented by the user and represents a consumer for the disruptor.
- **Producer**: this is the user code that calls the disruptor to enqueue events. 



![disruptor with a set of dependent consumers](https://github.com/LMAX-Exchange/disruptor/wiki/images/Models.png)

## multicast events



this is the biggest behavioural difference between queues and the disrutpr. when you have multiple consumers listening on the same disruptor all events are published to all consumers in contrast to a queue where a single event will only be sent to a single consumer. the behaviour of the disruptor is intended to be used in cases where you need to independent multiple parallel operations on the same data. the canonical example from LMAX is where we have three operations, journalling(writing the input data to a persistent journal file), replication(sending the input data to another machine to ensure that these is a remote copy of the data), and business logic (the real processing work).the executor-style event processing, where scale is found by processing different events in parallel at the same is also possible using the workerpool. note that is bolted on top of the existing disruptor classes and is not treated with the same first class support, hence it may not be the most efficient way to achieve that paticular goal.



looking at figure above is possible to see that there are 3 event handlers listening (JournalConsumer, ReplicationConsumer and ApplicationConsumer) to the disruptor, each of these event handlers will receive all of the messages available in the disruptor (in the same order). this allow for work for each of these consumers to operate in parallel.



## consumer dependency graph

to support real world applications of the parallel processing behaviour it was necessory to support co-ordination



## Optionally lock-free

another key implementation detail pushed by the desire for low-latency is the extensive use of lock-free algorithms to implement the disrutor. all of the memory visibility and conrretness guarantees are implemented using memory barriers and /or compare-and-swap operations. there is only one use-case where a actual lock is required and that is within the BlockingWaitStrategy.this is done solely for the purpose of using a condition so that a consuming thread can be parked while waiting for new events to arrive.



## Getting Started

### Getting the disruptor

> 源码比jar包里面多很多有用的**测试用例**及性能**测试脚本**

### basic event produce and consume

```java
// event that will carry data
@Data
public class LongEvent {
  private long data;
}
// eventFactory that preallocate events
public class LongEventFactory implements EventFactory<LongEvent> {
  public LongEvent newInstance() {
    return new LongEvent();
  }
}

// consumer that handle these events
public class LongEventHandler implements EventHandler<LongEvent> {
  
  public void onEvent(LongEvent event, long sequence, boolean endOfBatch) {
    System.out.println("Event: " + event);
  }
}

// a source of these events , in this example assume that the data is from some sort of i/o devices
public class LongEventProducer {
  private final RingBuffer<LongEvent> ringBuffer;
  
  public LongEventProducer(RingBuffer<LongEvent> ringBuffer) {
    this.ringBuffer = ringBuffer;
  }
  
  public void onData(ByteBuffer bb) {
    long sequence = ringBuffer.next(); // grab the next sequence
    try {
      LongEvent event = ringBuffer.get(sequence);
      event.set(bb.getLong(0)); // fill with data
    } finally {
      ringBuffer.publish(sequence);
    }
  }
}

// post 3.0

public class LongEventProducerWithTranslator {
  private final RingBuffer<LongEvent> ringBuffer;
  
  public LongEventProducerWithTranslator(RingBuffer<LongEvent> ringBuffer) {
    this.ringBuffer = ringBuffer;
  }
  
  private static final EventTranslatorOneArg<LongEvent, ByteBuffer> TRANSLATOR = new EventTranslatorOneArg<>() {
    public void translateTo(LongEvent event, long sequence, ByteBuffer bb) {
      event.set(bb.getLong(0));
    }
  }
  
  public void onData(ByteBuffer bb) {
    ringBuffer.publicEvent(TRANSLATOR, bb);
  }
}

public class LongEventMain {
  public static void main(String[] args) {
    // Executor that will be used to construct new threads for consumers
    Executor executor = Executors.newCachedThreadPool();
    
    // the factory for the event
    LongEventFactory factory = new LongEventFactory();
    
    // specify the size of the ring buffer, must be power of 2.
    int bufferSize = 1024;
    
    // construct the disruptor
    Disruptor<LongEvent> disrutor = new Disruptor<>(factory, bufferSize, executor);
    
    // connect the handler
    disruptor.handleEventsWith(new LongEventHandler());
    
    // start the disruptor, starts all threads running
    disruptor.start();
    
    // get the ringBuffer from the disruptor to be used for publishing
    RingBuffer<LongEvent> ringBuffer = disruptor.getRingBuffer();
    
    LongEventProducer producer = new LongEventProducer(ringBuffer);
    
    ByteBuffer bb = ByteBuffer.allocate(8);
    for (long l = 0; true; l++) {
      bb.putLong(0, l);
      producer.onData(bb);
      Thread.sleep(1000);
    }
  }
}

// in java 8
public LongEventMain {
  // executor that will be used to construct new threads for consumers
  Executor executor = Executors.newCachedThreadPool();
  // specify the size of the ring buffer, must be power of 2.
  int bufferSize = 1024;
  
  // construct the disruptor
  Disruptor disruptor = new Disruptor<>()(LongEvent::new, bufferSize, executor);
  // connect the handler
  disruptor.handleEventsWith((event, sequence, endOfBatch) -> System.out.println("Event: " + event));
  // start the disruptor, starts all threads running
  disruptor.start();
  
  // get the ringBuffer from the disruptor to be used for publishing.
  RingBuffer<LongEvent> ringBuffer = disruptor.getRingBuffer();
  
  ByteBuffer bb = ByteBuffer.allocate(8);
  for (long l = 0; true; l++) {
    bb.putLong(0, l);
    ringBuffer.publishEvent((event, sequence, buffer) -> event.set(buffer.getLong(0)), bb);
    
  }
}

```

