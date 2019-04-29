# trubleshooting

hbase 详解
http://blog.csdn.net/ldds_520/article/details/51648833

```log
09:32:23.020 [main-SendThread()] INFO org.apache.zookeeper.ClientCnxn - Opening socket connection to server /172.20.7.6:2181
09:32:23.020 [main] INFO org.apache.hadoop.hbase.zookeeper.RecoverableZooKeeper - Process identifier=hconnection-0x301abf87 connecting to ZooKeeper ensemble=172.20.7.6:2181
09:32:23.028 [main-SendThread(cloudera2.pjk-tvs...:2181)] INFO org.apache.zookeeper.ClientCnxn - Socket connection established to cloudera2.pjk-tvs.../172.20.7.6:2181, initiating session
09:32:23.029 [main-SendThread(cloudera2.pjk-tvs...net:2181)] DEBUG org.apache.zookeeper.ClientCnxn - Session establishment request sent on cloudera2.pjk-tvs.../172.20.7.6:2181
09:32:23.035 [main-SendThread(cloudera2.pjk-tvs...:2181)] INFO org.apache.zookeeper.ClientCnxn - Session establishment complete on server cloudera2.pjk-tvs.../172.20.7.6:2181, sessionid = 0x2423aeb0f790ff2, negotiated timeout = 60000
09:32:23.037 [main-EventThread] DEBUG org.apache.hadoop.hbase.zookeeper.ZooKeeperWatcher - hconnection-0x301abf87 Received ZooKeeper Event, type=None, state=SyncConnected, path=null
09:32:23.040 [main-EventThread] DEBUG org.apache.hadoop.hbase.zookeeper.ZooKeeperWatcher - hconnection-0x301abf87-0x2423aeb0f790ff2 connected
09:32:23.041 [main-SendThread(cloudera2.pjk-tvs...:2181)] DEBUG org.apache.zookeeper.ClientCnxn - Reading reply sessionid:0x2423aeb0f790ff2, packet:: clientPath:null serverPath:null finished:false header:: 1,3 replyHeader:: 1,8589982838,0 request:: '/hbase/hbaseid,F response:: s{16,8589934615,1383919470521,1383967999584,4,0,0,0,85,0,16}
09:32:23.044 [main-SendThread(cloudera2....:2181)] DEBUG org.apache.zookeeper.ClientCnxn - Reading reply sessionid:0x2423aeb0f790ff2, packet:: clientPath:null serverPath:null finished:false header:: 2,4 replyHeader:: 2,8589982838,0 request:: '/hbase/hbaseid,F response:: ffffffff0002c3131363140636c6f7564657261312e706a6b2d7476732e633467642e6772696464796e616d6963732e6e657465633932386436332d316334652d346336652d616238392d646237343239646231336638,s{16,8589934615,1383919470521,1383967999584,4,0,0,0,85,0,16}
09:32:23.100 [main] DEBUG org.apache.hadoop.ipc.RpcClient - Codec=org.apache.hadoop.hbase.codec.KeyValueCodec@11fb24d3, compressor=null, tcpKeepAlive=true, tcpNoDelay=true, maxIdleTime=10000, maxRetries=0, fallbackAllowed=false, ping interval=60000ms, bind address=null
09:32:23.109 [main-SendThread(cloudera2.pjk-tvs...:2181)] DEBUG org.apache.zookeeper.ClientCnxn - Reading reply sessionid:0x2423aeb0f790ff2, packet:: clientPath:null serverPath:null finished:false header:: 3,4 replyHeader:: 3,8589982838,-101 request:: '/hbase/meta-region-server,F response::
09:32:23.116 [main] DEBUG org.apache.hadoop.hbase.zookeeper.ZKUtil - hconnection-0x301abf87-0x2423aeb0f790ff2 Unable to get data of znode /hbase/meta-region-server because node does not exist (not an error)
09:32:23.317 [main-SendThread(cloudera2.pjk-tvs...:2181)] DEBUG org.apache.zookeeper.ClientCnxn - Reading reply sessionid:0x2423aeb0f790ff2, packet:: clientPath:null serverPath:null finished:false header:: 4,4 replyHeader:: 4,8589982838,-101 request:: '/hbase/meta-region-server,F response::
09:32:23.318 [main] DEBUG org.apache.hadoop.hbase.zookeeper.ZKUtil - hconnection-0x301abf87-0x2423aeb0f790ff2 Unable to get data of znode /hbase/meta-region-server because node does not exist (not an error)
```

