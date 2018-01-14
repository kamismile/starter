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

## debug sources

```java
 @Test
    public void t01() throws InterruptedException {
        Disruptor<LongEvent> disruptor = new Disruptor<>(LongEvent::new, 1024, r -> {
            return new Thread(r);
        });

        disruptor.handleEventsWith((event, sequence, endOfBatch) -> {
            System.out.println("event = " + event);
        });

        RingBuffer<LongEvent> ringBuffer = disruptor.start();

        ByteBuffer bb = ByteBuffer.allocate(8);
        for (int i = 0; i < 10; i++) {
            bb.putLong(0, i);
            ringBuffer.publishEvent((event, sequence, args) -> event.setValue(args.getLong(0)), bb);
            TimeUnit.SECONDS.sleep(1);
        }

        disruptor.shutdown();

    }

    @Data
    static class LongEvent {
        private long value;
    }
```

## AtomicReferenceFieldUpdater

### TODO

## Unsafe

```java
Field theUnsafe = Unsafe.class.getDeclaredField("theUnsafe");
theUnsafe.setAccessible(true);
Unsafe unsafe = (Unsafe)theUnsafe.get(null);

// alternatively
Constructor<Unsafe> unsafeConstructor = Unsafe.class.getDeclaredConstructor();
unsafeConstructor.setAccessible(true);
Unsafe unsafe = unsafeConstructor.newInstance();
```

### create an instance of a class without calling a constructor

```java
class ClassWithExpensiveConstructor {
  private final int value;
  private ClassWithExpensiveConstructor() {
    value = doExpensiveLookup();
  }
  private int doExpensiveLookup() {
    try {
      Thread.sleep(2000);
    } catch (InterruptedException e) {
      e.printStackTrace();
    }
    return 1;
  }
  
  public int getValue() {
    return value;
  }
}

@Test
public void testObjectCreation() {
	ClassWithExpensiveConstructor instance = (ClassWithExpensiveConstructor)
      unSafe.allocateInstance(ClassWithExpensiveConstructor.class);
  	assertEquals(0, instance.getValue());
}
```

