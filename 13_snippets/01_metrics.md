[TOC]

[官网教程](http://metrics.dropwizard.io/3.2.2/getting-started.html)

# Getting Started

## setting up maven

```xml
<dependencies>
  <dependency>
  	<groupId>io.dropwizard.metrics</groupId>
    <artifactId>metrics-core</artifactId>
    <version>${metrics.version}</version>
  </dependency>
</dependencies>
```

## meters

测量事件在时间尺度上发生的频率例如(请求数每秒)。 加之平均频率，meters可以追踪1-， 5-， 乃至15-分钟的平均值

```java
private final MetricRegistry metrics = new MetricRegistry();
private final Meter requests = metrics.meter("requests");

public void handleRequest(Request request, Response response) {
  requests.mark();
}
```

这个meter会测量每秒的请求量

## Console Reporter

```java
ConsoleReporter reporter = ConsoleReporter.forRegister(metrics)
  .convertRatesTo(TimeUnit.SECONDS)
  .convertDurationsTo(TimeUnit.MILLISECONDS)
  .build();

reporter.start(1, TimeUnit.SECONDS);
```

## the registry

所有应用的metrics的容器， 全局static

```java
final MetricRegistry metrics = new MetricRegistry();
```

## Gauges 计量器

瞬时测试手段。例如测量一个队列的大小

```javascript
public class QueueManager {
  private final Queue queue;
  
  public QueueManager(MetricRegistry metrics, String name) {
    this.queue = new Queue();
    metrics.registry(MetricRegistry.name(QueueManager.class, name, "size"), new Guage<Integer>() {
      @Override
      public Integer getValue() {
        return queue.size();
      }              
    });
  }
}
```

每个在registry中的metric都有一个唯一的以.号连接的名字名字*things.count*, *com.example.things.latency*, MetricRegistry有一个静态的帮助方法来构建这些个名字

## counters

一个counter就是一个*atomicLong*的gauges实例。可以增加或减少。当我们想用一个更有效的方式测量工作中的队列

```javascript
private final Counter pendintJobs = metrics.counter(name(QueueManager.class, "pending-jobs"));
```

