# MySQL  

Mysql 一种关系型数据库管理系统, 在WEB应用方面 MySQL 是最好的  RDBMS (Relational Database Management System : 关系数据库管理系统) 应用软件之一。

## base  

1. 安装  

linux 下推荐使用 rpm 安装 MySQL , 包括 MySQL , MySQL-client , MySQL-devel , MySQL-shared , MySQL-bench 。

```bash
yum install mysql mysql-devel mysql-client -y

# 启动
service mysqld start

# mac
mysql.server start|stop
```

通过 rpm 安装 mysql 5.5, 5.6, 5.7:

```bash
yum install -y http://repo.mysql.com/mysql-community-release-el6-5.noarch.rpm
# 根据需要的 mysql 版本修改 /etc/yum.repos.d/mysql-community.repo 中的内容, 即选择启用的 yum 源。
yum install -y mysql-community-server mysql-community-client mysql-community-lib
```

2. 安装后需要完成的工作

MySQL 安装后, 默认密码为空, 首先需要做的事为 MySQL 创建密码:

```
mysqladmin -u root password "new_password";
```

通过以下指令连接 MySQL:

```
mysql -u root -p

# 命令行输出: Enter Password:
# 输入密码连接到MySQL
```

3. 查看 MySQL 实例进程

```
# 通过 ps 指令查看进程
ps -aux | grep mysql
```

4. 用户设置

通过 root 用户设置其他用户。

    1. 创建用户

```sql
# 通配符为 %
# 创建一个名为 user 的用户, 所有 IP 能访问, 未设置密码
CREATE USER 'user'@'%';
# 创建一个名为 user 的用户, 指定 IP 能访问, 密码为 password
CREATE USER 'user'@'192.168.1.101' IDENDIFIED BY 'password';
# 创建一个名为 user 的用户, 所有 IP 能访问, 密码为 password
CREATE USER 'user'@'%' IDENTIFIED BY 'password';
# 创建一个名为 user 的用户, 所有 IP 能访问, 无密码
CREATE USER 'user'@'%' IDENTIFIED BY '';
```

    2. 为数据库添加用户:

```sql
# 通过 root 用户对数据库进行添加用户
use mysql;
INSERT INTO user
    (host, user, password,
    select_priv, select_priv, select_priv)
    VALUES ('localhost', 'guest',
    PASSWORD('password'), 'Y', 'Y', 'Y');

# 在添加用户时, 注意使用MySQL提供的 PASSWORD() 函数来对密码进行加密。
# 在 MySQL5.7 中 user 表的 password 已换成了 authentication_string 。
# 可以在创建用户时, 为用户指定权限, 在对应的权限列中, 在插入语句中设置为 'Y' 即可

# or
GRANT SELECT,INSERT,UPDATE,DELETE,CREATE,DROP
    ON mysql.*
    TO 'user'@'%'
    IDENTIFIED BY 'password';

# 需要执行 FLUSH PRIVILEGES 语句, 执行后会重新载入授权表。
```

    3. 修改 root 用户密码:

```sql
update user set password=password('123456') where user='root';
flush privileges;
```

5. /etc/my.cnf 文件配置

在配置文件中, 可以指定不同的错误日志文件存放的目录, 一般你不需要改动这些配置。该文件默认配置如下：

```
[mysqld]
datadir=/var/lib/mysql
socket=/var/lib/mysql/mysql.sock

[mysql.server]
user=mysql
basedir=/var/lib

[safe_mysqld]
err-log=/var/log/mysqld.log
pid-file=/var/run/mysqld/mysqld.pid
```
6. 管理命令

    1. use database_name; 使用 database_name 这个数据库。
    2. show databases; 查看所有数据库。
    3. show tables; 查看当前数据库的所有表。
    4. show columns from table_name; 查看 table_name 的表属性, 属性类型, 主键信息, 是否为 NULL, 默认值等其他信息 。
    5. show index from table_name; 显示数据表的详细索引信息，包括PRIMARY KEY (主键) 。
    6. show table status link  [form db_name] [like 'pattern'] \G; 输出 MySQL 数据库管理系统的性能及统计信息。

```sql
# show table status example:
SHOW TABLE STATUS  FROM RUNOOB;   # 显示数据库 RUNOOB 中所有表的信息
SHOW TABLE STATUS from RUNOOB LIKE 'runoob%';     # 表名以runoob开头的表的信息
SHOW TABLE STATUS from RUNOOB LIKE 'runoob%'\G;   # 加上 \G，查询结果按列打印
```

## 数据类型

MySQL支持多种类型, 大致可以分为三类: 数值、日期/时间 和 字符串(字符)类型。

1. 数值类型

类型 | 大小 | 范围(有符号) | 范围(无符号) | 用途  
---|---|---|---|---  
TINYINT | 1 字节 | (-128, 127) | (0, 255) | 小整数值  
SMALLINT | 2 字节 |  | (0, 65535) | 大整数值  
MEDIUMINT | 3 字节 |  | (0, 16 777 215) | 大整数值  
INT/INTEGER | 4 字节 |  | (0, 4 294 967 295) | 大整数值  
BIGINT | 8 字节 |  | (0, 18 446 744 073 709 551 615) | 极大整数值  
FLOAT | 4 字节 |  | 0，(1.175 494 351 E-38，3.402 823 466 E+38) | 单精度浮点数值  
DOUBLE | 8 字节 |  | 0，(2.225 073 858 507 201 4 E-308，1.797 693 134 862 315 7 E+308) | 双精度浮点数值  
DECIMAL | DECIMAL(M,D) |  | 依赖于M和D的值 | 小数值  

