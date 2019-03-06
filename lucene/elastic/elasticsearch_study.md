# elastic 破冰

Elastic 是一个实时分布式搜索和分析引擎, 使用 Java 开发, 基于 Lucene, 目的是通过 RESTful API 来隐藏 Lucene 的复杂性, 简化全文搜索。

特点:

- 分布式的实时文件存储, 每个字段都被索引并可被索引。
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

```yml
client.transport.sniff: true
transport.tcp.port: 9300
```

- 安装 kibana

为方便查询, 使用 Kibana 的 `Dev Tools`。 Kibana 同时也是一个分析工具。

## 基本介绍

### `索引` 含义区分

1. Document

2. Field

3. Term

4. Token

## 基本 API 及其背后可能设计的操作

### REST ful 查询

1. Document

2. Index

3. 字段的自动类型

4. 特殊查询

### Java API

1. 节点客户端(node client)

2. 传输客户端(Transport client)

依赖:

```xml
<!-- maven 配置依赖添加 -->
<dependency>
    <groupId>org.elasticsearch.client</groupId>
    <artifactId>transport</artifactId>
    <version>${elastic.version}</version>
</dependency>
```

```groovy
// gradle 配置依赖添加
compile('org.elasticsearch.client:transport:${elasticsearch.version}')
```

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

- 注意:

  1. `transportClient` 是一个池化对象, 在初始化该对象前配置好池中的线程数即可。
  2. 在应用中, 一般通过单例的形式使用 `transportClient` 来连接 ES。

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

Elasticsearch 测试需要引入 Elasticsearch 的测试框架 Elasticsearch-framework:

- maven 版:
```xml
<dependency>
    <groupId>org.elasticsearch.test</groupId>
    <artifactId>framework</artifactId>
    <version>${elasticsearch.version}</version>
    <scope>test</scope>
</dependency>
```

- gradle 版

```groovy
testCompile group: 'org.elasticsearch.test', name: 'framework', version: ${version.es_test_framework}
```

这里采用在本地 Mock Elasticsearch Node 作为实际需要连接的 ES cluster, 并添加测试数据。 

Elasticsearch 提供相应的测试基类 `ESIntegTestCase`, 通过继承该类并 `Override` 需要的配置来启动自己的 Node, 自动在 `@Test` 中 添加用例内容。 

Demo: 

```java
@ESIntegTestCase.ClusterScope(scope = ESIntegTestCase.Scope.SUITE)
@ThreadLeakScope(ThreadLeakScope.Scope.NONE)
@RunWith(com.carrotsearch.randomizedtesting.RandomizedRunner.class)
public class ESMockTest extends ESIntegTestCase {

    private Client client;

    /**
     * 启动 Mock 的 ES Node, 并获取一些 Node 相关的变量。
     * 完成一些预加载的数据, 将其存入该 ES Node
     */
    @Before
    @Override
    public void setUp() throws Exception {
        super.setUp();
        this.client = ESIntegTestCase.client();
        setUpTestData(client);
    }

    private void setUpTestData(Client client) throws Exception {
        final String indexName = "testindex";
        final CreateIndexResponse createIndexResponse = client.admin().indices()
                .prepareCreate(indexName)
                .execute()
                .actionGet();

        if (!createIndexResponse.isAcknowledged()) {
            fail("Could not create index: " + indexName);
        }
    }

    /**
     * 配置 Mock 的 ES Node 的 Settings。
     * 注意配置项对应的 plugin 是否已经在 classpath 中。
     */
    @Override
    protected Settings nodeSettings(int nodeOrdinal) {
        return Settings.builder().put(super.nodeSettings(nodeOrdinal))
                .put("config-name", "config-value")
                .build();
    }

    /**
     * 用例, 自定义。
     */
    @Test
    public void test1() throws UnknownHostException {
        assertTrue(true);
    }
}

```

- 踩坑记录