```log
ERROR: org.apache.hadoop.hbase.PleaseHoldException: Master is initializing
        at org.apache.hadoop.hbase.master.HMaster.checkInitialized(HMaster.java:2402)
        at org.apache.hadoop.hbase.master.MasterRpcServices.getTableNames(MasterRpcServices.java:901)
        at org.apache.hadoop.hbase.protobuf.generated.MasterProtos$MasterService$2.callBlockingMethod(MasterProtos.java:57172)
        at org.apache.hadoop.hbase.ipc.RpcServer.call(RpcServer.java:2127)
        at org.apache.hadoop.hbase.ipc.CallRunner.run(CallRunner.java:107)
        at org.apache.hadoop.hbase.ipc.RpcExecutor.consumerLoop(RpcExecutor.java:133)
        at org.apache.hadoop.hbase.ipc.RpcExecutor$1.run(RpcExecutor.java:108)
        at java.lang.Thread.run(Thread.java:745)

Here is some help for this command:
List all tables in hbase. Optional regular expression parameter could
be used to filter the output. Examples:

  hbase> list
  hbase> list 'abc.*'
  hbase> list 'ns:abc.*'
  hbase> list 'ns:.*'

TABLE

ERROR:

Here is some help for this command:
List all tables in hbase. Optional regular expression parameter could
be used to filter the output. Examples:

  hbase> list
  hbase> list 'abc.*'
  hbase> list 'ns:abc.*'
  hbase> list 'ns:.*'

```

描述：“baseZNode=/hbase-unsecure Unable to get data of znode /hbase-unsecure/meta-region-server because node does not exist (not an error)”

```log
request:: '/hbase-unsecure,F  response:: s{4294967360,4294967360,1493888683458,1493888683458,0,153,0,0,0,17,111669150271}
   819 2017-08-31 11:03:04,981 DEBUG [main-SendThread(ab-03:2181)] zookeeper.ClientCnxn: Reading reply sessionid:0x35e362be55b000e, packet:: clientPath:null serverPath:null finished:false header:: 261,4  replyHeader:: 261,111669150273,-10
       1  request:: '/hbase-unsecure/meta-region-server,F  response::
   820 2017-08-31 11:03:04,981 DEBUG [ab-05:16000.activeMasterManager] zookeeper.ZKUtil: master:16000-0x35e362be55b000e, quorum=ab-01:2181,ab-02:2181,ab-03:2181, baseZNode=/hbase-unsecure Unable to get data of znode /hbase-unsecure/meta-r
       egion-server because node does not exist (not an error)
```

参考资料：
https://stackoverflow.com/questions/19903622/hbase-hbase-meta-region-server-node-does-not-exist
https://issues.apache.org/jira/browse/HBASE-8560
https://stackoverflow.com/questions/34969270/hbase-meta-region-server-because-node-does-not-exist-not-an-error
http://blog.csdn.net/liuxiao723846/article/details/53146304

### 问题2

