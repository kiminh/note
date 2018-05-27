# spark

## 基本概念

- RDD: Resilient Distributed Dataset, 弹性分布式数据集。
- Operation: 作用于 RDD 的各种操作, 包括 Transformation 和 Action。
- Job: 作业, 一个 Job 包含多个 RDD 及作用于相应 RDD 的各种操作。
- Stage: 一个作业分为多个阶段。
- Partition: 数据分区, 一个 RDD 中的数据可以分成多个不同的区。
- DAG: Directed Acycle Graph, 有向无环图, 反应 EDD 之间的依赖关系。
- Narrow Dependency: 窄依赖, 子 RDD 依赖于父 RDD 中固定的 Data Partition。
- Wide Dependency: 宽依赖, 子 RDD 依赖于父 RDD 中的所有 Data Partition 都有依赖。
- Caching Management: 缓存管理, 对 RDD 的中间计算结果进行缓存管理, 以加快整体的处理速度。

## RDD

RDD 是 Spark 的最基本抽象, 是对分布式内存的抽象使用, 以操作本地集合的方式来操作分布式数据集的抽象实现。
