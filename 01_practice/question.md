[TOC]

# 01 jmeter压测心得(待验证)

1. 单一线程多次调用(方便精确获取平均值),此值描述的是正常情况下一次调用耗时， 记此时average为avg1

2. 逐渐加大线程数，平均时长average无明显变化，说明服务器端存在并行情况，那么应该可以观察到吞吐量throughput会随线程数有近似线性增长， 当加大线程数到average开始增长前的峰值时，此throughput * average(ms) / 1000 ≈ 服务器端并发上限 (线程切换等，可能上限值会略有浮动)，记此时本地线程数为num1(服务器端并发度)

3. 线程数继续增大，average会开始变长，说明客户端线程数超过服务器端并发数，多出的请求会出现阻塞无法及时响应积压延时等待，throughput基本不再变化，可以验证，基本满足规律： 当前average ≈ 当前线程数num2 / num1 * avg1

4. 线程数再继续增大，假定设定的超时时间为timeout1, 加大线程会出现超时，此时临界条件下线程大小应满足规律num3 = timeout1 / avg1 * num1

5. 继续加大线程数，会发生超时，假定当前线程数为num4, 超时异常数量占比应当满足规律: percent = num4 / num3 * 100%


jmeter当中使用察看结果树和使用gui方式测试时，结果会相当不准，因为测试结果要在gui界面中实时动态渲染和显示，试想一下，发送的请求数轻松过万，10万，百万更高都有可能，可以想像打开一个上10万行数据的excel效果如何，这种情况，如果发生数据先高后低的情况，基本上可确认是此原因，不妨移除掉察看结果树一项或使用命令行方式

