# hive note

1. beeline 连接 hive:  

```
beeline -u jdbc:hive2://adm-test:10000/default -n hive
```

2. hiveserver2 ha

hiveserver2 ha 通过在 Zookeeper 中的 Namespace 实现。通过连接 Zookeeper 后，zookeeper 来完成 hiveserver2 的连接。
  - 当正在执行 query 的过程中, 某个 hiveserver2 挂掉, query 执行状况为:

3. 
