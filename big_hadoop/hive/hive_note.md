# hive note

1. beeline 连接 hive:  

```
beeline -u jdbc:hive2://adm-test:10000/default -n hive
```

2. hiveserver2 ha

hiveserver2 ha 通过在 Zookeeper 中的 Namespace 实现。通过连接 Zookeeper 后，zookeeper 来完成 hiveserver2 的连接。
  - 当正在执行 query 的过程中, 某个 hiveserver2 挂掉, query 执行状况为:

  - hive-JIRA hive-8376, 0.14 fix。 驱逐 instance 不是用 kill -9, 新的任务不会分配到这个 instance。(动态发现)

3. beeline 连接 hive, 在启动 Kerberos 的状态下, 首先需要通过 ```kinit``` 认证, 通过 beeline 连接时, 相对普通连接方式需要增加 ```principal=<principal>```

4. Kerberos 认证, 使用 pyhs2: [Python connect to Hive use pyhs2 and Kerberos authentication](https://stackoverflow.com/questions/29814207/python-connect-to-hive-use-pyhs2-and-kerberos-authentication)

```
conn_config = {'krb_host': 'master1.hadoop.spacestar.com', 'krb_service': 'hive'}

pyhs2.connect(host='master3.hadoop.spacestar.com',
                   port=10000,
                   authMechanism="KERBEROS",
                   password="something",
                   user='hive/master3.hadoop.spacestar.com@PCDS.COM')
```
