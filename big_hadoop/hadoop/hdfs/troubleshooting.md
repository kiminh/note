1. 元数据问题

```log
2017-12-04 16:45:16,480 INFO  namenode.RedundantEditLogInputStream (RedundantEditLogInputStream.java:nextOp(177)) - Fast-forwarding stream 'http://xxx.xxx.xxx.xxx:8480/getJournal?jid=mycluster&segmentTxId=37516&storageInfo=-63%3A1386923016%3A0%3ACID-90eabf92-f5d6-4e3a-acc1-afdfc7e8c335' to transaction ID 31561
2017-12-04 16:45:16,510 WARN  namenode.FSNamesystem (FSNamesystem.java:loadFromDisk(692)) - Encountered exception loading fsimage
java.io.IOException: There appears to be a gap in the edit log.  We expected txid 31813, but got txid 37516.
        at org.apache.hadoop.hdfs.server.namenode.MetaRecoveryContext.editLogLoaderPrompt(MetaRecoveryContext.java:94)
        at org.apache.hadoop.hdfs.server.namenode.FSEditLogLoader.loadEditRecords(FSEditLogLoader.java:215)
        at org.apache.hadoop.hdfs.server.namenode.FSEditLogLoader.loadFSEdits(FSEditLogLoader.java:143)
        at org.apache.hadoop.hdfs.server.namenode.FSImage.loadEdits(FSImage.java:843)
        at org.apache.hadoop.hdfs.server.namenode.FSImage.loadFSImage(FSImage.java:698)
        at org.apache.hadoop.hdfs.server.namenode.FSImage.recoverTransitionRead(FSImage.java:294)
        at org.apache.hadoop.hdfs.server.namenode.FSNamesystem.loadFSImage(FSNamesystem.java:1015)
        at org.apache.hadoop.hdfs.server.namenode.FSNamesystem.loadFromDisk(FSNamesystem.java:690)
        at org.apache.hadoop.hdfs.server.namenode.NameNode.loadNamesystem(NameNode.java:688)
        at org.apache.hadoop.hdfs.server.namenode.NameNode.initialize(NameNode.java:752)
        at org.apache.hadoop.hdfs.server.namenode.NameNode.<init>(NameNode.java:992)
        at org.apache.hadoop.hdfs.server.namenode.NameNode.<init>(NameNode.java:976)
        at org.apache.hadoop.hdfs.server.namenode.NameNode.createNameNode(NameNode.java:1686)
        at org.apache.hadoop.hdfs.server.namenode.NameNode.main(NameNode.java:1754)
2017-12-04 16:45:16,513 INFO  mortbay.log (Slf4jLog.java:info(67)) - Stopped HttpServer2$SelectChannelConnectorWithSafeStartup@ds14.spacestar.com:50070
2017-12-04 16:45:16,514 INFO  impl.MetricsSystemImpl (MetricsSystemImpl.java:stop(211)) - Stopping NameNode metrics system...
2017-12-04 16:45:16,515 INFO  impl.MetricsSinkAdapter (MetricsSinkAdapter.java:publishMetricsFromQueue(141)) - timeline thread interrupted.
2017-12-04 16:45:16,516 INFO  impl.MetricsSystemImpl (MetricsSystemImpl.java:stop(217)) - NameNode metrics system stopped.
2017-12-04 16:45:16,516 INFO  impl.MetricsSystemImpl (MetricsSystemImpl.java:shutdown(606)) - NameNode metrics system shutdown complete.
2017-12-04 16:45:16,516 ERROR namenode.NameNode (NameNode.java:main(1759)) - Failed to start namenode.
java.io.IOException: There appears to be a gap in the edit log.  We expected txid 31813, but got txid 37516.
        at org.apache.hadoop.hdfs.server.namenode.MetaRecoveryContext.editLogLoaderPrompt(MetaRecoveryContext.java:94)
        at org.apache.hadoop.hdfs.server.namenode.FSEditLogLoader.loadEditRecords(FSEditLogLoader.java:215)
        at org.apache.hadoop.hdfs.server.namenode.FSEditLogLoader.loadFSEdits(FSEditLogLoader.java:143)
        at org.apache.hadoop.hdfs.server.namenode.FSImage.loadEdits(FSImage.java:843)
        at org.apache.hadoop.hdfs.server.namenode.FSImage.loadFSImage(FSImage.java:698)
        at org.apache.hadoop.hdfs.server.namenode.FSImage.recoverTransitionRead(FSImage.java:294)
        at org.apache.hadoop.hdfs.server.namenode.FSNamesystem.loadFSImage(FSNamesystem.java:1015)
        at org.apache.hadoop.hdfs.server.namenode.FSNamesystem.loadFromDisk(FSNamesystem.java:690)
        at org.apache.hadoop.hdfs.server.namenode.NameNode.loadNamesystem(NameNode.java:688)
        at org.apache.hadoop.hdfs.server.namenode.NameNode.initialize(NameNode.java:752)
        at org.apache.hadoop.hdfs.server.namenode.NameNode.<init>(NameNode.java:992)
        at org.apache.hadoop.hdfs.server.namenode.NameNode.<init>(NameNode.java:976)
        at org.apache.hadoop.hdfs.server.namenode.NameNode.createNameNode(NameNode.java:1686)
        at org.apache.hadoop.hdfs.server.namenode.NameNode.main(NameNode.java:1754)
2017-12-04 16:45:16,518 INFO  util.ExitUtil (ExitUtil.java:terminate(124)) - Exiting with status 1
2017-12-04 16:45:16,518 INFO  timeline.HadoopTimelineMetricsSink (HadoopTimelineMetricsSink.java:run(416)) - Closing HadoopTimelineMetricSink. Flushing metrics to collector...
2017-12-04 16:45:16,539 INFO  namenode.NameNode (LogAdapter.java:info(47)) - SHUTDOWN_MSG:
/************************************************************
SHUTDOWN_MSG: Shutting down NameNode at ds14.spacestar.com/172.31.1.14
************************************************************/
```

```
Backup node: An extension to the Checkpoint node. In addition to checkpointing it also receives a stream of edits from the NameNode and maintains its own in-memory copy of the namespace, which is always in sync with the active NameNode namespace state. Only one Backup node may be registered with the NameNode at once.
```

  - 解决思路1:

```bash
# 格式化新的共享编辑目录并复制足够的编辑日志段, 以便备用NameNode可以启动。
hdfs namenode -initializeSharedEdits
```

  - 解决思路2:
  > 备份 standby namenode 的元数据, 重新同步 standby namenode 的元数据为 active namenode 的元数据, 使用如下指令
  > `hdfs namenode -bootstrapStandby -force` 强制重新初始化 standby namenode。

  - 解决思路3:
  > 手动 copy namenode（active）所在的那台服务器上 `/XXX/dfs/name/current/` 下的所有文件到 namenode（standby）所在的那台服务器的对应目录下