```log
2017-09-11 20:38:10,411 INFO  [B.priority.fifo.QRpcServer.handler=1,queue=1,port=16000] master.ServerManager: Registering server=ab-01,16020,1505133487628
2017-09-11 20:38:10,422 INFO  [ab-05:16000.activeMasterManager] master.ServerManager: Waiting for region servers count to settle; currently checked in 1, slept for 5517 ms, expecting minimum of 1, maximum of 2147483647, timeout of 4500 ms
, interval of 1500 ms.
2017-09-11 20:38:10,539 INFO  [B.priority.fifo.QRpcServer.handler=3,queue=1,port=16000] master.ServerManager: Registering server=ab-04,16020,1505133487478
2017-09-11 20:38:10,572 INFO  [ab-05:16000.activeMasterManager] master.ServerManager: Waiting for region servers count to settle; currently checked in 2, slept for 5667 ms, expecting minimum of 1, maximum of 2147483647, timeout of 4500 ms
, interval of 1500 ms.
2017-09-11 20:38:10,849 ERROR [B.priority.fifo.QRpcServer.handler=0,queue=0,port=16000] master.MasterRpcServices: Region server ab-01,16020,1505133487628 reported a fatal error:
ABORTING region server ab-01,16020,1505133487628: Unhandled: cannot get log writer
Cause:
java.io.IOException: cannot get log writer
        at org.apache.hadoop.hbase.wal.DefaultWALProvider.createWriter(DefaultWALProvider.java:378)
        at org.apache.hadoop.hbase.regionserver.wal.FSHLog.createWriterInstance(FSHLog.java:765)
        at org.apache.hadoop.hbase.regionserver.wal.FSHLog.rollWriter(FSHLog.java:730)
        at org.apache.hadoop.hbase.regionserver.wal.FSHLog.rollWriter(FSHLog.java:640)
        at org.apache.hadoop.hbase.regionserver.wal.FSHLog.<init>(FSHLog.java:573)
        at org.apache.hadoop.hbase.wal.DefaultWALProvider.init(DefaultWALProvider.java:97)
        at org.apache.hadoop.hbase.wal.WALFactory.getProvider(WALFactory.java:148)
        at org.apache.hadoop.hbase.wal.WALFactory.<init>(WALFactory.java:180)
        at org.apache.hadoop.hbase.regionserver.HRegionServer.setupWALAndReplication(HRegionServer.java:1648)
        at org.apache.hadoop.hbase.regionserver.HRegionServer.handleReportForDutyResponse(HRegionServer.java:1381)
        at org.apache.hadoop.hbase.regionserver.HRegionServer.run(HRegionServer.java:917)
        at java.lang.Thread.run(Thread.java:745)
Caused by: java.io.IOException: Key provider org.apache.hadoop.hbase.io.crypto.KeyStoreKeyProvider failed test: java.lang.RuntimeException: java.io.FileNotFoundException: /local/hbase/conf/hbase.jks (No such file or directory)
        at org.apache.hadoop.hbase.util.EncryptionTest.testKeyProvider(EncryptionTest.java:68)
        at org.apache.hadoop.hbase.regionserver.wal.SecureProtobufLogWriter.buildWALHeader(SecureProtobufLogWriter.java:53)
        at org.apache.hadoop.hbase.regionserver.wal.ProtobufLogWriter.init(ProtobufLogWriter.java:94)
        at org.apache.hadoop.hbase.wal.DefaultWALProvider.createWriter(DefaultWALProvider.java:367)
        ... 11 more
Caused by: java.lang.RuntimeException: java.lang.RuntimeException: java.io.FileNotFoundException: /local/hbase/conf/hbase.jks (No such file or directory)
        at org.apache.hadoop.hbase.io.crypto.Encryption.getKeyProvider(Encryption.java:560)
        at org.apache.hadoop.hbase.util.EncryptionTest.testKeyProvider(EncryptionTest.java:64)
        ... 14 more
Caused by: java.lang.RuntimeException: java.io.FileNotFoundException: /local/hbase/conf/hbase.jks (No such file or directory)
        at org.apache.hadoop.hbase.io.crypto.KeyStoreKeyProvider.init(KeyStoreKeyProvider.java:153)
        at org.apache.hadoop.hbase.io.crypto.Encryption.getKeyProvider(Encryption.java:553)
        ... 15 more
Caused by: java.io.FileNotFoundException: /local/hbase/conf/hbase.jks (No such file or directory)
        at java.io.FileInputStream.open(Native Method)
        at java.io.FileInputStream.<init>(FileInputStream.java:146)
        at org.apache.hadoop.hbase.io.crypto.KeyStoreKeyProvider.load(KeyStoreKeyProvider.java:124)
        at org.apache.hadoop.hbase.io.crypto.KeyStoreKeyProvider.init(KeyStoreKeyProvider.java:147)
        ... 16 more

2017-09-11 20:38:11,025 ERROR [B.priority.fifo.QRpcServer.handler=2,queue=0,port=16000] master.MasterRpcServices: Region server ab-04,16020,1505133487478 reported a fatal error:
ABORTING region server ab-04,16020,1505133487478: Unhandled: cannot get log writer
Cause:
java.io.IOException: cannot get log writer
        at org.apache.hadoop.hbase.wal.DefaultWALProvider.createWriter(DefaultWALProvider.java:378)
        at org.apache.hadoop.hbase.regionserver.wal.FSHLog.createWriterInstance(FSHLog.java:765)
        at org.apache.hadoop.hbase.regionserver.wal.FSHLog.rollWriter(FSHLog.java:730)
        at org.apache.hadoop.hbase.regionserver.wal.FSHLog.rollWriter(FSHLog.java:640)
        at org.apache.hadoop.hbase.regionserver.wal.FSHLog.<init>(FSHLog.java:573)
        at org.apache.hadoop.hbase.wal.DefaultWALProvider.init(DefaultWALProvider.java:97)
        at org.apache.hadoop.hbase.wal.WALFactory.getProvider(WALFactory.java:148)
        at org.apache.hadoop.hbase.wal.WALFactory.<init>(WALFactory.java:180)
        at org.apache.hadoop.hbase.regionserver.HRegionServer.setupWALAndReplication(HRegionServer.java:1648)
        at org.apache.hadoop.hbase.regionserver.HRegionServer.handleReportForDutyResponse(HRegionServer.java:1381)
        at org.apache.hadoop.hbase.regionserver.HRegionServer.run(HRegionServer.java:917)
        at java.lang.Thread.run(Thread.java:745)
Caused by: java.io.IOException: Key provider org.apache.hadoop.hbase.io.crypto.KeyStoreKeyProvider failed test: java.lang.RuntimeException: java.io.FileNotFoundException: /local/hbase/conf/hbase.jks (No such file or directory)
        at org.apache.hadoop.hbase.util.EncryptionTest.testKeyProvider(EncryptionTest.java:68)
        at org.apache.hadoop.hbase.regionserver.wal.SecureProtobufLogWriter.buildWALHeader(SecureProtobufLogWriter.java:53)
        at org.apache.hadoop.hbase.regionserver.wal.ProtobufLogWriter.init(ProtobufLogWriter.java:94)
        at org.apache.hadoop.hbase.wal.DefaultWALProvider.createWriter(DefaultWALProvider.java:367)
        ... 11 more
Caused by: java.lang.RuntimeException: java.lang.RuntimeException: java.io.FileNotFoundException: /local/hbase/conf/hbase.jks (No such file or directory)
        at org.apache.hadoop.hbase.io.crypto.Encryption.getKeyProvider(Encryption.java:560)
        at org.apache.hadoop.hbase.util.EncryptionTest.testKeyProvider(EncryptionTest.java:64)
        ... 14 more
Caused by: java.lang.RuntimeException: java.io.FileNotFoundException: /local/hbase/conf/hbase.jks (No such file or directory)
        at org.apache.hadoop.hbase.io.crypto.KeyStoreKeyProvider.init(KeyStoreKeyProvider.java:153)
        at org.apache.hadoop.hbase.io.crypto.Encryption.getKeyProvider(Encryption.java:553)
        ... 15 more
Caused by: java.io.FileNotFoundException: /local/hbase/conf/hbase.jks (No such file or directory)
        at java.io.FileInputStream.open(Native Method)
        at java.io.FileInputStream.<init>(FileInputStream.java:146)
        at org.apache.hadoop.hbase.io.crypto.KeyStoreKeyProvider.load(KeyStoreKeyProvider.java:124)
        at org.apache.hadoop.hbase.io.crypto.KeyStoreKeyProvider.init(KeyStoreKeyProvider.java:147)
        ... 16 more

2017-09-11 20:38:11,028 INFO  [main-EventThread] zookeeper.RegionServerTracker: RegionServer ephemeral node deleted, processing expiration [ab-01,16020,1505133487628]
2017-09-11 20:38:11,028 INFO  [main-EventThread] master.ServerManager: Master doesn't enable ServerShutdownHandler during initialization, delay expiring server ab-01,16020,1505133487628
2017-09-11 20:38:11,119 INFO  [main-EventThread] zookeeper.RegionServerTracker: RegionServer ephemeral node deleted, processing expiration [ab-04,16020,1505133487478]
2017-09-11 20:38:11,119 INFO  [main-EventThread] master.ServerManager: Master doesn't enable ServerShutdownHandler during initialization, delay expiring server ab-04,16020,1505133487478
2017-09-11 20:38:12,077 INFO  [ab-05:16000.activeMasterManager] master.ServerManager: Finished waiting for region servers count to settle; checked in 2, slept for 7172 ms, expecting minimum of 1, maximum of 2147483647, master is running  
2017-09-11 20:38:12,100 INFO  [ab-05:16000.activeMasterManager] master.MasterFileSystem: Log folder hdfs://ab-01:8020/apps/hbase/data/WALs/ab-01,16020,1504839824373 doesn't belong to a known region server, splitting
2017-09-11 20:38:12,102 INFO  [ab-05:16000.activeMasterManager] master.MasterFileSystem: Log folder hdfs://ab-01:8020/apps/hbase/data/WALs/ab-01,16020,1504839940862 doesn't belong to a known region server, splitting
```

