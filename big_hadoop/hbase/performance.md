# HBase 调优

- [调优思路](#调优思路)
- [机器基本配置](#机器基本配置)
- [Rowkey 设计](#rowkey-设计)
  - [基本原则](#基本原则)
  - [Data Block Encoding and Compression](#data-block-encoding-and-compression)
- [HBase 参数调优](#hbase-参数调优)
- [HDFS 调优](#hdfs-调优)
- [JVM 调优](#jvm-调优)

## 调优思路

1. 客户端使用方式(eg: 批量 put 代替单条 put)
2. 业务角度优化(包括表设计(高标/宽表), **Rowkey 设计**, 压缩方式, 序列化方式)
3. HBase 参数优化
4. HDFS 优化
5. JVM 优化
6. Region 在 RegionServer 上**均匀分布**。
7. RegionServer 读取 HFile 尽可能读取本地的文件, 即 Region 分布在 HFile 的 3个 Replication 的其中一个上。

## 机器基本配置

关闭 swap, hugpage。 (原因 TODO)

## Rowkey 设计

### 基本原则

1. Rowkey 长度尽可能短。
2. Rowkey 尽可能包含检索的所有信息, 如果无法通过 Rowkey 查询, 查询将通过 Scan 完成。
3. 当 Region 到一定大小时, 会执行 Region 拆分, 可以通过建表时预拆分来防止由于 Region 拆分带来的查询效率下降。
4. 写 Hotspot, 会导致无法充分利用集群资源。 可以适当加盐(MD5 Hash 等)。(写多读少)
5. Rowkey 按照包含的每个要素的查询的查询的频率排列, 查询频率较高的部分放到 Rowkey 的前面, 查询频率较低的部分放到 Rowkey 的后面。
6. 当 IO 成为瓶颈时, 可以选用合适的 **编码方式** 或 **压缩算法** 将部分压力专业到 CPU。

### Data Block Encoding and Compression

- Encoding: long keys (compared to the values) or many columns, use a prefix encoder.

1. Prefix
2. Diff
3. Fast Diff
4. Prefix Tree

- Compression: none, Snappy, LZO, LZ4, GZ

> 如果 value 很大, 使用 可以使用相应的压缩算法来减少 value 的大小。

- 各个压缩算法的比较(TODO translate to Chinese)

1. Use GZIP for cold data, which is accessed infrequently. GZIP compression uses more CPU resources than Snappy or LZO, but provides a higher compression ratio.

2. Use Snappy or LZO for hot data, which is accessed frequently. Snappy and LZO use fewer CPU resources than GZIP, but do not provide as high of a compression ratio.

3. In most cases, enabling Snappy or LZO by default is a good choice, because they have a low performance overhead and provide space savings.

4. Before Snappy became available by Google in 2011, LZO was the default. Snappy has similar qualities as LZO but has been shown to perform better.

## HBase 参数调优

主要目的是让集群中所有 RegionServer 负载相对平均。

计算集群 Region 个数公式:

$$((RS Xmx)*hbase.regionserver.global.memstore.size)/(hbase.hregion.memstore.flush.size*(\# column fimilies))$$

## HDFS 调优

## JVM 调优
