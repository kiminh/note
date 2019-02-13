# Tuning Spark

- [数据序列化](#数据序列化)
- [内存调优](#内存调优)
  - [内存管理](#内存管理)
  - [决定内存消耗](#决定内存消耗)
  - [调整数据结构](#调整数据结构)
  - [调整垃圾回收](#调整垃圾回收)
  - [数据持久化的序列化](#数据持久化的序列化)
    - [Measuring the Impact of GC](#measuring-the-impact-of-gc)
    - [Advanced GC Tuning](#advanced-gc-tuning)
- [其他考虑](#其他考虑)
  - [并行级别](#并行级别)
  - [Reduce Task 内存使用](#reduce-task-内存使用)
  - [Broadcasting Large Variables](#broadcasting-large-variables)
  - [数据本地化](#数据本地化)
- [Summary](#summary)

translate from [Tuning Spark](http://spark.apache.org/docs/latest/tuning.html)

由于大多数Spark计算的内存特性, Spark程序可能会受到集群中任何资源(CPU、网络带宽或内存)的瓶颈。 通常, 如果数据适合存储在内存中, 瓶颈是网络带宽, 但有时, 还需要进行一些调优, 比如以序列化的形式存储 rdd, 以减少内存使用。

## 数据序列化

序列化在任何分布式应用程序的性能中都扮演着重要的角色。序列化对象很慢的格式, 或者消耗大量字节的格式, 将大大降低计算速度。通常, 这是优化Spark应用程序时应该做的第一件事。Spark的目标是在方便(允许在操作中使用任何Java类型)和性能之间取得平衡。它提供了两个序列化库:

- [Java Serialization](https://docs.oracle.com/javase/8/docs/api/java/io/Serializable.html): 默认情况下, Spark 使用 Java 的 `ObjectOutputStream` 框架序列化对象, 并且可以与创建的任何实现 `java.io.serializable` 的类一起工作。还可以通过扩展 `java.io.Externalizable` 来更紧密地控制序列化的性能。 Java序列化是灵活的, 但通常很慢, 并且会导致许多类的大型序列化格式。
- [Kryo serialization](https://github.com/EsotericSoftware/kryo): Spark 还可以使用 Kryo 库更快地序列化对象。 Kryo 比 Java 序列化快得多, 也更紧凑(通常是10倍), 但是不支持所有可序列化类型, 并且要求提前注册将在程序中使用的类, 以获得最佳性能。

可以通过 `conf.set("spark.serializer", "org.apache.spark.serializer.KryoSerializer")` 来配置 `SparkConf` 的方式初始化 Job 的序列化方式。 该项序列化配置不只在不同 worker 间 shuffing data 过程中起作用, 同时在持久化的序列化中起作用。 Kryo 不是默认值的唯一原因是自定义注册要求, 但是建议在任何网络密集型应用程序中尝试它。 由于 **Spark 2.0.0** 在使用简单类型, 简单类型数组或字符串类型对 `RDD` 进行 shuffling 时, 内部使用 Kryo 序列化器。

Spark 自动为 [Twitter chill](https://github.com/twitter/chill) 库中的AllScalaRegistrar 中介绍的许多常用核心 Scala 类包含 Kryo 序列化器。

通过以 `registerKyroClasses` 方法注册自定义 Kryo 类:

```scala
val conf = new SparkConf().setMaster("...").setAppName("")
conf.registerKryoClasses(Array(classOf[MyClass1], classOf[MyClass2]))
val sc = new SparkContext(conf)
```

如果对象很大, 可能还需要增加 `spark.kryoserialize.buffer` 配置。 该值需要足够大, 以容纳要序列化的最大对象。

如果没有注册自定义类, Kryo 仍然可以工作, 但是它必须为每个对象存储完整的类名, 这是一种浪费。

## 内存调优

在调优内存使用时, 有三个考虑因素: 对象使用的内存量(可能希望整个数据集都适合内存); 访问这些对象的成本; 垃圾收集的开销(如果对象的周转率很高)。

默认情况下, Java 对象访问速度很快, 但与字段中的原始数据相比, 很容易消耗2-5倍的空间。 这是由于几个原因:

- 每个不同的 Java 对象都有一个“对象头”, 大约 16 个字节, 包含指向其类的指针等信息。 对于数据很少的对象(比如一个 Int 字段), 这个值可能比数据大。
- Java 字符串在原始字符串数据上有大约 40 字节的开销(因为它们将其存储在字符数组中, 并保留额外的数据, 如长度), 由于字符串内部使用 `UTF-16` 编码, 因此将每个字符存储为两个字节。 因此, 一个10个字符的字符串可以很容易地消耗 60 个字节。
- 公共集合类, 如 `HashMap` 和 `LinkedList`, 使用链接数据结构, 其中每个条目都有一个“包装器”对象(例如 `Map.Entry`)。 这个对象不仅有一个头, 而且还有指向列表中下一个对象的指针(通常每个指针8字节)。
- 基本类型的集合通常将它们存储为包装类型, 如 `java.lang.Integer`。

### 内存管理

Spark 中的内存使用主要分为两类: 执行和存储。 执行内存是指用于在 shuffle, join, sort和 aggregation 中进行计算的内存, 而存储内存是指用于在集群中缓存和传播内部数据的内存。 在Spark中, 执行和存储共享一个统一的区域(M), 当不使用执行内存时, 存储可以获取所有可用的内存, 反之亦然。执行可能会在必要时回收存储, 但只能在总存储内存使用低于某个阈值(R)时才会回收。 换句话说, R描述了M中的一个分区, 其中缓存的块永远不会被回收。 存储可能不会因为实现的复杂性而收回执行。

这种设计确保了几个理想的性能。首先, 不使用缓存的应用程序可以使用整个空间执行, 避免不必要的磁盘溢出。其次, 使用缓存的应用程序可以保留最小存储空间(R), 其中的数据块不会被删除。最后, 这种方法为各种工作负载提供了合理的开箱即用性能, 而不需要用户了解如何在内部分配内存。

虽然有两种相关的配置, 但典型的用户不需要调整它们, 其默认值适用于大多数工作负载:

- `spark.memory.fraction` 将 M 的大小表示为(JVM 堆空间: 300MB)(默认0.6)的一部分。 其余的空间(40%)用于用户数据结构, Spark中的内部元数据, 以及在记录稀疏和异常大的情况下防止 `OOM` 错误。
- `spark.memory.storageFraction` 将 R 的大小表示为 M 的一部分(默认值0.5)。 R是M中的存储空间, 其中缓存的块不会被执行删除。

### 决定内存消耗

确定数据集所需内存消耗量的最佳方法是创建RDD, 将其放入缓存, 并查看web UI中的“Storage”页面。 页面将展示 RDD 占用了多少内存。

使用 `org.apache.spark.util.SizeEstimator#estimate()` 方法估计特定对象的内存消耗。 这对于尝试使用不同的数据布局来减少内存使用, 以及确定广播变量将占用每个执行器堆上的空间大小非常有用。

### 调整数据结构

当对象仍然太大而无法有效存储时, 减少内存使用的一种简单得多的方法是使用[RDD persistence API](http://spark.apache.org/docs/latest/rdd-programming-guide.html#rdd-persistence) (如: MEMORY_ONLY_SER)中的序列化存储图以序列化的形式存储它们, 从而减少内存使用。 然后 Spark 将每个 RDD 分区存储为一个大字节数组。 以序列化形式存储数据的惟一缺点是访问时间较慢, 因为必须动态地反序列化每个对象。 如果希望以序列化的形式缓存数据, 我们强烈推荐使用 Kryo, 因为它的大小比 Java 序列化小得多(当然也比原始 Java 对象小)。

### 调整垃圾回收

减少内存消耗的第一种方法是避免增加开销的 Java 特性, 例如基于指针的数据结构和包装器对象。 有几种方法可以做到这一点:

1. 将数据结构设计为对象数组和基本类型, 而不是标准的 Java 或 Scala 集合类(例如HashMap)。 [fastutil](http://fastutil.di.unimi.it) 库为与Java标准库兼容的基本类型提供了方便的集合类。
2. 尽可能避免使用带有大量小对象和指针的嵌套结构。
3. 考虑使用数字 id 或 `枚举对象` 而不是字符串作为键。
4. 如果的内存小于 32 GB, 那么将 JVM 标志设置为 `-XX:+UseCompressedOops`, 使指针为 4 个字节, 而不是 8 个字节。可以在 `spark-env.sh` 中添加这些选项。

### 数据持久化的序列化

当程序存储的 `RDD` 有很大的 **波动** 时, JVM GC 可能是一个问题。 (在只读取一次 RDD 并在其上运行许多操作的程序中, 这通常不是问题。)当 Java 需要清除旧对象以为新对象腾出空间时, 它将需要遍历所有 Java 对象并找到未使用的对象。 垃圾收集的成本与 Java 对象的数量成正比, 因此使用对象更少的数据结构(例如, 使用 `int` 数组而不是 `LinkedList`)可以大大降低此成本。一个更好的方法是以序列化的形式持久化对象, 如前所述: 现在每个 RDD 分区只有一个对象(字节数组)。在尝试其他技术之前, 如果 GC 有问题, 首先要尝试的是使用序列化缓存。

由于任务的工作内存(运行任务所需的空间)和节点上缓存的 `RDD` 之间的干扰, GC 也可能成为一个问题。

#### Measuring the Impact of GC

#### Advanced GC Tuning

## 其他考虑

### 并行级别

### Reduce Task 内存使用

### Broadcasting Large Variables

### 数据本地化

## Summary