https://community.hortonworks.com/questions/18637/i-am-not-getting-hive-shell-prompt-it-is-usual-pro.html

手工模拟local/hbase/conf/hbase.jks文件； 处理filenotfound异常后出现：

```log
2017-09-11 21:19:46,846 INFO  [ab-05:16000.activeMasterManager] zookeeper.MetaTableLocator: Failed verification of hbase:meta,,1 at address=ab-01,16020,1505135874716, exception=org.apache.hadoop.hbase.NotServingRegionException: Region hbase:meta,,1 is not online on ab-01,16020,1505135982539
        at org.apache.hadoop.hbase.regionserver.HRegionServer.getRegionByEncodedName(HRegionServer.java:2928)
        at org.apache.hadoop.hbase.regionserver.RSRpcServices.getRegion(RSRpcServices.java:974)
        at org.apache.hadoop.hbase.regionserver.RSRpcServices.getRegionInfo(RSRpcServices.java:1254)
        at org.apache.hadoop.hbase.protobuf.generated.AdminProtos$AdminService$2.callBlockingMethod(AdminProtos.java:22731)
        at org.apache.hadoop.hbase.ipc.RpcServer.call(RpcServer.java:2127)
        at org.apache.hadoop.hbase.ipc.CallRunner.run(CallRunner.java:107)
        at org.apache.hadoop.hbase.ipc.RpcExecutor.consumerLoop(RpcExecutor.java:133)
        at org.apache.hadoop.hbase.ipc.RpcExecutor$1.run(RpcExecutor.java:108)
        at java.lang.Thread.run(Thread.java:745)

```

