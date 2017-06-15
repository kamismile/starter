[TOC]

# 广义微服务

- 服务快速发布，快速验证，容灾容备
- 小，问题容易定位
- 学习成本，开发成本较低，两到三天，两到三人快速成型
- 技术更新，技术多元化成本低，

## 引入的新概念 、组件（术语）

- r&d 注册与发现 去中心化 (相较于soa esb 等bus总线) 不代表没有承载中心角色的东西，比如注册中心，但并不像传统esb路由转发，c直接与p交互
- 配置中心 config server 环境相关配置项，动态配置（负载策略、容灾），业务配置
- 客户端负载 ribbon (算法: 轮询，可用等)
- 自保护 隔离 熔断 curcuit broker hystrix (信号量隔离、线程数隔离)  （超时，错误率）
- api gateway nginx zuul kong fabro 内外网络隔离， 关注分离(routing, logging, whitelist, auth)、
- hystrix dashboard 
- kaban

....

- 灰度发布 先加后减
- 金丝雀理论
- devops  -> Smart Ops 
- monitor 监控 jvm memory , gc frequence, log info, interface qps, slexus docker kingstone, filebeate
- ​



