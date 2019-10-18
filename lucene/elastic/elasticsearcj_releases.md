# Elasticsearch Releases

Elasticsearch release notes。 5.x, 6.x, 只介绍大版本 Release Note, 7.x 介绍每个次版本 Release Note.

- [5.0 Release Note](#50-release-note)
  - [5.0 废弃](#50-废弃)
  - [5.0 增强](#50-增强)
  - [5.0 Bug 修复](#50-bug-修复)
  - [5.0 升级](#50-升级)
- [6.0 Release Note](#60-release-note)
  - [6.0 Breaking Changes](#60-breaking-changes)
  - [6.0 Breaking Java Changes](#60-breaking-java-changes)
  - [6.0 废弃](#60-废弃)
  - [6.0 新特性](#60-新特性)
  - [6.0 增强](#60-增强)
  - [6.0 Bug 修复](#60-bug-修复)
  - [6.0 回滚](#60-回滚)
  - [6.0 升级](#60-升级)
- [6.1 Release Note](#61-release-note)
- [6.2 Release Note](#62-release-note)
- [6.3 Release Note](#63-release-note)
- [6.4 Release Note](#64-release-note)
- [6.5 Release Note](#65-release-note)
- [6.6 Release Note](#66-release-note)
- [6.7 Release Note](#67-release-note)
- [6.8 Release Note](#68-release-note)
- [7.0 Release Note](#70-release-note)
- [7.0 Breaking Changes](#70-breaking-changes)
  - [7.0 Breaking Java Changes](#70-breaking-java-changes)
  - [7.0 废弃](#70-废弃)
  - [7.0 新特性](#70-新特性)
  - [7.0 增强](#70-增强)
  - [7.0 Bug 修复](#70-bug-修复)
  - [7.0 回滚](#70-回滚)
  - [7.0 升级](#70-升级)
- [7.1 Release Note](#71-release-note)
- [7.2 Release Note](#72-release-note)
- [7.3 Release Note](#73-release-note)
- [7.4 Release Note](#74-release-note)

## 5.0 Release Note

### 5.0 废弃

- 废弃 `VersionType.FORCE`

### 5.0 增强

- Aggregations: 应该在 `TopHits` 聚合中应用 `Rescorer`
- Internal: `ShardActiveResponseHandler` 不应该保持整个集群状态
- Network: 修正了处理程序名未被完全读取的问题
- Packaging
  - 为存档发行版添加空插件目录
  - 对 Windows 服务进行显式的缺少设置
  - 更改配置文件的权限
- Search: 使用 URL 中的类型筛选器优化查询(t/t/_search)

### 5.0 Bug 修复

- Aggregatuibs
  - 在 `FilterAggregationBuilder` 中重写 查询/过滤器, 并确保客户端使用标记查询不可缓存
  - `ip_range` 聚合应该接受空界限
  - Thread safety for scripted significance heuristics(issues: [#18120](https://github.com/elastic/elasticsearch/issues/18120))
- Cat API: 尽早使用 `full_id` 请求参数
- Cache: 修正请求缓存键不保存对 `SearchContext` 的引用
- Circuit: 集群状态发布不应该触发断路器
- Core
  - 修复 ShardInfo#toString
  - 保护 `BytesStreamOutput` 不受当前写入字节数溢出的影响
  - 即使不满足 `_rollover` 条件, 也要返回目标索引名
- Engine: 在副本上重试复制请求不会调用 `onRetry`
- Index API: 如果 `dry_run=true`, 则尽早验证 `_rollover` 目标索引名也会失败
- Ingest: 存储的脚本和摄取节点配置应该包含在快照中
- Internal: 在返回线程池之前, 恢复线程的原始上下文
- Java Rest client: 不要在多次重试中重用同一个 `HttpAsyncResponseConsumer`
- Packaging
  - 安装 `systemd` 时设置 `vm.max_map_count`
  - `SysV` 初始化时导出 `ES_JVM_OPTIONS`
  - Debian: 启动/停止 守护进程不在后台运行
- Query DSL: 使用 `rescoreer` 时, 应该更新 `m1ax score`
- REST
  - mget 支持路由查询字符串参数, 但 rest 规范中没有
  - 修正了 `thread_pool_patterns` 路径变量的定义
  - 确保 `XContentBuilder` 在 `RestBuilderListener` 中始终处于关闭状态
- Reindex API
  - 将 `reindex-from-remote` 的缓冲区提升到 200mb
  - 修正了<2.0中 reindex-from-remote 中 parent/chils 的问题
- Search
  - 修正了搜索时获取 TTL 值时的可缓存性问题
  - 删除 `LateParsingQuery` 以防止上下文冻结后的时间戳访问
- Snapshot/Restore
  - 确保在快照期间清除临时 `index-*` 分代 blobs
  - 修复程序在请求 `_all` 时获得快照副本

### 5.0 升级

- Core: 升级到 `Lucene` 6.2.1
- Date: 升级到 `Joda time` 2.9.5

## 6.0 Release Note

### 6.0 Breaking Changes

- Aggregation: 更改date_range聚合中数值 `to` 和 `from` 参数的解析
- Aliases: Wrong behavior deleting alias
- Allocation: 移除 `cluster.routing.allocation.snapshot.relocation_enable` 配置
- Analysis
  - 不允许自定义分析器具有与内置分析器相同的名称
  - 在 `_analyze` API中删除查询字符串参数
- CAT API: 在 cat 线程池中的无界队列上写入 -1
- CURD
  - `GetRequest` 中不允许 `Version.FORCE`
  - 在 6.x 的 indices 中不允许 `Version.FORCE`
  - 如果索引不存在, 删除 doc 将不会自动创建它
- Cluster
  - 数据路径中不再允许集群名
  - Disallow : in cluster and index/alias names
- Core
  - 简化文件存储
  - 严格执行布尔转换
  - 删除 `default` 存储类型
  - 删除存储节流
- Geo
  - 删除废弃的 Geo 查询特性
  - Reduce GeoDistance Insanity
- Highlighting: 删除张贴高亮, 并使统一的默认高亮选择
- Index APIs
  - 在索引表达式中移除对 + 的支持
  - 删除 index API, 只针对具体的索引工作
  - 默认情况下, 打开/关闭 index api 来允许 `_no_indices`
  - 从 indices exists api中删除对有争议的 `ignore_unavailable` 和 `allow_no_indices` 的支持
- Index Template: 允许为索引模板指定多个模式
- Index Scripts/Template: Scripting: 删除搜索模板动作
- Ingest
  - 更新 ingest-user-agent regexes.yml
  - 删除 ingest.new_date_format
- Inner Hits: 返回嵌套的内部 `hit` 的 `_source`, 而不将它包装到完整的路径上下文中
- Java API: 在 rest 层上执行 `Content-Type` 需求. 并删除废弃方法
- Java Rest Client: 删除 `Index`, `Delete` 和 `Bulk` 中已废弃的 create 和 found
- Mapping
  - 拒绝超出浮点数、双精度浮点数和半浮点数范围的数字
  - 最多执行一种类型
  - 6.0+ 的 indices 不允许 `include_in_all`
  - 默认情况下禁用 `_all`, 不允许在 6.0+ indices 上配置 `_all`
  - 在无法识别的 `match_mapping_type` 上抛异常
- Network
  - 删除未使用的 `Netty-related` 设置
  - 移除阻塞的 TCP 客户端和服务器
  - 删除模块 `transport_netty_3`, 用 `netty_4` 代替
  - 删除 `LocalTransport`, 用 `MockTcpTransport` 代替
- Packaging
- Percolator
- Plugin Analysis ICU
- Plugin Delete By Query
- Plugin Discovery Azure Classic
- Plugin Discovery EC2
- Plugin Lang JS
- Plugin Mapper Attachment
- Plugin Repository Azore
- Plugin Repository GCS
- Plugin Repository S3
- Plugins
- Query DSL
  - 为 `match query` 删除废弃的 `_type`, `slop` 字段
- REST
- Scripting
- Search
- Sequence IDs
- Shadow Replicas
- Similarities

### 6.0 Breaking Java Changes

- Aggregation:
- Internal
- Java API
- Java High Level REST Client
- Java REST Client
- Network
- Plugin Delete by Query
- Plugins
- Query DSL

### 6.0 废弃

- Index APIs
- Index Template
- Index Scripts/Template
- Internal
- Percolator
- Plguins
- Scripting
- Settings
- Tribe Node

### 6.0 新特性

- Aggregation
- Analysis
- Core
- Internal
- Mapping
- Parent/Child
- Plugin analysis ICU
- Search
- Sequence IDs
- Stats
- Task Manager
- Upgrade Manager

### 6.0 增强

- Aggregation
- Allocation
- Analysis
- Bulk
- CAT API
- CRUD
- Circute Breakers
- Cluster
- Core
- Discovery
- English
- Exceptions
- Highlighting
- Index APIs
- Index Template
- Ingest
- Inner Hits
- Internal
- Java API
- Java High Level REST Client
- Java REST Client
- Logging
- Mapping
- Network
- Packaging
- Parent/Child
- Percolator
- Plugin Discovery EC2
- Plugin Lang Pipeline
- Plugin Repository GCS
- Plugin Repository HDFS
- Plugin Repository S3
- Query DSL
- REST
- Recovery
- Scripting
- Search
- Search Templates
- Sequence IDs
- Settings
- Snapshot/Restore
- Stats
- Store
- Suggesters
- Task Manager
- Translog

### 6.0 Bug 修复

- Aggregation
- Aliases
- Analysis
- CAT API
- CRUD
- Cache
- Circute Breakers
- Cluster
- Core
- Dates
- Discovery
- Engine
- Geo
- Highlighting
- Index APIs
- Ingest
- Inner Hits
- Internal
- Java API
- Java High Level REST Client
- Java Rest Client
- Logging
- Mapping
- More Like this
- Not Classified
- Nested Dock
- Packaging
- Parent/Child
- Percolator
- Plugin Analysis Kuromoji
- Plugin Analysis Phonetic
- Plugin Discovery Phonetic
- Plugin Discovery File
- Plugin Ingest Attachment
- Plugin lang Pipeline
- Plugin Repository Azure
- Plugin Repository GCS
- Plugin Repository HDFS
- Plugin Repository S3
- Plugins
- Query DSL
- REST
- Recovery
- Reindex API
- Scroll
- Search
- Sequence IDs
- Settings
- Similarities
- Snapshot/Restore
- Stats
  - 修复了 RestGetAction 名字输入错误
  - 以微秒为单位累积滚动时间
  - `_nodes/stats` 不应该因为并发的AlreadyClosedException而失败
  - 避免在当前查询计数上重复减量
  - 在 `huge FSes` 将可用和空闲字节调整为非负的
- Suggesters
  - 修正短语指示符中除0导致断言失败的问题
  - Context Suggester 应该过滤 doc 值字段
  - 修正了上下文建议从关键字类型字段中读取值的问题
- Templates: 修复了 `FullClusterRestartIT.testSnapshotRestore` 尝试失败
- Translog: 为序列化 ID 修改了 `TransLog.Delete` 的序列化
- Upgrade API: 修正过多的日志记录和不必要的模板更新

### 6.0 回滚

- Bulk: 只有在需要更新映射时才重新解析操作([Issue#23665][https://github.com/elastic/elasticsearch/pull/23665])
- Highlighting: 修正快速向量高亮 NPE 匹配短语前缀([Issue#25088](https://github.com/elastic/elasticsearch/issues/25088))
- Search: 始终使用 DisjunctionMaxQuery 来构建跨子段析取([Issue#25088](https://github.com/elastic/elasticsearch/issues/25088))
- Sequence IDs: 在 6.0.0-beta1 中索引性能下降([Issue#26339](https://github.com/elastic/elasticsearch/issues/26339))

### 6.0 升级

- Core: 升级到 Lucene 7.0.0
- Logging: 升级到 Log4j 2.9.1
- Network: 升级到 Netty 4.1.13 Final
- Plugin Ingest Attachment: 升级到 [Tika](http://tika.apache.org/) 1.14
- Upgrade API: 改进 `TemplateUpgradeServiceIT` 测试的稳定性和日志记录

## 6.1 Release Note

## 6.2 Release Note

## 6.3 Release Note

## 6.4 Release Note

## 6.5 Release Note

## 6.6 Release Note

## 6.7 Release Note

## 6.8 Release Note

## 7.0 Release Note

## 7.0 Breaking Changes

- Aggregation
  - 移除对已启用的用于脚本化的度量聚合参数 params._agg/_aggs
  - Percentile/Ranks 75   返回 `null` 而不是 `Nan`
  - 如果统计为零, 则将 sum 呈现为零
- Analysis
  - 移除 `delimited_payload_filter`
  - 限制 `_analyze` 生成的令牌数量
  - 为 `ngram` 和 `shingle` 设置添加限制
- Audit
  - 日志文件审核设置在弃用后删除
  - 删除索引审计输出类型
- Authentication
  - Security: 移除包装, 放入用户响应
  - 删除令牌失效的 `bwc` 逻辑
- Authorization
  - 在启用安全性时移除别名解析限制
  - 删除隐式索引监视器权限
- Circle Breakers: 降低 `fielddata` 断路器的默认限制
- CRUD
  - 版本冲突异常消息增强
  - 在 `UpdateRequest` 中使用 `ObjectParser`
  - 删除对并发控制的内部版本控制的支持
- Distribution
  - 删除非法 `action.master.force_local` 设置
  - 删除部落节点支持[Tribe node 是一类特殊的 Coordinating node (Client node), 这种节点可以连接多个集群, 可以对多个集群进行搜索和其他操作。]
  - 禁止 `index.unassigned.node_left.delayed_timeout` 为负值
  - 删除集群状态大小
- Features/Features: 删除 `Migration`, `Upgrade` 和 `Assistance` API
- Features/Indices APIs
  - Indices Exists 的 `API` 应该为空通配符返回404
  - 默认 1 个 Shard
  - 限制 `nested documents` 的数量
- Features/Ingest
  - 将配置 `Except.Data` 添加到 `Metadata` 中
  - 为用户代理 `ingest` 处理器添加 ECS 模式
  - 移除 `ingest` 插件的特殊处理
- Features/Java Low level REST Client: 放弃对JDK 7上的 Low level REST Client 的支持
- Features/Watcher: 删除监视帐户的 `unsecure` 设置
- Features/Stats
  - 从stats api中删除 `suggest metrics`
  - 将 cat 线程池信息与线程池配置对齐
  - 将线程池信息与线程池配置对齐
- Geo: 使用 `geohash cell` 代替 `geo_bounding_box` 中的一个角落
- Index APIs: 始终执行集群范围的碎片限制
- Infra/Circuit Breakers
  - Introduce durability of circuit breaking exception
  - 基于实际内存使用的电路中断
- Infra/Core
  - 默认节点名到主机名
  - 删除写线程池的批量回退
  - CSS: 从远程集群信息中删除 `HTTP` 地址
  - 删除索引线程池
  - 主响应在正常时不应该有 503 状态
  - Automatically prepare indices for splitting
  - 不要在 `_flush`, `_force_merge` 和 `_upgrade` 上刷新
- Infra/Logging: Elasticsearch json logging
- Infra/RES`T API
  - Packaging: 从 tar 发行版中删除 Windows bin 文件
  - 将 `ingest-user-agent` 打包为一个模块
  - 将ingest-geoip打包为一个模块
- Infra/Packaging
  - 删除对清除缓存索引的 GET 支持
  - 清除索引缓存API删除废弃的 url 参数
- Infra/Scripting
  - 删除对已废弃 StoredScript 上下文的支持
  - 从 `ScriptDocValues` 中删除 `getDate` 方法
  - 在 7.0.0 中删除 `ScriptDocValues#date` 和 `ScriptDocValues#dates`
- Infra/Settings: 删除配置提示的秘密和文本
- Mechine Learning: 从 `datafeed` 中删除 `types`
- Mapping
  - 对非索引字段的短语查询应该抛出异常
  - 删除遗留 `mapping` 代码
  - 拒绝更新 `_default_mapping`
  - 删除 `update_all_types` 选项
  - 删除 `_default_` mapping
  - 拒绝数值字段的 `index_options` 参数
  - 将 `include_type_name` 的默认值更新为 false
  - 在 `RestGetIndicesAction` 中支持 `include_type_name`
- Network
  - 删除 `http.enabled` 配置
  - 删除 HTTP 最大内容长度从宽
  - 删除作为默认 `SSL` 协议的 `TLS 1.0`
  - Security: 删除 SSL 设置回退
- Percolator: 删除已废弃的 percolator `map_unmapped_fields_as_string` 设置
- Ranking
  - Add minimal sanity checks to custom/scripted similarities
  - 要求重核的滚动查询被认为是无效的
  - 禁止在 `function_score` 查询中出现负分数
  - 禁止分析查询中的负字段提升
- Scripting: 从 `ScriptDocValues` 中删除废弃的 `getValues`
- Search
  - 删除已废弃的 url 参数 `_source_include` 和 `_source_exclude`
  - 不允许负查询增加
  - 禁止在函数得分查询中出现负权值
  - 在 field capabilities API 中, 删除对在请求体中提供字段的支持
  - 删除 `query_string` 的废弃选项
  - 修正拉普拉斯记分器乘以(and not add)
  - 删除 `_primary` 和 `_replica` 碎片首选项
  - 限制扩展字段的数量, 如 `query_string` 和 `simple_query_string`
  - 使纯负查询返回 0 分
  - 删除已弃用的 _termvector endpoint
  - 删除不赞成的 Graph endpoints
  - 验证 `_msearch` 上的元数据
  - 将 `hits.total` 作为搜索响应中的对象
  - 在 `Querybuilder` 中删除 query 和 filter context 之间的区别
  - 在 span_or 查询中设置 boost 时抛出解析异常
  - 默认情况下, hit 总数最多可达 10,000 个
  - 默认使用 `mappings` 来格式化 `doc-value` 字段
- Security: 删除在试用许可证上启用安全性的启发方法
- Snapshot/Restore
  - 在快照元数据中包含快照的大小
  - 删除azure 废弃的设置
- Store
  - 在 7.0 中删除 `elasticsearch-translog`
  - completely drop index.shard.check_on_startup: fix for 7.0
- Suggesters
  - 修正 `Suggesters` 中阈值频率的计算
  - 使 `Geo Context` 映射解析更加严格
  - 删除在没有上下文的情况下索引或查询上下文建议的功能
- ZenDiscovery
  - Best-effort cluster formation if unconfigured
  - 删除 `DiscoveryPlugin#getDiscoveryTypes`

### 7.0 Breaking Java Changes

- Aggregations
- Analysis
- Features/Java High Level REST Client
- Features/Java Low Level REST Client
- Geo
- Infra/Core
- Infra/Transport API
- ZenDiscovery

### 7.0 废弃

- Aggregations
- Analysis
- Audit
- Core
- Cluster Coordination
- Features/Indices APIs
- Features/Ingest
- Features/Java High Level REST Client
- Features/Watcher
- Graph
- Infra/Core
- Infra/Packaging
- Infra/Scripting
- Infra/Transport API
- Mechine Learning
- Mapping
- Migration
- Monitor
- Rollup
- Scripting
- Search
- Security
- SQL
- Watcher

### 7.0 新特性

- Allocation
- Analysis
- Authentication
- Authorization
- CCR
- Distributed
- Features/ILM
- Features/Ingest
- Features/Java High Level REST Client
- Features/Monitor
- Geo
- Infra/Core
- Infra/Logging
- Infra/Plugins
- Infra/Rest API
- Java High Level REST Client
- Java Low Level REST Client
- Mechine Learning
- Mapping
- Ranking
- Recovery
- SQL
- Search
- Security
- Suggesters

### 7.0 增强

- Aggregations
- Allocation
- Analysis
- Audit
- Authentication
- Authorization
- Build
- CCR
- Client
- Cluster Coordination
- Core
- CRUD
- Discovery-plugins
- Distributed
- Engines
- Features/CAT APIs
- Features/Features
- Features/ILM
- Features/Indices APIs
- Features/Ingest
- Features/Java High Level REST Client
- Features/Monitor
- Features/Stats
- Features/Watcher
- Geo
- Index APIs
- Infra/Core
- Infra/Logging
- Infra/Packaging
- Infra/REST API
- Infra/Scripting
- Infra/Packaging
- Infra/Setting
- Infra/Transport API
- Ingest
- Java High Level REST Client
- Java Low Level REST Client
- License
- Mechine Learning
- Mapping
- Network
- Packaging
- Percolator
- Plugins
- Ranking
- Recovery
- Rollup
- Recovery
- Scripting
- Setting
- Search
- Security
- Snapshot/Restore
- Stats
- Store
- SQL
- Suggest
- Task Management
- ZenDiscovery

### 7.0 Bug 修复

- Aggregation
- Allocation
- Analysis
- Audit
- Authorization
- Authentication
- Build
- CCR
- Circuit Breakers
- Core
- CRUD
- Cluster Coordination
- Distributed
- Engine
- Features/Features
- Features/ILM
- Features/Indices APIs
- Features/Ingest
- Features/Java High Level REST Client
- Features/Java Low Level REST Client
- Features/Monitoring
- Features/Watcher
- Geo
- Highlighting
- Infra/Core
- Infra/Packaging
- Infra/REST AIP
- Infra/Scripting
- Infra/Settings
- Infra/Transport API
- Index APIs
- Ingest
- License
- Mechine Learning
- Mapping
- Network
- Packaging
- Plugins
- Ranking
- Recovery
- Rollup
- Scripting
- Search
- Security
- Snapshot/Restore
- SQL
- Suggesters
- Take Management
- Watcher
- ZenDiscovery

### 7.0 回滚

- Infra/Core
- Mapping
- Scripting

### 7.0 升级

- Discovery-Plugin
- Engine
- Geo
- Ingest
- Infra/Core
- Security
- Search
- Snapshot/Resotre
- Network
- Mechine Learning

## 7.1 Release Note

## 7.2 Release Note

## 7.3 Release Note

## 7.4 Release Note
