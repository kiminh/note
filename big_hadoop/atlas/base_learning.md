# atlas

atlas 是一个可扩展和可扩展的核心基础治理服务集 —— 使企业能够有效地和高效地满足 Hadoop 中的合规性要求，并允许与整个企业数据生态系统的集成。提供 Hadoop 生态下的数据治理。

主要提供以下功能：
  1. Metadata truth in Hadoop(truth?)
  2. Developed in the Open
  3. Data lineage
  4. Agile data modeling(?)
  5. REST API
  6. Metadata exchange

## 编译

单独编译 atlas: `mvn clean -DskipTests package -Pdist`

嵌入 HBase 和 Solr 编译 atlas: `mvn clean -DskipTests package -Pdist,embedded-hbase-solr`

编译后结果路径:
```
distro/target/apache-atlas-{version}-bin.tar.gz
distro/target/apache-atlas-{version}-bin.tar.gz
distro/target/apache-atlas-{version}-sources.tar.gz
```

- 编译遇到的问题
  atlas-ui 模块(dashboard2) 编译需要 node 和 npm, 下载超时。
  - 解决方法:

## atlas 说明

- **Atlas 的组件分为以下几个部分:**
  1. Core
  2. Integration
  3. Metadata source
  4. Application

- **Atlas 技术栈**

### Core

次类别包含实现 Atlas 功能的核心组件，包括:
  - Type System
  - Graph Engine
  - Titan DB

#### Type System

Type System 包含内容有:
  1. TypeCategory
  2.

Type System 是 Atlas 定义的元数据模型, 允许用户自定义自己的元数据类型, 开放对元数据类型的扩展。
目前 Atlas 支持的类型有:
  1.



#### Graph DB(Titan)

#### Graph Engine

### Integration

管理 atlas 元数据的方式:

- API
- Messaging

### Metadata source


### Application

  1. Search and proscription lineage
  2. 元数据驱动的权限控制
  3. 业务操作和数据建模
  4. 数据分类
  5. 元数据交换

## 用途

- 对以下服务提供元数据管理
  1. Hive
  2. Ranger
  3. Sqoop
  4. Storm/Kafka(limited support)
  5. Falcon(limited support)

- 特点
  1. Hadoop 元数据中心
  2. 数据声明周期管理
  3. 审计中心
  4. 安全: 和 HDP 安全(Kerberos & Ranger) 无缝集成
  5. 协议引擎

##

atlas modules:
common
typesystem: 类型系统, atlas 的核心模块
server-api

notification
client

authorization
catalog: get metadata source from hive catalog

```
### DB ###
dashboardv2: atlas ui
webapp
graphdb
titan
repository
```

plugin-classloader

addons/hdfs-model: atlas bridge
addons/hive-bridge-shim: atlas bridge
addons/hive-bridge: atlas bridge
addons/falcon-bridge-shim: atlas bridge
addons/falcon-bridge: atlas bridge
addons/sqoop-bridge-shim: atlas bridge
addons/sqoop-bridge: atlas bridge
addons/storm-bridge-shim: atlas bridge
addons/storm-bridge: atlas bridge

distro: build, package directory