2. 日期和时间类型  

类型 | 大小 | 范围 | 格式 | 用途  
---|---|---|---|---  
DATE | 3 | 1000-01-01/9999-12-31 | YYYY-MM-DD | 日期值  
TIME | 3 | '-838:59:59'/'838:59:59' | HH:MM:SS | 时间值或持续时间
YEAR | 1 | 1901/2155 | YYYY | 年份值
DATETIME | 8 | 1000-01-01 00:00:00/9999-12-31 23:59:59 | YYYY-MM-DD HH:MM:SS | 混合日期和时间值
TIMESTAMP | 4 | 1970-01-01 00:00:00/2037 年某时 | YYYYMMDD HHMMSS | 混合日期和时间值, 时间戳

3. 字符串类型

类型 | 大小 | 用途  
---|---|---  
CHAR | 0-255字节 | 定长字符串
VARCHAR | 0-65535 字节 | 变长字符串
TINYBLOB | 0-255字节 | 不超过 255 个字符的二进制字符串
TINYTEXT | 0-255字节 | 短文本字符串
BLOB | 0-65 535字节 | 二进制形式的长文本数据
TEXT | 0-65 535字节 | 长文本数据
MEDIUMBLOB | 0-16 777 215字节 | 二进制形式的中等长度文本数据
MEDIUMTEXT | 0-16 777 215字节 | 中等长度文本数据
LONGBLOB | 0-4 294 967 295字节 | 二进制形式的极大文本数据
LONGTEXT | 0-4 294 967 295字节 | 极大文本数据

    1. CHAR 和 VARCHAR 类型类似, 但它们保存和检索的方式不同。它们的最大长度和是否尾部空格被保留等方面也不同。在存储或检索过程中不进行大小写转换。
    2. BINARY 和 VARBINARY 类类似于 CHAR 和 VARCHAR, 不同的是它们包含二进制字符串而不要非二进制字符串。也就是说, 它们包含字节字符串而不是字符字符串。这说明它们没有字符集, 并且排序和比较基于列值字节的数值值。
    3. BLOB 是一个二进制大对象, 可以容纳可变数量的数据。有 4 种 BLOB 类型: TINYBLOB, BLOB, MEDIUMBLOB 和 LONGBLOB 。它们只是可容纳值的最大长度不同。
    4. 有 4 种 TEXT 类型: TINYTEXT, TEXT, MEDIUMTEXT 和 LONGTEXT 。这些对应 4 种 BLOB 类型, 有相同的最大长度和存储需求。

## 创建数据库表

```sql
# CREATE TABLE table_name (column_name column_type);

CREATE TABLE IF NOT EXISTS `table_name`(
   `id` INT UNSIGNED AUTO_INCREMENT,
   `title` VARCHAR(100) NOT NULL,
   `author` VARCHAR(40) NOT NULL,
   `date` DATE,
   PRIMARY KEY ( `runoob_id` )
)ENGINE=InnoDB DEFAULT CHARSET=utf8;
# 如果需要字段不为 NULL 可以设置字段的属性为 NOT NULL。
# AUTO_INCREMENT 定义列为自增的属性, 一般用于主键, 数值会自动加1。
# PRIMARY KEY 关键字用于定义列为主键。可以使用多列来定义主键，列间以逗号分隔。
# ENGINE 设置存储引擎, CHARSET 设置编码。

# example
CREATE TABLE table_name(
   id INT NOT NULL AUTO_INCREMENT,
   title VARCHAR(100) NOT NULL,
   author VARCHAR(40) NOT NULL,
   submission_date DATE,
   PRIMARY KEY ( id )
   )ENGINE=InnoDB DEFAULT CHARSET=utf8;
```

## mysql 数据备份和导入

1. 备份
```bash
musqldump -u <user> <database> > <filename>
```

2. 导入
```bash
mysql -u <user> -p <database> < <filename>
```

## 删除操作

1. 删除数据库

```sql
drop database database_name;

drop database if exists database_name;
# 删除一个不确定存在的数据库
```

2. 删除表  

```sql
DROP TABLE table_name ;

DROP TABLE if exists table_name ;
# 删除一个不确定存在的表
```

3. 删除表中的数据

```sql
DELETE FROM table_name [WHERE Clause];
# 如果没有指定 WHERE 子句, MySQL 表中的所有记录将被删除。
# 可以在 WHERE 子句中指定任何条件。

# example
DELETE FROM table_name WHERE id=3;
# 删除 table_name 表中 id 为 3 的记录
DELETE FROM table_name WHERE id>3;
# 删除 table_name 表中 id 大于 3 的记录
```

## 插入数据  

通过 INSERT INTO 语句插入数据。

```sql
insert into table_name (field1, field2 ...)
    values
    (value1, value2 ...);

# example
insert into table_name (id, name, score)
    values
    (1, "Tom", "98");
```

## 查询语句

使用 SELECT 语句查询数据:

```sql
select clone_name1, clone_name2
    from table_name
    [where clause] [offset M] [limit N];
# 可以查询一条或多条数据
# 可以使用通配符 * 代表所有
# 可以使用 where 语句增加限制条件
# 可以通过 offset 指定偏移量, 默认为 0
# 可以通过 limit 属性设定返回的记录数

# example
select * from table_name;
# 查询 table_name 的所有字段的的所有数据
select id from table_name;
# 查询 table_name 的 id 字段的的所有数据
select * from table_name where id > 4;
# 查询 table_name 的所有字段的的所有数据 id > 4 的数据
```

## 更新数据  

通过 update 语法更新数据  

```sql
update table_name set field1=new_data1, field2=new_data2
    [where clause];
```

##