1. ***Jar Hell***

    1. 关于 Jar Hell 的解释参考 [[译] JAR 地狱 (JAR Hell)](https://toutiao.io/posts/452124/app_preview)。
    2. 社区的讨论 [Elasticsearch jar hell when writing integration tests](https://discuss.elastic.co/t/elasticsearch-jar-hell-when-writing-integration-tests/64273)
    3. Github 上的 isuse [[TEST] Move Jarhell check inside the test.security.manager guard](https://github.com/elastic/elasticsearch/pull/16042), [Fail startup (and tests) on jar hell](https://github.com/elastic/elasticsearch/pull/11932)。
        > 解决方案, 通过工程管理工具查看工程引入的依赖关系, 如: `mvn dependency:tree`, `gradle dependencies`。 将重复引入或者与依赖中引入的有冲突的依赖配置的其中之一 `exclude` 掉。
    4. 其中一个 `Jar Hell` 是 JDK 中的, 为解决该问题, 最终跳过执行检查。 方式是复制 `org.elasticsearch.bootstrap.JarHell` 类, 并将相应的方法删除。 非万不得已不建议这么做。

2. 部分配置项无法在 Override 的 `nodeSetting(int nodeOrdinal)` 中添加, 如: `cluster.name` 等, 具体无法添加的配置项后续更新, 官网暂未找到说明; 部分配置项的配置需要有相应的插件, 才能够进行配置, 可以通过 Override `setUp()` 方法加载插件。

3. (本机使用 IDE 执行)用例执行过程中, 产生异常后, 会在一个临时路径下生成一个目录, 存放了生成的 Node 和相应的数据。无异常时在用例执行完成后会将生成的目录删除, 暂不清楚原因(在 OS X 上, 其他操作系统还未尝试过执行用例, 路径为 `/private/var/folders/7k/c9fgr0hs5v9g9vnfdsfrhnyr0000gn/T/com.c503/<Class>`)。

4. 直接使用命令行执行测试用例, 产生异常, 暂未解决。 怀疑是由于 `packager.jar` 和 `ant-javafx.jar` 的 Jar Hell 引起, 怀疑原因是在 IDE 中的 Jar Hell 也是这两个 jar 引起, 将 `ant-javafx.jar` 移出 classpath 后执行用例正常。但是, 将该包移出 java classpath 路径后命令行执行用例依然报以下错误:

    ```log
    java.lang.RuntimeException: found jar hell in test classpath
        at org.elasticsearch.bootstrap.BootstrapForTesting.<clinit>(BootstrapForTesting.java:91)
        at org.elasticsearch.test.ESTestCase.<clinit>(ESTestCase.java:175)
        at java.base/java.lang.Class.forName0(Native Method)
        at java.base/java.lang.Class.forName(Class.java:374)
        at com.carrotsearch.randomizedtesting.RandomizedRunner$2.run(RandomizedRunner.java:592)
    Caused by: java.nio.file.NoSuchFileException:<project_dir>/build/resources/test
        at java.base/sun.nio.fs.UnixException.translateToIOException(UnixException.java:92)
        at java.base/sun.nio.fs.UnixException.rethrowAsIOException(UnixException.java:111)
        at java.base/sun.nio.fs.UnixException.rethrowAsIOException(UnixException.java:116)
        at java.base/sun.nio.fs.UnixFileAttributeViews$Basic.readAttributes(UnixFileAttributeViews.java:55)
        at java.base/sun.nio.fs.UnixFileSystemProvider.readAttributes(UnixFileSystemProvider.java:145)
        at java.base/java.nio.file.Files.readAttributes(Files.java:1755)
        at java.base/java.nio.file.FileTreeWalker.getAttributes(FileTreeWalker.java:219)
        at java.base/java.nio.file.FileTreeWalker.visit(FileTreeWalker.java:276)
        at java.base/java.nio.file.FileTreeWalker.walk(FileTreeWalker.java:322)
        at java.base/java.nio.file.Files.walkFileTree(Files.java:2708)
        at java.base/java.nio.file.Files.walkFileTree(Files.java:2788)
        at org.elasticsearch.bootstrap.JarHell.checkJarHell(JarHell.java:201)
        at org.elasticsearch.bootstrap.JarHell.checkJarHell(JarHell.java:90)
        at org.elasticsearch.bootstrap.BootstrapForTesting.<clinit>(BootstrapForTesting.java:89)
        ... 4 more
    ```

6. elasticsearch 通过 `Mock node` 的方式执行单元测试, 不能使用 root 用户执行, 这与 elasticsearch 无法通过 root 用户启动原理相同。 
