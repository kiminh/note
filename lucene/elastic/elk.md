# elk 环境搭建和使用

## 环境搭建

### elasticsearch

查看 [elasticsearch_studt.md](./elasticsearch_studt.md)

### kibana

TODO

### logstash

  1. 安装 Java 环境
  2. 下载安装 logstash 安装包, 安装包[下载地址](https://www.elastic.co/downloads/logstash)。 建议翻墙下载, 会稍快些。
  3. 配置 logstash, 启动 logstash。

- 配置 logstash
  > logstash 配置文件包括: `jvm.options`, `log4j2.properties`, `logstash.yml`, `log4j_to_es.conf`, `startup.options`。
  > 最小配置只需要配置 `logstash.yml`, `log4j_to_es.conf` 即可。

- logstash.yml 配置

```yml
#default configurations except http.host

# default path.data conf: LOGSTASH_HOME/data
# path.data:
pipeline.workers: 2
queue.max_events: 0
pipeline.output.workers: 1
pipeline.batch.size: 125
pipeline.batch.delay: 5
pipeline.unsafe_shutdown: false
#
# path.config:
config.test_and_exit: false
config.reload.automatic: false
queue.type: memory
queue.page_capacity: 250mb
# Default is 0 (unlimited)
queue.max_events: 0
queue.max_bytes: 1024mb
queue.checkpoint.acks: 1024
queue.checkpoint.writes: 1024
queue.checkpoint.interval: 1000
# 该处重新配置了允许所有 IP 传入, 生产环境下建议配置目标节点
http.host: "0.0.0.0"
http.port: 9600-9700
log.level: info
# default path.data conf: LOGSTASH_HOME/logs
# path.logs:
path.plugins: []
```

- log4j_to_es.conf 主要配置 input, output 和 filter。

提供 file 和 filebeat 两种配置方式, 其余方式请查看官网。
filter 配置提供一个 demo, 查看官网。

```conf
input{
  beat {
    port => 5044
  }
}
filter{
  grok {
     match => ['message','%{TIMESTAMP_ISO8601:logdate}']
  }
}
output{
  elasticsearch {
    action => "index"
    host => "elasticsearch.host:9200"
    index => "index_name"
  }
}
```

这里使用 filebeat 来采集日志, 也可以使用 logstash 自身来采集数据, 配置如下:

```conf
input{
  file {
    path => /path/to/your/logs/*.log
  }
}
filter{
  grok {
     match => ['message','%{TIMESTAMP_ISO8601:logdate}']
  }
}
output{
  elasticsearch {
    action => "index"
    host => "elasticsearch.host:9200"
    index => "index_name"
  }
}
```

### filebeat

filebeat 是用来替代 Logstash Forwarder 的下一代 Logstash 收集器, 是为了更快速稳定轻量低耗地进行收集工作, 它可以很方便地与 Logstash 连接, 也可以直接与 Elasticsearch 进行对接。
参考:

- [Beats 基础](http://soft.dog/2015/12/24/beats-basic/#section)
- [官方文档](https://www.elastic.co/guide/en/beats/filebeat/master/index.html)

filebeat 配置文件有 `filebeat.full.yml`, `filebeat.template-es2x.json`, `filebeat.template.json`, `filebeat.yml`。

## 使用
