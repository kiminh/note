```sql
-- 创建外部分区表
create external table tsdb_aqi
              (
              metric string,
              mn string,
              md string,
              mp string,
              mp_type string,
              ec string,
              value double,
              sample_time timestamp,
              receive_time timestamp,
              create_time timestamp,
              other_info string
              ) partitioned by (year string, month string, day string) row format delimited fields terminated by '\|';

-- 关联外部分区路径、文件
alter table tsdb.tsdb_aqi add partition(year=2019, month=01, day=24) location "hdfs://dscluster/user/dse/data/tsdb_data/2019/01/24"
```