# HDFS 架构

[Hadoop分布式文件系统：架构和设计](http://hadoop.apache.org/docs/r1.0.4/cn/hdfs_design.html)

名词解释:

1. 硬件错误

2. 流式数据访问

3. 简单一致性模型

4. NameNode 和 DataNode

![架构图](../imgs/hdfsarchitecture.gif)

5. namespace

6. 数据复制

7. 副本存放

8. 安全模式

## 基本指令

- NameNode 指令

`hdfs namenode [options]`

options | 描述
---|---
-backup | 开始备份节点
-checkpoint | 开始检查节点
-format [-clusterid cid] [-force] [-nonInteractive] | 初始化namenode
-upgrade [clusterid cid] [-renameReserved \<k v pairs\>] | 升级 namenode 并通过升级选项启动
-upgradeOnly | 仅升级, 不启动
-rollback | 将 namenode 回滚到以前的版本, 应该在停止就去那并分发就的Hadoop版本后使用
-rollingUpgrade [downgrade&#124;roolback&#124;started] |
-finalize | Finalize将删除文件系统的以前的状态。最近的升级将成为永久的。回滚选项将不再可用。完成后关闭NameNode。
-importCheckpoint |
-initializeSharedEdits |
-bootstrapStandby | 允许通过复制活动NameNode中的最新命名空间快照来引导备用NameNode的存储目录。这在首次配置HA群集时使用。
-recover [-force] | 恢复模式的特殊NameNode启动模式, 可以恢复大部分数据
-metadataVersion | 验证配置的目录是否存在, 然后打印 software 和 image 的元数据版本
