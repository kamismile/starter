[TOC]

# 分布式调度

​	两大任务

 - 任务调度
    - 大量计算任务
    - 任务如何切分
    - 数据如何分割/运算
    - 监控运算状态
- 资源调度
  - 供给方
  - 不同业务间平衡资源 
  - 支持优先级抢占
- 市面上分布式调度系统
  - Hadoop MR
    - 主从架构
    - JobTracker单点问题，规模扩展瓶颈
    - JobTracker单点，没有failover,容错性差
    - 不利于功能扩展，不同任务采用不同的调度策略，热拔插
  - YARN
    - 将资源调度与任务调度区分
    - 仅支持内存级别资源调度， cpu, disk, net
    - 资源交互性能，会归还资源，反复分配
  - Mesos
    - scheduler与mesos master之间不能描述精确的资源需求
    - 一次资源分配需要两次通信交互(offer & accept)，调度效率低
    - 不支持资源抢占
  - aliyun-fuxi

## 任务调度

数据量增大时怎么办？

- 多线程
- 多进程

单台物理机器内存是有上限的

对海量数据的并发处理

- MR

  - 统计图书馆中所有属于自然科学的书本
  - 统计全部学生的成绩的平均分
  - 系统的日志分析
  - 网页搜索中的PageRank

- 技术要点

  - 数据本地化Locality

    - 将instance分配到对应数据最多的node
    - 不同的instance做到负载均衡

  - 数据shuffle MR, 抽象成一个partion函数，重载一个hash函数实现下面的三种

    - 1:1
    - 1:N
    - M:N

  - Instance重跑和BackUp instance

    - 硬件故障 / 软件故障

      - 重试 : 在另一机器上重跑
      - 长尾问题: 机器在性能和处理能力上存在差异，一个跑的慢，在另一台机器上跑相同数据，取最先完成的instance的结果
        - 触发条件
          - 运行时间超过其它Instance的平均运行时间
          - 数据处理速度低于其它instance平均值
          - 已完成的instance比例

      ​

## 资源调度

资源调度问题: 将cpu, memory, disk等资源合理分配给任务job

目标:

- 最大化集群资源利用率
- 最小化任务的等待时间
- 能支持资源配额
- 支持任务抢占

设计:

- 策略之优先级(Priority)

  - 每个job有priority值，整数值，值越小优先级越高，越靠前

  - 相同优先级按提交时间，先提交高优先级

  - 优先分配高优先级的job，剩余分配次优先级job

  - 抢占

    - 从最低优先级任务强制收回，分配给紧急任务
    - 抢占递归，直到被强占优先级不高于紧急任务

  - ​
- 策略之公平调度
  - 按优先级分组，同一优先级组内平均分配
  - 有剩余资源再去下一优先级组分配
- 策略之配额quota
  - 多个任务组成group，按不同的业务区分
  - 每个group的job所分配资源付费
  - 最低保障资源和max资源，类似Executors.Threadpool, coresize, maxsize, maxsize在idle之后可以被回收

## 容错机制

​	故障

 - 硬件故障
   - 磁盘
   - 主板
   - 内存条
- 软件故障
  - 存在BUG
  - 内存访问越界
  - 进程crash
  - 整机宕机
- 故障恢复
  - 正在运行的任务不应该被中断
  - 对用户是透明的
  - 自动故障恢复
  - 系统恢复时保持可用性
- 任务调度的故障恢复
  - worker failover 会被app maser在另一台机器上重试
  - app master failover 会被重启，因有snapshot, checkpoint, 会定期保存在磁盘上
- 资源调度的failover
  - soft state (软状态)， 重新发送一份过来
    - 机器列表
    - 每个app master的资源请求
  - hard state(硬状态)
    - 用户提交的配置信息

## 规模挑战

​	资源增多，性能不是线性下降的过程，因为当节点更多时，节点间的通信和调度开销将成为瓶颈

分布式系统设计目标之一就是横向扩展scale-out

- 增量的消息通讯
- 异步持续多线程
- 设计要点
  - 多线程异步
    - 独立泳道： 不同线程池处理不同任务
  - 增量资源调度



## 安全与性能隔离

核心问题： 安全隔离/性能隔离

- 如何鉴别消息是否合法： 集群
  - capability访问控制：攻击性和恶意程序不会扰乱集群运行状态
    - 需要预先知道集群上有哪些用户，在集群部署前
  - 动态: 不知道未来运行什么作业
    - token方式： 动态口令
