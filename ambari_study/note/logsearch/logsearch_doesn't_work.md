# 

## 报错信息

现象: logsearch 界面不显示 service。

```
Solr has not accessible yet for audit_logs collection.(endpoint:/api/v1/audit/logs/resources/10)
Solr has not accessible yet for hadoop_logs collection.(endpoint:/api/v1/audit/logs/schema/fields)
Solr has not accessible yet for audit_logs collection.(endpoint:/api/v1/audit/logs/schema/fields)
```

zookeeper 错误信息
```
 Got user-level KeeperException when processing sessionid:0x35dc30269ff08b8 type:create cxid:0x4e5 zxid:0xa000049f0 txntype:-1 reqpath:n/a Error Path:/infra-solr/overseer Error:KeeperErrorCode = NodeExists for /infra-solr/overseer

```


## 第二次报错信息

```
[solr,hadoop_logs,worker=0] WARN  org.apache.ambari.logfeeder.output.OutputSolr$SolrWorkerThread (OutputSolr.java:393) - Solr is not reachable. Going to retry after 30 seconds. output=output:destination=solr,collection=hadoop_logs
org.apache.solr.client.solrj.impl.HttpSolrClient$RemoteSolrException: Error from server at http://192.168.1.101:8886/solr/hadoop_logs: No registered leader was found after waiting for 4000ms , collection: hadoop_logs slice: shard6
```



```
master3.hadoop.spacestar.com:2181,master1.hadoop.spacestar.com:2181,master2.hadoop.spacestar.com:2181/ranger_audits: cluster not found/not ready
```