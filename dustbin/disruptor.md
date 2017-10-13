# disruptor

- ring buffer： store, update
- sequence:  atomiclong
- sequencer:  passing data between producer and consumer
- sequence barrier: logic determine if has events available for consumer to process
- wait strategy: 
- event: unit of data passed from producer to consumer
- eventProcessor: handling loop
- eventhandler:  consumer
- producer: 

![Disruptor](https://github.com/LMAX-Exchange/disruptor/wiki/images/Models.png)

```java
@Data
public class LongEvent {
    private long value;
}

public class LongEventFactory implements EventFactory<LongEvent> {
    public LongEvent newInstnace() {
        return new LongEvent();
    }
}

public class LongEventHandler implements EventHandler<LongEvent> {
    public void onEvent(LongEvent event, long sequence, boolean endOfBatch) {
        System.out.println("Event: " + event);
    }
  
}


public class LongEventProducer {
    
  private final RingBuffer<LongEvent> ringBuffer;
  
  public LongEventProducer(RingBuffer<LongEvent> ringBuffer) {
      this.ringBuffer = ringBuffer;
  }
  
  public void onData(ByteBuffer bb) {
      long sequence = ringBuffer.next();
      try {
          LongEvent event = ringBuffer.get(sequence);
        	event.setValue(bb.getLong(0));
      } finally {
          ringBuffer.publish(sequence);
      }
  }
}

public class LongEventProducerWithTranslator {
    private final RingBuffer<LongEvent> ringBuffer;
  
  	public LongEventProducerWithTranslator(RingBuffer<LongEvent> ringBuffer) {
        this.ringBuffer = ringBuffer;
    }
  
 	private static final EventTranslatorOneArg<LongEvent, ByteBuffer> TRANSLAOR = new EventTranslatorOneArg<> {
        public void translateTo(LongEvent event, long sequence, ByteBuffer bb) {
            event.set(bb.getLong(0));
        }
    }
  
  	public void onData(ByteBuffer bb) {
        ringBuffer.publishEvent(TRANSLATOR, bb);
    }
}

public class LongEventMain {
    public static void main(String... args) {
        Executor executor = Executors.newCachedThreadPool();
      	LongEventFactory factory = new LongEventFactory();
      
      	int bufferSize = 1024;
      
      	Disruptor<LongEvent> disruptor = new Disruptor<>(factory, bufferSize, executor);
      	
      
      	disruptor.handleEventsWith(new LongEventHandler());
      
     	disruptor.start();
      
      	
    }
}

```