hbck hbase, 修复 hbase meta 表报错

```log
2017-09-12 11:09:00,306 INFO  [main] zookeeper.ZooKeeper: Initiating client connection, connectString=ab-01:2181,ab-02:2181,ab-03:2181 sessionTimeout=90000 watcher=org.apache.hadoop.hbase.zookeeper.PendingWatcher@74aaadcc
2017-09-12 11:09:00,322 INFO  [main-SendThread(ab-03:2181)] zookeeper.ClientCnxn: Opening socket connection to server ab-03/192.168.1.62:2181. Will not attempt to authenticate using SASL (unknown error)
2017-09-12 11:09:00,326 INFO  [main-SendThread(ab-03:2181)] zookeeper.ClientCnxn: Socket connection established to ab-03/192.168.1.62:2181, initiating session
2017-09-12 11:09:00,332 INFO  [main-SendThread(ab-03:2181)] zookeeper.ClientCnxn: Session establishment complete on server ab-03/192.168.1.62:2181, sessionid = 0x35e73c2bad80012, negotiated timeout = 40000
^C2017-09-12 11:09:01,945 INFO  [Thread-9] client.ConnectionManager$HConnectionImplementation: Closing master protocol: MasterService
2017-09-12 11:09:01,945 INFO  [Thread-9] client.ConnectionManager$HConnectionImplementation: Closing zookeeper sessionid=0x35e73c2bad80012
```

