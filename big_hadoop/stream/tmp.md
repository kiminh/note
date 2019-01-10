# tmp note

## base information

1. 术语和基本概念
   1. 依靠多个流事件来计算结果, 必须将数据从一个事件保留到下一个事件。 这些保存下来的数据叫作计算的状态。
2. 人们希望流式处理的特征:
   1. 低延迟处理数据
   2. 高吞吐量
   3. 可以处理中断
   4. 可以容错且保证 exactly-once
   5. 没有数据的时候不会产生太大的开销(基于事件发生的时间而不是随意设置处理间隔来保证按正确的顺序跟踪事件)
   6. 系统生成的结果与事件实际发生的顺序一致(比如能够处理乱序事件)
   7. 能够准确的替换数据流
3. 技术演变
   1. Apache Storm(具体架构相关后面去查官网[TODO](storm.apache.org)):
      1. 低延时, 但吞吐量不高
      2. 并不能保证 exactly-once
      3. 开销较大
   2. Apache Spark Streaming
      1. 将连续事件中的流数据 分割成 一系列微小的批量作业, 分割的足够小(微批处理作业)
      2. 无法做到完全实时, 存在几秒 或 亚秒 的延迟
      3. 可以实现 exactly-once 语义, 保障状态一致, 但在延迟方面会付出较大代价
   3. Apache Flink:
4. exactly-once
5. Flink 起源
   1. 源自 Stratosphere 项目
   2. Apache 孵化
   3. 核心开发人员将其商业化
   4. 改名为 Flink
6. 如何统一批处理？
   1. 将批处理当作一种特殊的流处理
7. 运行环境
   1. Yarn
   2. 单机

## transitional frameworl & streaming feamework

### transitional

一个中心化的数据库系统, 用于存储事务性数据。

### 息传输层和流处理层

kafka

### 生产者和消费者解耦

## difference between stream and batch

用流处理器编程和用批处理器编程最关键的区别在于对时间的处理

## Checkpoint

### 一致性

1. at-most-once

2. at-least-once

3. exactly-once