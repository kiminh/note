# ES Mapping Type

- [Mapping 介绍](#mapping-介绍)
- [Mapping Type](#mapping-type)
  - [Core Data Type](#core-data-type)
    - [字符串类型](#字符串类型)
    - [数值类型](#数值类型)
    - [时间类型](#时间类型)
    - [布尔类型](#布尔类型)
    - [二进制类型](#二进制类型)
    - [Range 类型](#range-类型)
  - [Complex Data Type](#complex-data-type)
  - [Geo Data Type](#geo-data-type)
  - [Specialised Data Type](#specialised-data-type)
  - [Multi-fields](#multi-fields)

## Mapping 介绍

## Mapping Type

ES 在 Document 中提供了一系列不同的 field 类型, 详见 [Field datatypes](https://www.elastic.co/guide/en/elasticsearch/reference/current/mapping-types.html)。

### Core Data Type

#### 字符串类型

- [text 类型](https://www.elastic.co/guide/en/elasticsearch/reference/5.5/text.html)

  - index 的 full-text 字段。 在索引之前通过[分析器](https://www.elastic.co/guide/en/elasticsearch/reference/5.5/analysis.html)对讲字符串转换为单个术语列表的方式对这些字段进行分析。 分析过程允许 ES 在每个 full-text 字段中搜索单个单词。 text 字段不用于聚合([significant terms aggregation](https://www.elastic.co/guide/en/elasticsearch/reference/5.5/search-aggregations-bucket-significantterms-aggregation.html)是个例外)。

  - text 字段参数

参数 | 描述
---|---
analyzer | 分析器应该用于在索引时和搜索时分析字符串字段(除非被search_analyzer覆盖)。默认为默认索引分析器或标准分析器。
boost |
eager_global_ordinals |
fielddata |
fielddata_frequency_filter |
fields |
include_in_all |
index |
index_options |
norms |
position_increment_gap |
store |
search_analyzer |
search_quote_analyzer |
similarity |
term_vectory |

> 从 ES 2.x 导入的 index 不支持 text, 会尝试将 `text` 降级为 `string`, 因此可以将 `modern mappings` 与 `legacy mappings` 合并。 在升级到 6.x 前, 除了按照特定计划重建 index 关系外, 必须重建长期存在的 index。

- [keyword](https://www.elastic.co/guide/en/elasticsearch/reference/5.5/keyword.html)

  - 索引结构化内容, 如: email, hostname, 状态码, 邮编或标记等字段。 这些字段通常用于筛选, 排序, 聚合。 keyword 字段只能根据它们的准确值进行搜索。 如需搜索索引全文内容, 使用 `text field`。
  - keyword 字段参数

参数 | 描述
---|---
boost | 映射字段级查询时间增加, 接受浮点数, 默认 1.0。
doc_values |
eager_global_ordinals |
fields |
ignore_above |
include_in_all |
index |
index_options |
norms |
null_value |
store |
similarity |
normalizer |

```text
PUT my_index
{
  "mappings": {
    "my_type": {
      "properties": {
        "tags": {
          "type":  "keyword"
        }
      }
    }
  }
}
```

#### 数值类型

- long: $-2^{63}$ ~ $2^{63}-1$
- integer: $-2^{31}$ ~ $2^{31}-1$
- short: -32768 ~ 32767
- byte: -128 ~ 127
- double: 双精度 64 位 `IEEE 754` 浮点数。
- float: 单精度 64 位 `IEEE 754` 浮点数。
- half_float: 半精度 64 位 `IEEE 754` 浮点数。
- scaled_float: 由一个长而固定的比例因子支持的浮点数。

#### 时间类型

#### 布尔类型

#### 二进制类型

#### Range 类型

### Complex Data Type

- 数组类型

- 对象类型

- 嵌套类型

### Geo Data Type

提供位置数据支持。

- Geo-point 类型

- Geo-shape 类型

### Specialised Data Type

一些专用类型的支持。

- IP 类型: IPv4, IPv6 地址

- Completion 类型: completion to provide auto-complete suggestions

- mapper-murmur3: 计算索引时值的 Hash 值并将其存储在索引中。

- Attachment datatype: 插件提供, 查看 [mapper-attachments](https://www.elastic.co/guide/en/elasticsearch/plugins/5.2/mapper-attachments.html)

- Percolator 类型

### Multi-fields