```log
2017-09-12 12:51:07,874 - WARN  [NIOServerCxn.Factory:0.0.0.0/0.0.0.0:2181:NIOServerCnxn@357] - caught end of stream exception
EndOfStreamException: Unable to read additional data from client sessionid 0x15e746caf22000f, likely client has closed socket
        at org.apache.zookeeper.server.NIOServerCnxn.doIO(NIOServerCnxn.java:228)
        at org.apache.zookeeper.server.NIOServerCnxnFactory.run(NIOServerCnxnFactory.java:208)
        at java.lang.Thread.run(Thread.java:745)
```

```log
017-09-13 18:00:44,385 INFO  [LruBlockCacheStatsExecutor] hfile.LruBlockCache: totalSize=417.35 KB, freeSize=395.89 MB, max=396.30 MB, blockCount=0, accesses=0, hits=0, hitRatio=0, cachingAccesses=0, cachingHits=0, cachingHitsRatio=0,evictions=9059, evicted=0, evictedPerRun=0.0
2017-09-13 18:00:46,690 INFO  [ab-05,16000,1505206244279_ChoreService_1] zookeeper.RecoverableZooKeeper: Process identifier=hconnection-0x2a235422 connecting to ZooKeeper ensemble=ab-01:2181,ab-02:2181,ab-03:2181
2017-09-13 18:00:46,690 INFO  [ab-05,16000,1505206244279_ChoreService_1] zookeeper.ZooKeeper: Initiating client connection, connectString=ab-01:2181,ab-02:2181,ab-03:2181 sessionTimeout=90000 watcher=org.apache.hadoop.hbase.zookeeper.PendingWatcher@756899ca
2017-09-13 18:00:46,691 INFO  [ab-05,16000,1505206244279_ChoreService_1-SendThread(ab-03:2181)] zookeeper.ClientCnxn: Opening socket connection to server ab-03/192.168.1.62:2181. Will not attempt to authenticate using SASL (unknown error)2017-09-13 18:00:46,692 INFO  [ab-05,16000,1505206244279_ChoreService_1-SendThread(ab-03:2181)] zookeeper.ClientCnxn: Socket connection established to ab-03/192.168.1.62:2181, initiating session
2017-09-13 18:00:46,695 INFO  [ab-05,16000,1505206244279_ChoreService_1-SendThread(ab-03:2181)] zookeeper.ClientCnxn: Session establishment complete on server ab-03/192.168.1.62:2181, sessionid = 0x35e746cc04a008d, negotiated timeout = 40000
2017-09-13 18:00:46,706 INFO  [ab-05,16000,1505206244279_ChoreService_1] client.ConnectionManager$HConnectionImplementation: Closing zookeeper sessionid=0x35e746cc04a008d
2017-09-13 18:00:46,707 INFO  [ab-05,16000,1505206244279_ChoreService_1] zookeeper.ZooKeeper: Session: 0x35e746cc04a008d closed
2017-09-13 18:00:46,707 INFO  [ab-05,16000,1505206244279_ChoreService_1-EventThread] zookeeper.ClientCnxn: EventThread shut down
```
hbase 中这段日志为正常情况。

- 正常后

