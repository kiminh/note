# DistCp

- [描述](#描述)
- [用法列表](#用法列表)
  - [基本用法](#基本用法)
  - [update & Overwrite](#update--overwrite)

## 描述

[DistCp Version2 Guide](http://hadoop.apache.org/docs/stable/hadoop-distcp/DistCp.html)

`DistCp` 用于 集群间/集群内 的 大型复制工具。 使用 `MapReduce` 实现, 错误处理和恢复及告警。 将文件和目录的列表展开为 `map` 任务的输入。

## 用法列表

[Copying Cluster Data Using DistCp](https://www.cloudera.com/documentation/enterprise/5-12-x/topics/cdh_admin_distcp_data_cluster_migrate.html)

### 基本用法

- 最常用的调用

```bash
hadoop distcp hdfs://namenode1:8020/foo/bar hdfs://namenode2:8020/bar/foo
```

- 将 source 扩展到一个 临时文件中, 在一组 `MapReduce` 任务中对其进行分区, 并在每个 NodeManager 上进行复制。 可以指定多个路径

```bash
# eg1
hadoop distcp hdfs://namenode1:8020/foo/a hdfs://namenode1:/foo/2 hdfs://namenode2:8020/bar/foo

# eg2
hadoop distcp hdfs://namenode1:8020/foo/srclist hdfs://namenode2://namenode2:bar/foo
# /foo/srclist 包含 (/foo/a & /foo/b)
```

多个 source 进行复制时, 如果两个 source 发生冲突, DistCp 将使用错误消息终止复制, 在 target 的冲突可以根据指定的选项进行解决。 默认情况下, 将跳过 target 位置上已存在的文件, 不被 source 替换。 每个作业结束时报告跳过的文件数量, 如果某次复制的某个文件子集出现故障, 则结果可能是不准确的。

每个 `NodeManager` 都与 source 文件系统和 target 文件系统进行通信。 对于 HDFS, source 和 target 必须运行相同版本的协议活着使用向后兼容的协议。

复制后, 官方建议生成并交叉检查 source 和 target, 验证是否真正复制成功。 由于 DistCp 同时使用 Map/Reduce 和 文件系统 API, 所以在这三种 API 中的任何一种或两者之间的问题都可能会对复制产生负面的, silently 影响。 有些成功在本地启用 `-update` 执行第二次遍历的情况下运行, 但是在尝试第二次遍历前应熟悉其语义。

如果有客户端正在向 source 写文件, 复制可能会失败。 在 target HDFS 上的文件写入也会失败。 如果在复制文件前移动过, 抛出 `FileNotFouneException`。

### update & Overwrite

`-update` 将 target 中不存在或与 source 中版本不同的文件复制到 target。 `-overrite` 将覆盖 target 文件。
