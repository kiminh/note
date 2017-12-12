# pinpoint 学习

## pinpoint 介绍

pinpoint 是一个开源的 APM 工具, 用于基于 java 的大规模分布式系统, 基于 Google Dapper 论文。

- Google Dapper: Google 大规模分布式跟踪系统, 详细信息链接: https://research.google.com/pubs/pub36356.html

- pinpoint 解决的问题

用于基于java的大规模分布式系统, 通过跟踪分布式应用之间的调用来提供解决方案, 以帮助分析系统的总体结构和内部模块之间如何相互联系。

- 特点

分布式事务跟踪, 跟踪跨分布式应用的消息, 自动检测应用拓扑, 帮助你搞清楚应用的架构, 水平扩展以便支持大规模服务器集群, 提供代码级别的可见性以便轻松定位失败点和瓶颈, 使用字节码增强技术, 添加新功能而无需修改代码, 安装 agent 部署简单, 支持hdfs存储 具有简单的阀值触发报警功能, 移植性比较强的, 插件化功能可扩展(参考: https://github.com/naver/pinpoint/wiki/Pinpoint-Plugin-Developer-Guide)。

## 使用


## 结构

- 模块划分

```
annotations
agent
bootstrap
bootstrap
bootstrap
collector
commons
commons
commons
tools
plugins
profiler
profiler
profiler
rpc
thrift
test
web
hbase
flink
```
- 依赖关系

1. agent(bootstrap) -> bootstrap-core(commons & annotations)
  >实现 javaagent 完成采集和发送到 collector 的功能

2. bootstrap-core
  > `com.navercorp.pinpoint.bootstrap.context` 规定了 plugin 的接口, 自定义 plugin 实现这些接口完成新的 plugin。
  > 默认 plugin: HTTP, JDBC, Proxy,
  > `com.navercorp.pinpoint.bootstrap.classloader` 插件加载
  > classloader 可以通过 bootstrap-core-options 进行更多配置

3. pinpoint-profiler -> pinpoint-profiler-options -> (pinpoint-profiler-options-jdk(6, 7, 8))

## 监控插件

pinpoint 官方提供以下插件:

activemq-client, arcus, cassandra, cubrid-jdbc, cxf, dbcp, dbcp2, dubbo, google-httpclient, gson, hikaricp, httpclient3, httpclient4, hystrix, ibatis, jackson, jboss, jdk-http, jetty, json-lib, jsp, jtds, log4j, logback, mariadb-jdbc, mybatis, mysql-jdbc, netty, ning-asynchttpclient, okhttp, oracle-jdbc, postgresql-jdbc, redis, resin, resttemplate, rxjava, spring, spring-boot, thrift, tomcat, user, vertx, websphere。

```
├── activemq-client
├── arcus
├── cassandra
├── cubrid-jdbc
├── cxf
├── dbcp
├── dbcp2
├── dubbo
├── google-httpclient
├── gson
├── hikaricp
├── httpclient3
├── httpclient4
├── hystrix
├── ibatis
├── jackson
├── jboss
├── jdk-http
├── jetty
├── json-lib
├── jsp
├── jtds
├── log4j
├── logback
├── mariadb-jdbc
├── mybatis
├── mysql-jdbc
├── netty
├── ning-asynchttpclient
├── okhttp
├── oracle-jdbc
├── postgresql-jdbc
├── redis
├── resin
├── resttemplate
├── rxjava
├── spring
├── spring-boot
├── thrift
├── tomcat
├── user
├── vertx
└── websphere
```

插件开发详解参考: http://www.tangrui.net/2016/pinpoint-plugin-development.html

官方 plugin 扩展 demo: https://github.com/naver/pinpoint-plugin-sample
