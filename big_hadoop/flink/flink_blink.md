# Flink Blink 尝试

- [Flink Blink 关系](#flink-blink-关系)
- [编译](#编译)
- [安装配置](#安装配置)
  - [Standalone](#standalone)
  - [Yarn Cluster](#yarn-cluster)
- [基本 Demo](#基本-demo)

1.28 早上看到 Flink 的 Blink 分支推送到 Flink 仓库, 决定看看, 尝尝鲜。

## Flink Blink 关系

详细参考 [阿里正式向 Apache Flink 贡献 Blink 源码](https://mp.weixin.qq.com/s/6_4l4zo3-FD-NF3Lmr0BxQ)。

目前在 Flink 中存在一个单独的 `blink` 分支, 了解到以后会合并到主分支。

## 编译

```bash
git clone https://github.com/apache/flink.git
git checkout origin/blink --track

mvn clean install -DskipTests -Drat.skip=true -Denforcer.skip=true -Dcheckstyle.skip=true -Pvendor-repos,mapr -Dhadoop.version=2.7.3.2.5.0.0-1245
```

编译过程中, 可能出现一些检查 task 执行失败, 所以跳过这些检查, 如: rat plugin 这里的作用是检查有没有 AFS 协议签名, 需要在每个文件开头有 ASF LICENSE 签名。 这里选择跳过。

`-Dhadoop.version` 用来指定 Hadoop 版本, 可以选择 Apache Hadoop 以及 CDH, HDP, MapR 的发行版。 这里指定为 HDP-2.5.0.0-1245。

`-Pvendor-repos` 用来启动包含 Cloudera, Hortonworks 和 MapR 这些当前流行的 Hadoop 发行版本的 Maven build profile。

## 安装配置

### Standalone

执行 `start-cluster.sh` 脚本启动即可。

### Yarn Cluster

需要在环境变量中添加 Hadoop 相关的内容, 包含 Hadoop 的依赖, 配置文件。

通过 `./bin/yarn-session.sh`(默认在 Flink 服务根路径) 创建 Application Master。 Application Master 创建成功后, 可以通过 `./bin/yarn-session.sh` 来提交任务。 需要注意的是在 HDP 发行版中与 Apache Hadoop 不同的是: HDFS, Yarn 的配置文件与依赖分别在不同的路径, 这些路径最终需要在 `HADOOP_CONF_DIR` 中, 包含以下 export:

```bash
export HADOOP_HOME=/usr/hdp/current/hadoop-client
export HADOOP_HDFS_HOME=/usr/hdp/current/hadoop-hdfs-client
export HADOOP_YARN_HOME=/usr/hdp/current/hadoop-yarn-client
export HADOOP_CONF_DIR=$HADOOP_HOME/etc/hadoop/*:$HADOOP_HOME/*:$HADOOP_HOME/lib/*:$HADOOP_HOME/libexec/*:$HADOOP_HOME/libnew/*:$HADOOP_YARN_HOME/*:$HADOOP_YARN_HOME/lib/*:$HADOOP_YARN_HOME/etc/hadoop:$HADOOP_HDFS_HOME/*:$HADOOP_HDFS_HOME/*:$HADOOP_HDFS_HOME/lib/*
```

任务可以直接提交到 yarn, 也可以提交到已经启动的 Application Master 中。 通过如下方式提交:

```bash
# 提交到 Yarn
./bin/flink run -m yarn-cluster -yn 4 -yjm 1024 -ytm 4096 ./examples/streaming/WordCount.jar

# 提交到 Application Master
./bin/flink run -m <host>:<port> -yn 4 -yjm 1024 -ytm 4096 ./examples/streaming/WordCount.jar
```

具体操作请参考官方文档 [pache Flink Documentation](https://ci.apache.org/projects/flink/flink-docs-release-1.7/)

## 基本 Demo

编译完成后, 包含以下 examples:

```text
examples
├── batch
│   ├── ConnectedComponents.jar
│   ├── DistCp.jar
│   ├── EnumTriangles.jar
│   ├── KMeans.jar
│   ├── PageRank.jar
│   ├── TransitiveClosure.jar
│   ├── WebLogAnalysis.jar
│   └── WordCount.jar
├── gelly
│   └── flink-gelly-examples_2.11-1.5.1.jar
├── python
│   ├── batch
│   │   ├── __init__.py
│   │   ├── TPCHQuery10.py
│   │   ├── TPCHQuery3.py
│   │   ├── TriangleEnumeration.py
│   │   ├── WebLogAnalysis.py
│   │   └── WordCount.py
│   └── streaming
│       ├── fibonacci.py
│       └── word_count.py
├── streaming
│   ├── IncrementalLearning.jar
│   ├── Iteration.jar
│   ├── Kafka010Example.jar
│   ├── SessionWindowing.jar
│   ├── SocketWindowWordCount.jar
│   ├── StateMachineExample.jar
│   ├── TopSpeedWindowing.jar
│   ├── TPCHQuery.jar
│   ├── Twitter.jar
│   ├── WindowJoin.jar
│   └── WordCount.jar
└── table
    ├── Interactive.jar
    ├── StreamJoinSQLExample.jar
    ├── StreamSQLExample.jar
    ├── StreamTableExample.jar
    └── WordCountSQL.jar
```

详见 [github/flink-example](https://github.com/apache/flink/tree/blink/flink-examples)
