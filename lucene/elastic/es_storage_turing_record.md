# ES 存储压缩优化及集群构建记录

- [Summary](#summary)
  - [场景描述](#场景描述)
  - [压缩优化](#压缩优化)
- [Storage 优化](#storage-优化)
  - [Field 类型优化](#field-类型优化)
  - [Compression 优化](#compression-优化)
- [集群构建](#集群构建)

## Summary

### 场景描述

船舶数据上报约每秒 100 条, 大致每月需存储 2.7 亿条船舶数据, 目前已有的情况是: 按月创建不同的 Index, 设置好地理位置字段, 其余字段全部使用默认配置。 其中某月存储数据 18595335 条, 5 个 shard(默认配置), `number_of_replicas = 1`, 消耗存储 4.21 GB。 原 ES 为单节点的部署形式, 虽然设置 `number_of_replicas = 1`, 但实际没有 replication 。 详细情况如下(省略部分为字段类型已存在的部分):

```json
{
    "state":"open",
    "settings":{
        "index":{
            "creation_date":"1548736143607",
            "number_of_shards":"5",
            "number_of_replicas":"1",
            "uuid":"7y7tQlsfR_GfYckG3Ir4VA",
            "version":{
                "created":"5020099"
            },
            "provided_name":"xxxx"
        }
    },
    "mappings":{
        "geopoint_history":{
            "properties":{
                "string_field":{
                    "type":"text",
                    "fields":{
                        "keyword":{
                            "ignore_above":256,
                            "type":"keyword"
                        }
                    }
                },
                "timestamp_field":{
                    "type":"long"
                },
                "location_field":{
                    "type":"geo_point"
                }
            }
        }
    }
}
```

关于 ES field 类型, 详见 [Field datatypes](https://www.elastic.co/guide/en/elasticsearch/reference/current/mapping-types.html), 使用正确的 Field 类型能够减少 Storage 用量。

### 压缩优化

ES 默认会对数据进行压缩, 默认使用 LZ4 压缩算法将数据存储在磁盘, 通过 `"index.codec": "best_compression"` 配置项设置使用压缩率更高的压缩算法 [DEFLATE](https://en.wikipedia.org/wiki/DEFLATE), 详见 [Index Modules#Static index settings](https://www.elastic.co/guide/en/elasticsearch/reference/5.2/index-modules.html#_static_index_settings)。

关于 LZ4 和 DEFLATE 压缩算法的比较, 可以参考博客: [Java不同压缩算法的性能比较](https://blog.csdn.net/runming56/article/details/49944021)。

## Storage 优化

- 对于存储的优化, 需要先对这些数据进行分类, 将数据分为 `hot` 和 `warm` 两类数据, index 为 `hot` 的数据通常为检索较频繁的数据; index 为 `warm` 的数据通常为检索不频繁, 不会再执行插入, 更新, 删除等操作的数据, 通常为归档数据。 针对这两种不同类型的数据, 需要对其使用不同的存储方式。
- 对 Index 中的每个 Field 的类型在建立 Index 时指定好, 比如某个 Field 如果只用于检索, 设置为 `Text` 字段, 如果只用于聚合操作, 设置为 `keyword` 类型, 使用默认类型同时满足上面两种类型, 最终持久化后的内容占用的存储也更多。
- 选择合适的压缩算法。

### Field 类型优化

在对 Field 类型进行优化时, 需要先了解 ES Field 类型, 以及不同类型的作用。 ES 在插入时会自动为所有插入的 Field 指定类型, 详见 [Dynamic Mapping](https://www.elastic.co/guide/en/elasticsearch/reference/current/dynamic-mapping.html), 因此需要再插入数据前配置 所有 Field 类型。 操作实际为 ES Mapping 操作, 详见 [Mapping](https://www.elastic.co/guide/en/elasticsearch/reference/current/mapping.html)。

根据上述原则, 将原 Mapping 按照一下原则配置:

```json
{
    "mappings":{
        "geopoint_history":{
            "properties":{
                "aggregation_field":{
                    "type":"keyword"
                    }
                },
                "index_field":{
                    "type":"text"
                    }
                },
                "both_field":{
                    "type":"text",
                    "fields":{
                        "keyword":{
                            "ignore_above":256,
                            "type":"keyword"
                        }
                    }
                    }
                },
                "timestamp_field":{
                    "type":"long"
                },
                "location_field":{
                    "type":"geo_point"
                }
            }
        }
    }
}
```

关闭 `_all` 字段。 `_all` 字段是把所有其它字段中的值, 以空格为分隔符组成一个大字符串, 然后被分析和索引, 但是不存储, 也就是说它能被查询, 但不能被取回显示, 如果开启 `_all` 字段, 指定 Field 或所有 Field 的值会组成一个超级字段, `_all` 字段在查询时占用更多的 CPU 和占用更多的磁盘空间, 如果确实不需要它可以完全的关闭它或者基于字段定制。 由于无使用 `_all` Field 需求, 因此将该字段关闭。 由于无只需要检索的 Field, 因此无 Field 只有 `Text` 类型。

操作指令为:

```elastic
PUT ship_data_hot_1
{
  "settings": {
    "index.number_of_replicas": "0",
    "index.routing.allocation.require.type": "hot"
  },
  "mappings": {
    "geopoint_history": {
      "_all": {
        "enabled": false
      },
      "properties": {
        "boss_field": {
          "type": "text",
          "fields": {
            "keyword": {
              "type": "keyword",
              "ignore_above": 256
            }
          }
        },
        "aggregation_field": {
          "type": "keyword"
        },
        "location": {
          "type": "geo_point"
        }
      }
    }
  }
}
```

通过修改 Field 类型, 不使用过大 Field 类型的方式, 相同数据最终使用存储 3.41 GB, 节省了 0.8 GB。

### Compression 优化

将 Warm 数据的压缩算法设置为 `DEFLATE` 来降低这部分数据的磁盘占用, 对于访问频繁的数据还是使用默认的 `LZ4` 算法压缩, 当 Index 转移为 Warm 时, 通过 `Reindex` 并设置别名(alias) 修改压缩方式(由于压缩算法是 Index 的 static setting, 无法在 Index 存在过程中动态修改, 只能通过 `Reindex` 来修改, 通过设置别名来避免对业务系统的影响, 由于 Index 是按时间创建的, `Reindex` 过程不会丢失数据)。

通过验证, 18595335 条数据在两种压缩算法上, 效率相差不大, 大致 $20ms$ 左右。 但完整的 2.7 亿多条数据, 这两种压缩算法的查询效率还需再验证。 最终 warm 数据的压缩算法选择必然是 `DEFLOTE`, 只是需要选择 hot 数据的压缩算法, 同时压缩算法的选择决定了是否需要 Reindex。

## 集群构建

建立 ES 集群时, 注意需要修改几项系统配置: 最大打开文件数, swap 分区, 内存限制等。 同时注意根据系统信息设置合理的 JVM 参数。
