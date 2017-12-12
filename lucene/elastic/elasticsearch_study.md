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
  4. check elasticsearch 状态: `curl http://localhost:9200`, 返回信息:
```json

```

- 安装 kibana

## 基本介绍

### `索引` 含义区分

1.

## 基本 API

### Java API

1. 节点客户端(node client)

2. 传输客户端(Transport client)

### RESTful API

可以通过 curl 命令与 elasticsearch 通信。

### 面向文档

### JSON

## 基本使用