- 多进程之间如何安全隔离： 单机 
  - 进程级别沙箱(sandbox)隔离
- 性能隔离
  - vm(kvm, xen) , docker(lxc+虚拟化), LXC

## 分布式调度的发展方向

- 在线应用

  - 网页服务: 进程起来以后不会退出，输入数据没有明显的起止节点

- 离线处理

  - mapreduce：输入数据固定的，就是一个文件或文件中的某一段

- 在线应用与离线任务混跑

  - 在线应用对响应时间(Latency)要求很高
  - 离线应用对调度的吞吐率(Throughput)要求高  (cpu, net io , disk io)
  - 如何将整个公司内部的在线业务与离线业务并存起来，最大化的利用集群的物理资源，同时，又不干扰在线应用对延迟的要求
  - 电商集群
    - 如何进行电商和离线任务混跑: 超迈
      - 电商集群处在休息状态下，能够在很多在线的进程机器上，往上面调度一些离线的计算，能够将剩余的CPU和内存的计算能力利用起来
    - 实时计算
      - 如何做好batch（批处理)
      - 如何做到容错，规模
        - 大数据实时分析 spark / storm / flink
        - bi bw
      - 通过集群管理员的配置
      - 通过发现集群白天使用量和晚上使用量的配置
        - 什么时候该缩减worker的数量？
        - 什么时候该增加worker的数量?
        - 弹性调节:
          - 在业务不繁忙时，释放集群资源 ，在集群业务量上来的时候，能够要到资源，并且拉起一组常驻进程
        - 更大规模，更多并发量
          - 时间维度， 有些白天，有点晚上
          - 资源维度 cpu, 内存
          - 如何来支撑
          - 如何 在做到资源的调度
          - 如何来做到更快的资源调度
          - 如何来做到更大规模的任务调度

  ​

  ​

  ​

  ​
# 虚拟化技术应用，原理与实践

## 虚拟化技术应用: 弹性计算

- 云计算典型应用

- Elastic Computer Service 简称ECS

  - 管理方式比物理服务器更高效稳定安全
  - 降低开发运维的难度和整体IT成本
  - 使客户能够更专注于核心业务创新

  ​

## 虚拟化技术原理探析

- 虚拟化定义及分类

  - 任何计算机的问题都可以通过另一层重定向解决
  - 对计算机资源的抽象
    - 进程级虚拟化 Java -应用层面虚拟化
    - 系统虚拟化 - 平台层面虚拟化

- 技术的分类

  - full virtualization
  - para-virtualization

- virtual machine monitor : hyper visor

-  物理机与虚拟机对比

- 虚拟机监视器的标准

  - 必须能够控制硬件
  - 必须有效隔离客户机
  - 多客户机之间强隔离

- 虚拟机技术的标准

  - 等价性
    - 不要求与物理硬件一致，但功能上需要等价
  - 高效性
    - 性能损失不能太大

- 常见的虚拟机监视器软件

  - xen
  - kvm
  - hyper-v
  - vmware esx server
  - vmware workstation
  - virtualbox

  ### CPU虚拟化

  - x86 不支持虚拟化

  - binary translation ： 让x86支持虚拟化

    - 实现复杂，性能损失较大 50% ~ 60%

  - para-virtualization 98%  

  - 硬件辅助虚拟化VT-X

  - | Binary Translation  | 软件扫描      | 效率低  |            |
    | ------------------- | --------- | ---- | ---------- |
    | Para-Virtualization | 修改guest源码 | 效率高  | 不支持windows |
    | VT-X                | 芯片解决      | 完美解决 |            |

  ### 内存虚拟化

  - 内存虚拟化面临的挑战

    - 操作系统要求内存地址从0开始
    - 操作系统需要内存地址是连续的
      - 低段内存连续
      - 连续内存管理更高效
      - 使用superTLB加速访问效率

  - 内存重映射技术

    - MMU虚拟化技术

      - direct page table
        - guest和hypervisor共享页表
        - 优点: 切换效率高
        - 缺点: 安全性必须通过审计保证
      - virtual TLB（vTLB)
        - 相对简单
        - 只能处理访问过的虚拟地址
        - 效率低
      - shadow page table
        - 不需要修改内核
        - 效率较低， 32位效率84%， 64位65%
      - extended page table
        - 支持windows ,效率很高 95% ~ 98%

      ​

  ### IO虚拟化

  - IO设备的工作原理

  - ```mermaid
    graph LR;
    device --> |DMA| sm[shared memory]
    device --> |interrupt| cpu
    cpu --> |register access| device

    sm --> |DMA| device
    sm --> cpu
    cpu --> sm


    ```

    - 虚拟中断
    - 虚拟寄存器访问
    - 虚拟DMA

  - IO虚拟化 - 软件模拟

    - 效率低
    - 只在早期使用

  - IO虚拟化-PV

    - 重新定义IO架构
      - 前端驱动/后端驱动
    - 效率高 Xen KVM virtualbox

  - IO虚拟化-设备直通

    - 所有的IO操作必须可以直接发往物理机
    - 中断必须能被捕获
    - VT-d技术: dma remapping

  - IO虚拟化-SRIOV技术： 可以把一个物理设备虚拟化成多个设备比如一块网卡虚拟成多个

  - 总结 

    - 软件模拟
    - PV
    - 设备直通 + SRIOV + VT-d

