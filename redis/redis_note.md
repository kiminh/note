# Redis  

## 简介

## 集群环境搭建  

参见 [Redis集群搭建与简单使用](http://www.cnblogs.com/wuxl360/p/5920330.html)

## 配置文件说明  

- redis.conf

```conf
# Redis 是否以守护进程 的方式运行
daemonize no

# 当 Redis 以守护进程的方式运行时，Redis 默认会把 pid 文件放到 /var/run/redis.pid, 可以配置其他地址
# 当运行多个 redis 实例时，需要指定不同的 pid 文件和端口
pidfile /var/run/redis.pid

# 连接端口
port 6379

# 指定 Redis 可以接收请求的地址, 不设置将处理所有请求
bind 127.0.0.1

# 客户端连接超时时间, 单位: s, 超时后关闭连接( 0 为 disable 该选项)
timeout 0

# 配置 log 等级 (debug, verbose, notice, warning)
loglevel notice

# 配置 log 文件地址, 缺省打印在命令行终端的窗口上
logfile stdout

# 设置数据库的个数, 可以使用 SELECT <dbid> 命令来切换数据库, 缺省使用的数据库是 0
databases 16

######## SNAPSHOTTING ########

# 设置 Redis 进行持久化
# 900 秒内有 1 个 key 发生变化时
# 300 秒内有 10 个 keys 发生变化时
# 60 秒内有 10000 个 keys 发生变化时
save 900 1
save 300 10
save 60 10000

# 持久化是否进行压缩
rdbcompression yes

# 持久化文件的文件名
dbfilename dump.rdb

# 持久化文件存储路径
dir ./

######## REPLICATION ########

# 设置该数据库为其他数据库的从数据库
# slaveof <masterip> <masterport>

# 指定与主数据库连接时需要密码验证
# masterauth <master-password>

# slave 向 master 发送 PINGS 的周期, 缺省周期为 10s
# repl-ping-slave-period 10

# master 响应时间, 缺省 60s
# repl-timeout 60

######## SECURITY ########

# 设置客户端连接后进行任何其他指定前需要使用密码
# requirepass foobared
# 限制同时连接的客户端数量
# maxclients 128

# 设置 Redis 能够使用的最大内存, 当内存满了还借都 set 指令的时候, Redis 先尝试剔除设置过 expire 信息的 key(无论 key 是否过期)
# 先剔除最早要过期的 key , 当所有 expire key 被删完时, 将返回错误。
# 此时 Redis 只接受 GET 请求, 不接受 SET 请求。
# 该配置能够将 Redis 当作类似于 memcached 的缓存来使用
# maxmemory <bytes>


```

- 集群配置相关  

```conf
cluster-enabled yes
cluster-config-file <filename>
cluster-node-timeout <time>
appendonly yes
```

## 创建集群相关  

- 依赖  

    1. ruby  
    2. gem
    3. opebssl
    4. ruby redis 接口

- 集群环境搭建  

    1. 启动多个 redis 实例
    2. 执行 ```ruby redis-trib.rb create --replicas 1 <host:port>```

- 注意:  

    1. ruby Redis 接口需要手动下载，通过 ```gem install redis``` 下载。  
    2. ```replicas 1``` 选项表示为集群中的每个主节点创建一个从节点。  
