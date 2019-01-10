# 数据流编程模型

- [抽象的级别](#抽象的级别)
- [程序和数据流](#程序和数据流)
- [平行的数据流](#平行的数据流)
- [窗口](#窗口)
- [时间](#时间)
- [有状态操作](#有状态操作)
- [检查容错点](#检查容错点)
- [基于 Streaming 的 Batch](#基于-streaming-的-batch)
- [NextStep](#nextstep)

## 抽象的级别

Flink 提供为开发者提供对 Streaming/Batch 的不同级别的抽象。

![levels_of_abstraction](imgs/levels_of_abstraction.svg)

- 最底层的抽象简单的提供 `stateful streaming`, 通过 `Process Function` 将其嵌入到 `DataStream API` 中。 它使用了一致的容错状态, 并允许用户自由的处理来自一个或多个 Stream 的事件。 此外, 用户可以注册时间时间和处理时间回调, 让程序能后实现复杂计算。

- 实践中, 大多数应用不需要上一个的底层抽象, 而是根据 `core-api` 如: [DataStream API(bounded/unbounded streams)](https://ci.apache.org/projects/flink/flink-docs-release-1.6/dev/datastream_api.html) 和 [DataSet Api(bounded data sets)](https://ci.apache.org/projects/flink/flink-docs-release-1.6/dev/batch/index.html)。 这些连贯的 API 为数据助力提供了常见的构建模块, 比如各种形式的用户用户指定转换, 连接, 聚合, 窗口, 状态等。 在这些 API 中处理的数据类型表示为各自编程语言中的类。  
低级 `Process Function` 与 `DataStream API`  集成使得某些操作进行低级抽象成为可能。 `DataSet API` 在有界数据集上提供了 如循环/迭代这种 额外的 primitives。

- `Table API` 声明了以 `Table` 为中心的 DSL, `Table` 是可以动态更改的。 [Table Api](https://ci.apache.org/projects/flink/flink-docs-release-1.6/dev/table_api.html) 遵循关系模型: Table 有一个固定的 `schema` (类似于关系数据库表), 并且这些 API 提供了类似的操作, 如`select`、`project`,`join`, `group by`, `aggregate`等。 `Table api` 声明性的定义了应该做什么逻辑操作, 而不是精确的指定操作代码。 尽管 `Table API` 可以由各种类型的用户定义函数扩展, 但它的表达能力不如 core api, 但使用起来更简洁。 此外, `Table API` 还会在执行前通过一个应用优化规则的优化器来优化执行步骤。  
`Table` 和 `DataStream/DataSet` 之间可以无缝的转换, 允许程序混合 `Table API` 和 `DataStream/DataSet API`。
- Flink 提供的最高级别的抽象是 [SQL](https://ci.apache.org/projects/flink/flink-docs-release-1.6/dev/table/index.html)。 这种抽象在语义和表达方式方面都和 `Table API` 相似, 都是将程序表示为 SQL 查询表达式。 SQL 抽象与 `Table API` 密切交互, SQL 查询可以在 `Table API` 定义的表上执行。

## 程序和数据流

Flink 程序的基本构建模块是 `Stream` 和 `transformations`(**注意, Flink的数据集也是内部流**)。 从概念上讲, `Stream` 是数据记录流, `transformations` 是将一个或多个 `Stream` 作为输入, 并最终生成一个或 `Output Stream` 的操作。

在执行时, Flink 程序被映射到数据流, 由 `Stream` 和 `transformation` 操作符组成。 每个数据流从一个或多个源开始, 以一个或多个接收器结束。 数据流类似于任意有向无环图(DAGs)。

![image](imgs/program_dataflow.svg)

通常在程序中的转换和数据流中的操作符之间存在一对一的关系, 然而有的转换可能包含多个转换操作符。

`Source` 和 `Sink` 记录在 [Stream connectors](https://ci.apache.org/projects/flink/flink-docs-release-1.6/dev/connectors/index.html) 和 [batch connector](https://ci.apache.org/projects/flink/flink-docs-release-1.6/dev/batch/connectors.html) docs 中。 转换记录在 [DataStream operators](https://ci.apache.org/projects/flink/flink-docs-release-1.6/dev/stream/operators/index.html) 和 [DataSet transformations](https://ci.apache.org/projects/flink/flink-docs-release-1.6/dev/batch/dataset_transformations.html) 中。

## 平行的数据流

## 窗口

## 时间

## 有状态操作

## 检查容错点

## 基于 Streaming 的 Batch

## NextStep
