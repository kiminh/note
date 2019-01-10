在数据量少的情况下, 调整都比较快。 数据量大, 一般倾向于先建新表, 入数据, 再删旧表。
理想的字段顺序是:
  1. 简单的, 短值字段在前, 复杂的, 非结构化的长值字段在后(如BLOB字段通常放到最后)。
  2. 带索引的字段在前, 不带索引的字段在后。
  3. 常读取的字段在前, 不常读取的字段在后。
  4. 主键字段在前, 非主键字段在后。
  5. 复合主键字段顺序与表中字段顺序一致。

graph DB:
  1. Titan DB

1. Java 类型与数据库字段类型对照表

数据库类型 | Java类型
:-:|:-:
`INTEGER` | `int` <br> `java.lang.Integer`
`BIGINT` | `long` <br> `java.lang.Long`
`SMALLINT` | `short` <br> `java.lang.Short`
`FLOAT` | `float` <br> `java.lang.Float`
`DOUBLE` | `double` <br> `java.lang.Double`
`NUMERIC` | `java.math.BigDecimal`
`CHAR` | `java.lang.String`
`VARCHAR` | `java.lang.String`
`TINYINT` | `byte` <br> `java.lang.Byte`
`BIT` | `boolean` <br> `java.lang.Boolean`
`DATE` | `java.util.Date` <br> `java.sql.Date`
`TIME` | `java.util.Date` <br> `java.sql.Time`
`TIMESTAMP` | `java.util.Date` <br> `java.sql.Timestamp`
`TIMESTAMP` | `java.util.Calendar`
`DATE` | `java.util.Calendar`
`VARBINARY` <br> `BLOB` | `byte[]`
`CLOB` | `java.lang.String`
`VARBINARY` <br> `BLOB` | any Java class that implements `java.io.Serializable`
`CLOB` | `java.sql.Clob`
`BLOB` | `java.sql.Blob`