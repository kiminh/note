# postgreSQL note

## install

```bash
# install repos
yum install https://download.postgresql.org/pub/repos/yum/9.3/redhat/rhel-6-x86_64/pgdg-redhat93-9.3-2.noarch.rpm
```

```bash
# install
yum install postgresql93
yum install postgresql93-server
```

```bash
# first start
service postgresql-9.3 initdb
chkconfig postgresql-9.3 on
service postgresql-9.3 start
```

## usage

1. 备份数据库

将数据库内的数据导出到 SQL 文件:  

```shell
pg_dump -U <user> <datanase_name> > <file_path>
```

2. 将 SQL 文件导入数据库:  

```SQL
CREATE database <database_name>;
```

```bash
psql -U <user> <SQL_file_with_path>
```

3. 用户和数据库建立完成, 权限分配完指令执行后, 需要将 data reload 来完成分配权限。

```bash
pg_ctl reload -D /var/lib/pgsql/data
```

4. 简单指令

指令 | 含义
---|---
\l | 查看所有数据库的列表, 相当于 ```show databases;```
\d | 查看某个数据库中的表, 相当于 ```show tables;```
\q | 退出数据库, 相当与 ```exit```
\c | 使用某个数据库, 相当于 ```use <database>;```
\du | 查看数据库的所有用户, 相当于 ``` ```
\d <table_name> | 查看 <table_name> 的表结构

5. SQL
