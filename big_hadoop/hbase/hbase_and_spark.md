# HBase and Spark

- [Basic Spark](#basic-spark)
- [Spark Streaming](#spark-streaming)
- [Bulk Load](#bulk-load)
- [SparkSQL/DataFrames](#sparksqldataframes)

[Apache Spark](https://spark.apache.org/) 是一个用于以分布式方式处理内存中的数据, 并在许多用例中取代 MapReduce 的软件框架。

Spark和HBase之间的4个主要交互点:

- Basic Spark: 在 Spark DAG 中的任何点都可以与 HBase 建立连接。

- Spark Streaming: 在 Spark Streaming 应用程序的任何点上都可以有HBase连接。

- Spark Bulk Load: 直接写入 HBase HFiles 以便批量插入HBase的能力

- SparkSQL/DataFrames: 能够编写 SparkSQL 来绘制 HBase 中表示的表。

## Basic Spark

所有 Spark 和 HBase 集成的基础是 HBaseContext。 HBaseContext 接受 HBase 配置并将其推送到 Spark Executor。 我们可以在静态位置为每个 Spark Executor 建立一个 HBase 连接。

Spark Executor 可以位于与 Region Server 相同的节点上, 也可以位于不依赖于共存位置的不同节点上。可以将每个 Spark Executor 看作一个多线程 client 应用程序。 这允许在 Executor 上运行的任何 Spark Task 访问共享连接对象。

HBaseContext 用例:

```scala
val sc = new SparkContext("local", "test")
val config = new HBaseConfiguration()
...
val hbaseContext = new HBaseContext(sc, config)

rdd.hbaseForeachPartition(hbaseContext, (it, conn) => {
 val bufferedMutator = conn.getBufferedMutator(TableName.valueOf("t1"))
 it.foreach((putRecord) => {
. val put = new Put(putRecord._1)
. putRecord._2.foreach((putValue) => put.addColumn(putValue._1, putValue._2, putValue._3))
. bufferedMutator.mutate(put)
 })
 bufferedMutator.flush()
 bufferedMutator.close()
})
```

```java
JavaSparkContext jsc = new JavaSparkContext(sparkConf);

try {
    List<byte[]> list = new ArrayList<>[];
    list.add(Bytes.toBytes("xxx"));
    ...
    list.add(Bytes.toBytes("xxx"));

    JavaRDD<byte[]> rdd = jsc.parallelize(list);
    Configuration conf = HBaseConfiguration.create();

    JavaHBaseContext hbaseContext = new JavaHBaseContext(jsc, conf);

    hbaseContext.foreachPartition(rdd, new VoidFunction<Tuple2<Iterator<byte[]>, Connection>>(){
        public void call(Tuple2<iterator<byte[]>, Connection> t) throws Exception {
            Table table = t._2().getTable(TableName.valueOf("tableName"));
            while(t._1().hasNext()) {
                byte[] b = t._1().next();
                Result r = table.get(new Get(b));
                if (r.getExists()) {
                    mutator.mutate(new Put(b));
                }
            }

            mutator.flush();
            mutator.close();
            table.close();
        }
    });
} finally {
    jsc.stop();
}
```

Scala 和 Java 都支持 Spark 和 HBase 之间的所有功能, 但 SparkSQL 除外, 它支持 Spark 支持的任何语言。

上面的示例演示了如何使用连接执行 `foreachPartition`。许多其他 Spark base 功能是支持开箱即用:

- `bulkPut`: 用于将 put 大规模并行发送到 HBase。

- `bulkDelete`: 用于将 delete 大规模并行发送到 HBase。

- `mapPartition`: 使用连接对象执行Spark `map` 函数，以允许完全访问 HBase。

- `hBaseRDD`: 简化分布式 `scan` 以创建 RDD。

有关所有这些功能的示例, 请参见 hbase-spark 模块。

## Spark Streaming

Spark Streaming 是建立在 Spark 上的微批流处理框架。 HBase 和 Spark Streaming 可以协同可以提供以下好处:

- 动态获取参考数据或配置文件数据的位置。
- 存储计数或聚合的位置, 支持 Spark 的 Only once processing 的 流式处理。

用法和 Basic Spark 类似, `bulkPut` example:

```scala
val sc = new SparkContext("local", "test")
val config = new HBaseConfiguration()

val hbaseContext = new HBaseContext(sc, config)
val ssc = new StreamingContext(sc, Milliseconds(200))

val rdd1 = ...
val rdd2 = ...

val queue = mutable.Queue[RDD[Array[Byte], Array[(Array[Byte]), (Array[Byte]), (Array[Byte])]]]()

queue += rdd1
queue += rdd2

val dStream = ssc.queueStream(queue)

dStream.hbaseBulkPut(
    hbaseContext,
    TableName.valueOf(tableName),
    (putRecord) => {
        val put = new Put(putRecord._1)
        putRecord._2.foreach((putValue) => put.addColumn(putValue._1, putValue._2, putValue._3))
        put
    }
)
```

## Bulk Load

## SparkSQL/DataFrames
