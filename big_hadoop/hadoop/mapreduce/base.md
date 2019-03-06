# note

## MapReduce

1. mapper
2. reducer:
  > shuffle: 准备阶段, 获取 map 结果来执行 Reduce
  > partitioner
  > sort, second sort
  > reduce
3. container

http://blog.csdn.net/techchan/article/details/53405519

### Map

Map 数据来源为 HDFS 的 block。 只读取 split。

### shuffle

### partitioner

- 过程
  > 输入的 <key, value> 对经过的 map() 处理后输出新的 <key, value> 对, 首先会被存储到一个环形的缓冲区中(字节数组实现)。 并且会对每个<key, value> hash 一个 partition 值, 相同的 partition 值为同一个分区。

- 作用
  > 由于 map() 处理后的数据量可能会非常大, 

### combiner

- 作用
  1. 将一个 map 产生的多个结果合并成一个新的作为 reduce 的输入
  2. 在 map 与 reduce 中间来减少 map 输出的中间结果

## TODO note these docs in my own words

### mapper notice

1. `The number of maps is usually driven by the total size of the inputs, that is, the total number of blocks of the input files.

The right level of parallelism for maps seems to be around 10-100 maps per-node, although it has been set up to 300 maps for very cpu-light map tasks. Task setup takes a while, so it is best if the maps take at least a minute to execute.

Thus, if you expect 10TB of input data and have a blocksize of 128MB, you’ll end up with 82,000 maps, unless Configuration.set(MRJobConfig.NUM_MAPS, int) (which only provides a hint to the framework) is used to set it even higher.`-- 多少个 block, 多少个 maps

### reducer notice

1. `The right number of reduces seems to be 0.95 or 1.75 multiplied by (<no. of nodes> * <no. of maximum containers per node>).

With 0.95 all of the reduces can launch immediately and start transferring map outputs as the maps finish. With 1.75 the faster nodes will finish their first round of reduces and launch a second wave of reduces doing a much better job of load balancing.

Increasing the number of reduces increases the framework overhead, but increases load balancing and lowers the cost of failures.

The scaling factors above are slightly less than whole numbers to reserve a few reduce slots in the framework for speculative-tasks and failed tasks.`
