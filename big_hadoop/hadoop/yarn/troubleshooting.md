# troubleshooting

### yarn 界面无法显示执行的 MapReduce 日志

- 问题描述
  > 在启用 Ranger 管理 HDFS 权限后, yarn 读取 MapReduce 执行日志失败, 报错为 dr.who 五访问权限

- 引起原因
  > 在网页界面访问数据使用的用户名。 默认值是一个不真实存在的用户(dr.who). 此用户权限很小, 不能访问不同用户的数据。
  > 可以配置拥有较高权限的用户, 但会导致能够登陆页面的用户看到其他用户的数据。

- 解决方案
  > 修改 Hadoop 配置文件 core-site.xml, 增加条目, 根据实际情况进行配置, 例子如下:

```xml
<property>
  <name>hadoop.http.staticuser.user</name>
  <value>yarn</value>
  <!-- value 中填写实际运行 yarn 的操作系统用户 -->
</property>
```
