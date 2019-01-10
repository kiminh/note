# Yarn

Hadoop 的资源管理器, 其基本思想是将资源管理和作业调度/监控功能拆分成独立的守护进程。 Yarn 有一个全局的资源管理器(Resource Manager) 和每个应用程序管理器(Application Master)。 每个应用程序是一个单独的进程。

实际生产中, 每个应用程序的 ApplicationMaster 是一个特定框架的库, 负责和 Resource Manager 协商资源, 并与 NodeManager 合作执行和监控任务。

整个框架如下图:

![yarn_architecture](imgs/yarn_architecture.gif)

ResourceManager 有两个主要的组件:

1. Scheduler
2. ApplicationMaster

- Scheduler

Scheduler 负责将资源分配给正在运行的应用程序, 这些应用程序收到容量、队列等约束。 Scheduler 只对 Job 进行调度, 不执行对应用程序状态的监控或跟踪。 同时, Scheduler 不能保证由于应用程序故障或硬件故障而重启失败的 Job。 Scheduler 根据应用程序的资源需求执行调度功能, 基于资源容器的抽象概念来实现, 资源容器包含 **内存, CPU, 磁盘, 网络** 等元素。

Scheduler 有一个可插拔策略, 负责在各种队列, 应用程序间划分集群资源。 目前主流的调度程序有: [Capacity Scheduler](http://hadoop.apache.org/docs/stable/hadoop-yarn/hadoop-yarn-site/CapacityScheduler.html) 和 [Fair Scheduler](http://hadoop.apache.org/docs/stable/hadoop-yarn/hadoop-yarn-site/FairScheduler.html)。

- ApplicationMaster

ApplicationMaster 负责接手工作提交, 协商执行特定与应用程序的 ApplicationMaster 的第一个容器, 并在失败时 重新启动 ApplicationMaster 容器的服务。 每个程序的 ApplicationMaster 负责与调度程序协商适当的资源容器, 跟踪其状态并监控进度。

- [基本操作](http://hadoop.apache.org/docs/stable/hadoop-yarn/hadoop-yarn-site/YarnCommands.html)

## Capacity Scheduler

Capacity Scheduler 是一个可插入的调度程序, 允许多租户安全地共享大型集群, 以便在分配容器的约束下及时分配应用程序的资源。 其核心思想是 Hadoop 集群中的可用资源在多个 Group 见共享, 这些 Group 根据计算需求共同为集群提供计算资源。 同时, Group 可以访问其他的 Group 未使用的任何过剩资源。 CapacityScheduler 还对来自单个用户和队列的初始化和刮起的应用程序进行了限制, 以确保集群的公平性和稳定性。

CapacityScheduler 提供的主要抽象是 **队列** 的概念。 通常由管理员设置, 以反映共享集群的 econimics。

### 特性

- Hierarchical Queue: 支持队列层次结构, 确保在允许其他队列使用空闲资源前, 在 Group 的子队列见共享资源, 从而提供更多的控制和可预测性。
- Capacity Guarantees
- Security: 每个队列都有严格的 acl, 以控制那些用户可以将应用程序提交到各个队列。 此外还有安全防护措施, 以确保用户不能查看/修改 来自其他用户的应用程序。 此外, 还支持每个队列和系统管理员角色。
- Elasticity: 空闲资源可以分配给任何超出其容量的队列。 当将来某个时间点上低于容量运行的队列对这些资源有需求时, 随着这些资源上调度的任务完成, 这些资源江北分配给低于容量运行队列上的应用程序(同时支持抢占)。 确保资源可以预测和以一种弹性的方式对队列可用, 防止集群的资源竖井。
- Multi-tenancy: 提供了一组全面的限制, 防止单个应用程序, 用户和队列独占队列或整个集群的资源, 以确保集群不会被 overwhelmed。
- Operability
  - Runtime Configuration: 管理员可以在运行时以安全的方式更改队列定义和属性(如容量, acl等), 最小化对用户的干扰。  同时提供了一个控制台, 供用户和管理员查看各队列当前的资源分配。 管理员可以在运行时添加其他队列, 但不能在运行时删除队列。
  - Drain application: 管理员可以在运行时暂停队列, 以确保现有应用程序到完成时, 不能提交新的应用程序。 如果队列处于停止状态, 则不能将新应用程序提交给它或其他子队列, 现有的应用程序将继续完成, 可以优雅地排干队列。 管理员可以启动已停止的队列。
- Resource-based Scheduling: 支持资源密集型应用程序, 在应用程序中可以选择比默认值更高的资源需求, 从而适应具有不同资源需求的应用程序。
- Queue Mapping based on User or Group: 该特性允许用户将作业映射到基于用户或组的特定队列。
- Priority: 允许以不同的优先级提交和调度应用程序, 较高的整数值表示应用程序具有较高的优先级。 目前只支持 FIFO 排序策略的应用程序优先级。

具体配置参见: [Capacity Schheduler Configuration](http://hadoop.apache.org/docs/stable/hadoop-yarn/hadoop-yarn-site/CapacityScheduler.html#Configuration)

## Fair Scheduler

Fair Scheduler 是一种将资源分配给应用程序的方法, 随着时间推移, 所有应用程序平均会得到相同份额的资源。 允许短周期的应用程序在合理的时间完成, 同时不会饿死长周期的应用程序。 Fair Scheduler 可以与应用程序的优先级一起工作, 优先级可以被用作权重, 以确保每个应用程序应高获得的总资源的比例。

### 具有可插入策略的分层队列


Fair Scheduler 支持分层队列。 所有队列都来自一个名为“root”的队列。 可用资源以典型的公平调度方式分布在根队列的子队列中。 然后, 孩子们以同样的方式把分配给他们的资源分配给他们的孩子。应用程序只能在叶队列上调度。 通过将队列作为其父队列的子元素放置在 Fair Scheduler 分配文件中, 可以将队列指定为其他队列的子队列。

队列的名称以其父队列的名称开头, 以句点作为分隔符。因此根队列下名为 `queue1` 的队列将被称为 `root.queue1`, 队列 `parent1` 下的队列 `queue2` 称为 `root.parent1.queue2`。 在引用队列时, 名称的根部分是可选的, 因此 `queue1` 可以称为 `queue1`, `queue2` 可以称为 `parent1.queue2`。

此外, 公平调度程序允许为每个队列设置不同的自定义策略, 以允许以用户希望的任何方式共享队列的资源。可以通过扩展`org.apache.hadoop.yarn.server.resourcemanager.scheduler.fair.SchedulingPolicy` 来构建自定义策略。 `FifoPolicy`, `FairSharePolicy` (默认)和`DominantResourceFairnessPolicy` 是内置的, 可以很容易地使用。

原始(MR1)公平调度程序中存在的某些外接程序还不受支持。 其中之一就是使用自定义策略来控制优先级在某些应用程序上的“提升”。

具体配置参见: [Fair Scheduler Configuration](http://hadoop.apache.org/docs/stable/hadoop-yarn/hadoop-yarn-site/FairScheduler.html#Configuration)

### Administrator

[管理员可执行的行为](http://hadoop.apache.org/docs/stable/hadoop-yarn/hadoop-yarn-site/FairScheduler.html#Administration)

## Resource Manager Restart

## Resource Manager HA

[配置参考](http://hadoop.apache.org/docs/stable/hadoop-yarn/hadoop-yarn-site/ResourceManagerHA.html#Configurations)

## YARN Timeline Server

[YARN Timeline Server](http://hadoop.apache.org/docs/stable/hadoop-yarn/hadoop-yarn-site/TimelineServer.html)

## YARN Timeline Service v.2

[The YARN Timeline Service v.2](http://hadoop.apache.org/docs/stable/hadoop-yarn/hadoop-yarn-site/TimelineServiceV2.html)

## 具体使用

[Writing YARN Applications](http://hadoop.apache.org/docs/stable/hadoop-yarn/hadoop-yarn-site/WritingYarnApplications.html)

## YARN Application Security

- Long-lived Yarn servoce
- Web UI & Rest API
- Yarn Application
- Checklist

## NodeManager

### Health Checker Service

### NodeManager Restart

## Docker Support
