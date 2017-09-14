# hbase meta 表 恢复相关

[hbase meta表修复](http://blog.csdn.net/u010316405/article/details/48003747)

1. meta 表修复1

```bash
# 查看 hbasemeta 情况
hbase hbck
# 重新修复 hbase meta 表 (根据 hdfs 上的 regioninfo 文件, 生成meta表)  
hbase hbck -fixMeta
# 重新将 hbase meta 表分给 regionserver (根据 meta 表, 将 meta 表上的 region 分给 regionservere)  
hbase hbck -fixAssignments
```

2. meta 表修复2

```bash
# 当出现漏洞
# 新建一个 region 文件夹
hbase hbck -fixHdfsHoles
# 根据 regioninfo 生成 meta 表
hbase hbck -fixMeta
# 分配 region 到 regionserver 上
hbase hbck -fixAssignments
```

3. meta 表修复3

```shell
# 缺少 regioninfo  
hbase hbck -fixHdfsOrphans 
```

4. 重复 region 问题: 

```bash
# in hbase shell
# 查看 meta 中的 region
scan 'hbase:meta' , {LIMIT=>10,FILTER=>"PrefixFilter('INDEX_11')"}
  
# 在数据迁移的时候碰到两个重复的 region
# b0c8f08ffd7a96219f748ef14d7ad4f8, 73ab00eaa7bab7bc83f440549b9749a3
  
# 删除两个重复的 region
  
delete 'hbase:meta','INDEX_11,4380_2431,1429757926776.b0c8f08ffd7a96219f748ef14d7ad4f8.','info:regioninfo'

delete 'hbase:meta','INDEX_11,5479_0041431700000000040100004815E9,1429757926776.73ab00eaa7bab7bc83f440549b9749a3.','info:regioninfo'
  
# 删除两个重复的 hdfs
# hdfs command
hdfs dfs -rm -r /hbase/data/default/INDEX_11/b0c8f08ffd7a96219f748ef14d7ad4f8
hdfs dfs -rm -r /hbase/data/default/INDEX_11/73ab00eaa7bab7bc83f440549b9749a3

# 对应的重启 regionserver (只是为了刷新 hmaster 上汇报的RIS的状态)

# 肯定会丢数据, 把没有上线的重复 region 上的数据丢失
```

5. 新 hbase hbck

    - 新版本的 hbck 可以修复各种错误, 修复选项是：
    1. -fix, 向下兼容用, 被 ```-fixAssignments```替代
    2. -fixAssignments, 用于修复 region assignments 错误
    3. -fixMeta, 用于修复 meta 表的问题, 前提是 HDFS 上面的 region info 信息有并且正确。
    4. -fixHdfsHoles, 修复 region holes (空洞, 某个区间没有 region) 问题
    5. -fixHdfsOrphans, 修复 Orphan region (hdfs上面没有.regioninfo 的 region)
    6. -fixHdfsOverlaps, 修复 region overlaps (区间重叠)问题
    7. -fixVersionFile, 修复缺失 hbase.version 文件的问题
    8. -maxMerge (n默认是5), 当 region 有重叠是, 需要合并 region, 一次合并的 region 数最大不超过这个值。
    9. -sidelineBigOverlaps , 当修复 region overlaps 问题时, 允许跟其他 region 重叠次数 最多的一些 region 不参与(修复后, 可以把没有参与的数据通过bulk load加载到相应的 region)
    10. -maxOverlapsToSideline (n默认是2), 当修复 region overlaps 问题时, 一组里最多允许多少个 region 不参与
    由于选项较多, 所以有两个简写的选项
    11. -repair, 相当于 ```-fixAssignments -fixMeta -fixHdfsHoles -fixHdfsOrphans -fixHdfsOverlaps -fixVersionFile -sidelineBigOverlaps```
    12. -repairHoles, 相当于 ```-fixAssignments -fixMeta -fixHdfsHoles -fixHdfsOrphans```

    - 新版本的 hbck   
    1. 缺失 hbase.version 文件: 
        - 加上选项 ```-fixVersionFile```
    2. 如果一个 region 即不在 META 表中, 又不在hdfs上面, 但是在 regionserver 的 online region 集合中: 
        - 加上选项 ```-fixAssignments```
    3. 如果一个 region 在 META 表中, 并且在 regionserver 的 online region 集合中, 但是在 hdfs 上面没有: 
        - 加上选项 ```-fixAssignments``` -fixMeta, ( -fixAssignments 告诉 regionserver close region), ( -fixMeta 删除 META 表中 region 的记录)
    4. 如果一个 region 在 META 表中没有记录, 没有被 regionserver 服务, 但是在 hdfs 上面有: 
        - 加上选项 ```-fixMeta``` -fixAssignments, ( -fixAssignments 用于 assign region), ( -fixMeta 用于在 META 表中添加 region 的记录)
    5. 如果一个 region 在 META 表中没有记录, 在 hdfs 上面有, 被 regionserver 服务了: 
        - 加上选项 ```-fixMeta```, 在 META 表中添加这个 region 的记录, 先 undeploy region, 后 assign
    6. 如果一个 region 在 META 表中有记录, 但是在 hdfs 上面没有, 并且没有被 regionserver 服务: 
        - 加上选项 ```-fixMeta```, 删除META表中的记录
    7. 如果一个 region 在 META 表中有记录, 在 hdfs 上面也有, table 不是 disabled 的, 但是这个 region 没有被服务: 
        - 加上选项 ```-fixAssignments```, assign 这个 region
    8. 如果一个 region 在 META 表中有记录, 在 hdfs 上面也有, table 是 disabled 的, 但是这个 region 被某个 regionserver 服务了: 
        - 加上选项 ```-fixAssignments```, undeploy 这个 region
    9. 如果一个 region 在 META 表中有记录, 在 hdfs 上面也有, table 不是 disabled 的, 但是这个 region 被多个 regionserver 服务了: 
        - 加上选项 ```-fixAssignments```, 通知所有 regionserver close region, 然后 assign region
    10. 如果一个 region 在 META 表中, 在 hdfs 上面也有, 也应该被服务, 但是 META 表中记录的 regionserver 和实际所在的 regionserver 不相符: 
        - 加上选项 ```-fixAssignments```
    11. region holes
        - 需要加上 ```-fixHdfsHoles```, 创建一个新的空 region , 填补空洞, 但是不 assign 这个 region, 也不在 META 表中添加这个 region 的相关信息
    12. region 在 hdfs 上面没有 .regioninfo 文件
        - 加上 ```-fixHdfsOrphans```
    13. region overlaps
        - 需要加上 ```-fixHdfsOverlaps```
   
    - 说明:    
    1. 修复 region holes 时, ```-fixHdfsHoles``` 选项只是创建了一个新的空 region, 填补上了这个区间, 还需要加上-fixAssignments -fixMeta 来解决问题, (-fixAssignments 用于 assign region), ( -fixMeta 用于在 META 表中添加 region 的记录), 所以有了组合 ```-repairHoles``` 修复 region holes, 相当于 ```-fixAssignments -fixMeta -fixHdfsHoles -fixHdfsOrphans```
    2. -fixAssignments, 用于修复 region 没有 assign, 不应该 assign, assign 了多次的问题
    3. -fixMeta, 如果 hdfs 上面没有, 那么从 META 表中删除相应的记录, 如果 hdfs 上面有, 在 META 表中添加上相应的记录信息
    4. -repair 打开所有的修复选项, 相当于-fixAssignments -fixMeta -fixHdfsHoles -fixHdfsOrphans -fixHdfsOverlaps -fixVersionFile -sidelineBigOverlaps
   
    - 新版本的hbck从  
        (1) hdfs目录;  
        (2) META;  
        (3) RegionServer 这三处获得 region 的 Table 和 Region 的相关信息, 根据这些信息判断并 repair。

Kylin hbase 表恢复  顺序: 
1. ```kylin_metadata```
2. ```kylin_metadata_acl```
3. ```kylin_metadata_user```
4. 



