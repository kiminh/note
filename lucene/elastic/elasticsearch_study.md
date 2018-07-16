# elastic

Elastic 是一个实时分布式搜索和分析引擎, 使用 Java 开发, 并 Apache Lucene(TM) 的开源搜索引擎作为其核心来实现所有索引和搜索功能, 目的是通过 RESTful API 来隐藏 Lucene 的复杂性, 简化全文搜索。

特点:
- 分布式的实时文件存储, 每个字段都被索引并可被索引
- 分布式的实时分析搜索引擎
- 可扩展到上百台服务器, 处理 PB 级结构化或非结构化数据

## 安装部署

  1. 安装 Java环境
  2. 下载 elasticsearch 二进制文件包
  3. 启动 elasticsearch( 可以使用默认配置, 也可以自行配置, 配置项后期单独介绍 ) `/path/to/elasticsearch/bin/elasticsearch [-d]`
  4. check elasticsearch 状态: `curl http://<host>:<port>`, 返回信息:

```json
{
  "name" : "your-name",
  "cluster_name" : "your-cluster-name",
  "cluster_uuid" : "cluster-uuid",
  "version" : {
    "number" : "your-version",
    "build_hash" : "build-hash",
    "build_date" : "build-date",
    "build_snapshot" : false,
    "lucene_version" : "6.4.0"
  },
  "tagline" : "You Know, for Search"
}
```

- 如果需要通过 Java 客户端的 TransportClient 连接 Elasticsearch, 配置中需要加上以下内容: 

```
client.transport.sniff: true
transport.tcp.port: 9300
```

- 安装 kibana

为方便查询, 使用 Kibana 的 `Dev Tools`

## 基本介绍

### `索引` 含义区分

1.

## 基本 API

### Java API

1. 节点客户端(node client)

2. 传输客户端(Transport client)

```java
// 配置 transport client, 注意与 elasticsearch 服务端配置相同的项的内容须相同
Settings settings = Settings.builder()
    .put("cluster.name", "cluster_name")
    .put("client.transport.ping_timeout", TimeValue.timeValueSeconds(60))
    // 开启使客户端去嗅探整个集群的状态
    .put("client.transport.sniff", true)
    .build();
TransportClient client = new PreBuiltTransportClient(settings)
    .addTransportAddress(new InetSocketTransportAddress(InetAddress.getByName("<elastic host>"), Integer.valueOf("9200")));
```

### RESTful API

可以通过 curl 命令与 elasticsearch 通信。

1. Update & Update By Query
2. Get
3. Index
4. Delete & Delete By Query
5. Mutil Get
6. Bulk API
7. Reindex API
8. Term Vectors
9. Mutil termvectors API
10. refresh

#### Search api

1. base usage

```bash
GET /_search
{
  # "from": 0, "size": 2,
  "query": {
    "term": {
      "key": "value"
    }
  }
}
```

### 面向文档

### JSON

## 基本使用

## 单元测试
