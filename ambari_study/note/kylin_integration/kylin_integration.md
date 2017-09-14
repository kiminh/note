# kylin integration

## kylin deploy

- 步骤: 
    1. 下载和集群环境对应的二进制文件包, 解压到指定路径。并修改所有者。(在 HDP 集群中新增 kylin user, 并追 kylin user 加到 hadoop group 中)
    2. 配置环境变量。(包括 Java 环境, Tomcat 环境。PS: Tomcat 环境变量即配置 ```CATALINA_HOME```)
    3. 下载 ojdbc6.jar 到 /usr/share/java。(PS: hbase 的 ojdbc 是软连接, 链接到 /usr/share/java 路径的 ojdbc6.jar)
    4. kylin.properties 新增配置
```
kylin.job.jar=<kylin_install_dir>/lib/kylin-job-2.0.0.jar
kylin.coprocessor.local.jar=<kylin_install_dir>/lib/kylin-coprocessor-2.0.0.jar
kylin.rest.server=<hostname>:7070
kylin.hdfs.working.dir=/kylin
kylin.job.hive.database.for.intermediatetable=root
kylin.job.mr.lib.jar=<kylin_install_dir>/lib/
```
    5. log 配置(主要是 log 路径配置, ```<install_dir>/conf/kylin-server-log4j.properties```, 配置 log 路径为 ```/var/log/kylin```)


2. 配置kylin.properties 
3. /etc/profile
    - 配置kylin安装包中的 tomcat 的 CATALINA 安装目录
    - JAVA、hadoop、hbase、hive、hcatalog 等的安装目录及配置文件路径
4. 去除/tomcat/server.xml文件部分内容
5. 导入部分mysql的jar包
依赖保证集群上hive  hbase可用，运行kylin.sh的用户有对hive和hbase的操作权限
http://blog.csdn.net/vivismilecs/article/details/72763665
6. 运行bin/check-env.sh检查Kylin运行依赖环境（没多大用处，一般有问题也检查不出来）
运行bin/kylin.sh start启动kylin
访问kylin web,http://IP:7070/kylin
//kylin安装包中的tomcat的conf文件夹下的web-server.xml文件中定义了kylin的web访问端口，保证此端口号与kylin.properties中的访问端口号一致。
运行bin/kylin.sh stop停止kylin服务
