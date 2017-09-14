# ambari-metrics 上报数据存储

## 数据库

- 使用 HBase 存储，phoenix 作为 sql 引擎，对上报数据进行存储。
- 数据属性： Snappy 压缩、只有一个 Version 的数据、多个分不同时间级别的表存储相同的数据、使用布隆过滤器、block 大小为64KB、使用缓存存储 block。

## 存储模式

- 本地模式  
使用 HBase 的本地存储模式。默认模式。

- 分布式模式  
修改Advanced ams-hbase-site：将hbase.rootdir修改为hdfs://namenode_site:port/user/ams/hbase；将 dfs.client.read.shortcircuit 改为 true

**注意：**  
**如果启用了 HDFS 的 HA，不能直接连接某一个 NameNode，连接 HDFS 的地址为hdfs：**  

```
//hdfsnameservice/apps/ams/metrics
```

- 为 AMS 完成分布式存储以下步骤：
1. 为 ams 用户创建一个 HDFS 目录。
2. 停止 Metrics-Collector。
3. 将创建好的路径复制到 ams 的 **hbase-site.xml** 的配置文件中。

```
su - hdfs -c 'hdfs dfs -copyFromLocal/var/lib/ambari-metrics-collector/hbase/* /apps/ams/metrics'

su - hdfs -c 'hdfs dfs -chown -R ams:hadoop/apps/ams/metrics'
```

4. 选择分布式的模式。
5. 重启 Metrics-Collector。

- 启动  

```
HBASE_CONF_DIR="/etc/ams-hbase/conf" hbase shell
```

- 查看数据

```
# 通过 HBase 的过滤器以关键字为 key 查询监控指标是否存储
scan 'METRIC_RECORD',{FILTER => "PrefixFilter('<metric_name>')"}
```

## 表属性

- [Metrics-Configuration](https://cwiki.apache.org/confluence/display/AMBARI/Configuration)

phoenix表  
表名 | 描述 | 清理时间（默认） 
---|---|---
METRIC_RECORD | 用于记录每个机器上收集的每个 Metrics 属性 | 1天 
METRIC_RECORD_MINUTE | 聚合统计每个机器上的 Metrics 属性 | 1周 
METRIC_RECORD_HOURLY | 聚合统计每个机器上的 Metrics 属性 |30天 
METRIC_RECORD_DAILY | 聚合统计每个机器上的 Metrics 属性 |1年 
METRIC_AGGREGATE | 聚合统计所有机器上的 Metrics 属性（集群） | 1周 
METRIC_AGGREGATE_MINUTE | 聚合统计所有机器上的 Metrics 属性（集群） | 30天 
METRIC_AGGREGATE_HOURLY | 聚合统计所有机器上的 Metrics 属性（集群） | 1年 
METRIC_AGGREGATE_DAILY | 聚合统计所有机器上的 Metrics 属性（集群） | 2年 

###  METRIC_RECORD

该表是所有表中唯一存储实际 metrics 数据的表，其它表都是在此表的基础之上进行时间段的相应统计。

- 针对采集的 hosts 指标，即由 monitor 发送的指标值


- 针对采集的 hadoop-sink 指标


## metrics问题汇总

### redis上报数据未存储

- **场景：** 使用 ab-01 作为数据收集端，向 metrics-collector 所在节点发送数据。
 
**发送数据的脚本如下：**

```shell
#!/bin/sh
url=http://$1:6188/ws/v1/timeline/metrics
while [ 1 ]
do
#A=$(redis-cli info | grep total_connections_received: | awk -F ':' '{print $2}')
#echo $A \> abc.txt
#sed -i 's/\r//' abc.txt
#A=$(head -1 abc.txt)
millon_time=$(( $(date +%s%N) / 1000000 ))
A=`expr $RANDOM % 10`
json="{
    \"metrics\": [
        {
            \"metricname\": \"$2\",
            \"appid\": \"$3\",
            \"hostname\": \"localhost\",
            \"timestamp\": ${millon_time},
            \"starttime\": ${millon_time},
            \"metrics\": {
                \"${millon_time}\": ${A}
            }
        }
    ]
}"
echo $json | tee -a /root/my_metric.log
curl -i -X POST -H "Content-Type: application/json" -d "${json}" ${url}
sleep 3
done
```

### 上报数据与显示问题

- 所有通过 metrics 的 rest 接口上报的数据能存入 AMS 的 HBase 数据库。
- metrics.json 中配置的能够通过url来获取。
- widget.json 中做好相应的配置后才能在 ambari-server 的 Web UI 中显示。

#### 问题1

- **结果：** Ambari 的界面上的 redis 无监控图表。

- 问题排查

1. app timeline server 无法启动（修复后排除）详情查看；
2. 查询hbase监控数据存储（数据未存储）；
3. 调试后，发现由于**数据的发送方（ab-01）和接收方(ab-05)**时间差异较大导致 metrics-collector 将上报数据丢弃。

- 解决方法  
1. 暂时将 ab-01 的时间手动调至和 ab-05 相同的时间，发送数据成功。  
2. 根源解决：部署时间同步服务器。