## 开源虚拟化项目Xen&KVM

xen

​	

KVM

- linux内核模块
- 依赖intel vmx或amd svm
- 考虑资源隔离
- Qemu
  - 模拟cpu, 内存，i/o
  - CPU交由VT完成
  - 内存交由硬件完成
  - I/O由what I/O完成
- 其它性能增强
  - para-virtualized spinlocks 解决自旋锁干等性能损失
  - para-virtualized timing
  - para-virtualized huge page

共同点

- 硬件虚拟化实现
- 使用Qemu模拟非关键设备
- I/O：使用Para-virtualized模式

不同点

- Xen: Type-1 Hypervisor, 直接运行在物理机上
- KVM: Type-2 Hypervisor，运行在host宿主机上
- Xen: 300K (Hypervisor) + 300K(management fools)
- KVM: ~30k代码



##实践: Xen安全漏洞热修复技术

linux内核hotfix技术

内核技术的实现方式

- 预留pre-defined接口
- 允许插入内核module
- 有权访问内核内存
- 函数级别的替换

Xen hypervisor底层架构

Xen热修复的挑战

- xen是type-1 hypervisor,内存被严格隔离
- xen hypervisor被装载的地址是动态的
- xen hypervisor不支持模块插入

通过DMA访问Xen内存

- 构造DMA请求

  - 正常的文件读操作流程

    - ```mermaid
      graph TD;
      s1(用户态分配内存buffer准备读文件) --> s2(用户态发起read系统调用)
      s2 --> s3(内核态把buffer挂入map_sg_list)
      s3 --> s4(内核怘调用驱动接口触发DMA操作)

      ```

    - 热修复文件读流程

      - ```mermaid
        graph TB
        s1(用户态分配内存buffer准备读文件) --> s2(用户态把buffer地址以及hotfix信息传入内核)
        s2 --> s3(用户态发起read系统调用)
        s3 --> s4(内核态把buffer挂入map_sg_list)
        s4 --> s5(过滤DMA请求,修改DMA目的地址)
        s5 --> s6(内核态调用驱动接口触发DMA操作)
        ```

      - ​

- 截获DMA请求的能力

  - 利用内核hotfix替换dom0内核的这两个函数
  - 在新的map_sg/unmap_sg中加入过滤逻辑
  - 筛选中特定的DMA请求，修改DMA目的地址

- 计算修复代码的地址

  - 设备DMA只能使用物理地址
  - hypervisor hotfix物理地址计算公式加载

- 冷修复前后汇编代码对比

  - 修复后机器码被编译器优化严重

- 机器码补丁注入流程

  - 确定要注入代码的物理地址
  - 从Hypervisor读出相关代码的机器码(4K)
  - 和期待的pattern比较是否一致
  - 如一致，把机器码patch和读出代码merge,生成新的patch
  - 暂停所有vm执行
  - 把新的patch通过dma写回到hypervisor
  - 恢复所有vm运行

- 小结

  - 云计算业务线中安全是头等大事
    - 数据安全是高压线
  - 完善的安全问题处理预案
    - 响应/执行也需要及时和迅速
  - 热修复技术对安全运营至关重要
  - 多团队协作尤为重要
    - 技术很重要，强大的团队协作也很重要