```log
2017-09-12 19:05:44,385 INFO  [LruBlockCacheStatsExecutor] hfile.LruBlockCache: totalSize=417.35 KB, freeSize=395.89 MB, max=396.30 MB, blockCount=0, accesses=0, hits=0, hitRatio=0, cachingAccesses=0, cachingHits=0, cachingHitsRatio=0,evictions=809, evicted=0, evictedPerRun=0.0
2017-09-12 19:06:16,865 INFO  [WALProcedureStoreSyncThread] wal.WALProcedureStore: Remove log: hdfs://ab-01:8020/apps/hbase/data/MasterProcWALs/state-00000000000000000973.log
2017-09-12 19:06:16,865 INFO  [WALProcedureStoreSyncThread] wal.WALProcedureStore: Removed logs: [hdfs://ab-01:8020/apps/hbase/data/MasterProcWALs/state-00000000000000000974.log]
2017-09-12 19:10:44,385 INFO  [LruBlockCacheStatsExecutor] hfile.LruBlockCache: totalSize=417.35 KB, freeSize=395.89 MB, max=396.30 MB, blockCount=0, accesses=0, hits=0, hitRatio=0, cachingAccesses=0, cachingHits=0, cachingHitsRatio=0,evictions=839, evicted=0, evictedPerRun=0.0
2017-09-12 19:15:44,385 INFO  [LruBlockCacheStatsExecutor] hfile.LruBlockCache: totalSize=417.35 KB, freeSize=395.89 MB, max=396.30 MB, blockCount=0, accesses=0, hits=0, hitRatio=0, cachingAccesses=0, cachingHits=0, cachingHitsRatio=0,evictions=869, evicted=0, evictedPerRun=0.0
2017-09-12 19:20:44,385 INFO  [LruBlockCacheStatsExecutor] hfile.LruBlockCache: totalSize=417.35 KB, freeSize=395.89 MB, max=396.30 MB, blockCount=0, accesses=0, hits=0, hitRatio=0, cachingAccesses=0, cachingHits=0, cachingHitsRatio=0,evictions=899, evicted=0, evictedPerRun=0.0
2017-09-12 19:25:44,385 INFO  [LruBlockCacheStatsExecutor] hfile.LruBlockCache: totalSize=417.35 KB, freeSize=395.89 MB, max=396.30 MB, blockCount=0, accesses=0, hits=0, hitRatio=0, cachingAccesses=0, cachingHits=0, cachingHitsRatio=0,evictions=929, evicted=0, evictedPerRun=0.0
2017-09-12 19:30:44,385 INFO  [LruBlockCacheStatsExecutor] hfile.LruBlockCache: totalSize=417.35 KB, freeSize=395.89 MB, max=396.30 MB, blockCount=0, accesses=0, hits=0, hitRatio=0, cachingAccesses=0, cachingHits=0, cachingHitsRatio=0,evictions=959, evicted=0, evictedPerRun=0.0
2017-09-12 19:35:44,385 INFO  [LruBlockCacheStatsExecutor] hfile.LruBlockCache: totalSize=417.35 KB, freeSize=395.89 MB, max=396.30 MB, blockCount=0, accesses=0, hits=0, hitRatio=0, cachingAccesses=0, cachingHits=0, cachingHitsRatio=0,evictions=989, evicted=0, evictedPerRun=0.0
2017-09-12 19:40:44,385 INFO  [LruBlockCacheStatsExecutor] hfile.LruBlockCache: totalSize=417.35 KB, freeSize=395.89 MB, max=396.30 MB, blockCount=0, accesses=0, hits=0, hitRatio=0, cachingAccesses=0, cachingHits=0, cachingHitsRatio=0,evictions=1019, evicted=0, evictedPerRun=0.0
2017-09-12 19:45:44,385 INFO  [LruBlockCacheStatsExecutor] hfile.LruBlockCache: totalSize=417.35 KB, freeSize=395.89 MB, max=396.30 MB, blockCount=0, accesses=0, hits=0, hitRatio=0, cachingAccesses=0, cachingHits=0, cachingHitsRatio=0,evictions=1049, evicted=0, evictedPerRun=0.0
2017-09-12 19:50:44,385 INFO  [LruBlockCacheStatsExecutor] hfile.LruBlockCache: totalSize=417.35 KB, freeSize=395.89 MB, max=396.30 MB, blockCount=0, accesses=0, hits=0, hitRatio=0, cachingAccesses=0, cachingHits=0, cachingHitsRatio=0,evictions=1079, evicted=0, evictedPerRun=0.0
2017-09-12 19:50:44,389 INFO  [MobFileCache #0] mob.MobFileCache: MobFileCache Statistics, access: 0, miss: 0, hit: 0, hit ratio: 0%, evicted files: 0
```

hbase 开始清理 WAL 后, 恢复正常。

恢复正常的时间及现场日志:

