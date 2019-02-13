# tmp note

1. `ApplicationSubmissionContext#setUnmanagedAM(boolean)`, 设置是否不通过 Yarn 托管 ApplicationMaster。

2. 入口s:
   1. Standalone Job 集群 入口: `org.apache.flink.container.entrypoint.StandaloneJobClusterEntryPoint`。
   2. Standalone Session 集群 入口: `org.apache.flink.runtime.entrypoint.StandaloneSessionClusterEntrypoint`。
   3. History 入口: `org.apache.flink.runtime.webmonitor.history.HistoryServer`。
   4. TaskManager 入口: `org.apache.flink.runtime.taskexecutor.TaskManagerRunner`。
   5. 提交任务入口: `org.apache.flink.client.cli.CliFrontend`。
   6. Yarn Session(AM) 入口: `org.apache.flink.yarn.YarnApplicationMasterRunner`。
   7. Zookeeper 相关部分入口: `org.apache.flink.runtime.zookeeper.FlinkZooKeeperQuorumPeer`。