```log
ab-01,16020,1505206264265
2017-09-12 16:51:15,727 INFO  [WALProcedureStoreSyncThread] wal.WALProcedureStore: Remove log: hdfs://ab-01:8020/apps/hbase/data/MasterProcWALs/state-00000000000000000934.log
2017-09-12 16:51:15,728 INFO  [WALProcedureStoreSyncThread] wal.WALProcedureStore: Removed logs: [hdfs://ab-01:8020/apps/hbase/data/MasterProcWALs/state-00000000000000000935.log, hdfs://ab-01:8020/apps/hbase/data/MasterProcWALs/state-00000000000000000936.log, hdfs://ab-01:8020/apps/hbase/data/MasterProcWALs/state-00000000000000000937.log, hdfs://ab-01:8020/apps/hbase/data/MasterProcWALs/state-00000000000000000940.log, hdfs://ab-01:8020/apps/hbase/data/MasterProcWALs/state-00000000000000000941.log, hdfs://ab-01:8020/apps/hbase/data/MasterProcWALs/state-00000000000000000942.log, hdfs://ab-01:8020/apps/hbase/data/MasterProcWALs/state-00000000000000000943.log, hdfs://ab-01:8020/apps/hbase/data/MasterProcWALs/state-00000000000000000944.log, hdfs://ab-01:8020/apps/hbase/data/MasterProcWALs/state-00000000000000000945.log, hdfs://ab-01:8020/apps/hbase/data/MasterProcWALs/state-00000000000000000946.log, hdfs://ab-01:8020/apps/hbase/data/MasterProcWALs/state-00000000000000000947.log, hdfs://ab-01:8020/apps/hbase/data/MasterProcWALs/state-00000000000000000948.log, hdfs://ab-01:8020/apps/hbase/data/MasterProcWALs/state-00000000000000000949.log, hdfs://ab-01:8020/apps/hbase/data/MasterProcWALs/state-00000000000000000950.log, hdfs://ab-01:8020/apps/hbase/data/MasterProcWALs/state-00000000000000000951.log, hdfs://ab-01:8020/apps/hbase/data/MasterProcWALs/state-00000000000000000952.log, hdfs://ab-01:8020/apps/hbase/data/MasterProcWALs/state-00000000000000000953.log, hdfs://ab-01:8020/apps/hbase/data/MasterProcWALs/state-00000000000000000954.log, hdfs://ab-01:8020/apps/hbase/data/MasterProcWALs/state-00000000000000000955.log, hdfs://ab-01:8020/apps/hbase/data/MasterProcWALs/state-00000000000000000956.log, hdfs://ab-01:8020/apps/hbase/data/MasterProcWALs/state-00000000000000000957.log, hdfs://ab-01:8020/apps/hbase/data/MasterProcWALs/state-00000000000000000958.log, hdfs://ab-01:8020/apps/hbase/data/MasterProcWALs/state-00000000000000000959.log, hdfs://ab-01:8020/apps/hbase/data/MasterProcWALs/state-00000000000000000960.log, hdfs://ab-01:8020/apps/hbase/data/MasterProcWALs/state-00000000000000000961.log, hdfs://ab-01:8020/apps/hbase/data/MasterProcWALs/state-00000000000000000962.log, hdfs://ab-01:8020/apps/hbase/data/MasterProcWALs/state-00000000000000000963.log, hdfs://ab-01:8020/apps/hbase/data/MasterProcWALs/state-00000000000000000964.log, hdfs://ab-01:8020/apps/hbase/data/MasterProcWALs/state-00000000000000000965.log, hdfs://ab-01:8020/apps/hbase/data/MasterProcWALs/state-00000000000000000966.log, hdfs://ab-01:8020/apps/hbase/data/MasterProcWALs/state-00000000000000000967.log, hdfs://ab-01:8020/apps/hbase/data/MasterProcWALs/state-00000000000000000968.log, hdfs://ab-01:8020/apps/hbase/data/MasterProcWALs/state-00000000000000000969.log, hdfs://ab-01:8020/apps/hbase/data/MasterProcWALs/state-00000000000000000970.log, hdfs://ab-01:8020/apps/hbase/data/MasterProcWALs/state-00000000000000000971.log]
...
```

不断清理 WAL 直到清理完所有的 WAL, 之后每 1H 清理一次(即每 1H flush 一次)。
