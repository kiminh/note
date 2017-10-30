# jenkins

## 概述

Jenkins是一个开源软件项目, 是基于Java开发的一种持续集成工具, 用于监控持续重复的工作, 旨在提供一个开放易用的软件平台, 提供软件持续集成的功能。

- 参考

https://www.ibm.com/developerworks/cn/java/j-lo-jenkins-plugin/


编写 Jenkins 插件需要引入 Jenkins 的 maven 仓库, 在 pom.xml 中配置。

- 功能
  1. 持续的软件版本发布 / 测试项目。
  2. 监控外部调用执行的工作。

- 安装

可以直接下载 jenkins.war 将其部署在 Tomcat 这类容器中, 也可以通过 官方提供的不通发行版的系统的安装包进行安装部署。

  1. 获取 Jenkins 源: `sudo wget -O /etc/yum.repos.d/jenkins.repo http://jenkins-ci.org/redhat/jenkins.repo`
  2. 导入 key: 'sudo rpm --import http://pkg.jenkins-ci.org/redhat/jenkins-ci.org.key'
  3. 执行安装指令: `yum install jenkins`
    1. jenkins.war 路径: `/usr/lib/jenkins/`
    2. 配置文件路径: `/etc/sysconfig/jenkins`
    3. ps: 最重要的是需要指定 java 环境
  4. 启动指令: `service jenkins start`,
  可以设置成开机自启动。

- 配置
默认已经安装 java 环境以及 maven 环境(maven 环境对 Jenkins 本身的部署没有影响)

  1. 配置 java 环境: `JENKINS_JAVA_CMD="<java_home>/bin/java`, 即 java 指令的完整路径。
  2. 配置开放端口: `JENKINS_PORT="port"`, 配置服务发布到的端口。
  3. 其余参数配置请查看 `/etc/sysconfig/jenkins` 文件中注释。

```grove
env.JAVA_HOME="${tool 'java'}"
env.PATH="${env.JAVA_HOME}/bin:${env.PATH}"
```

- project 相关
  1. 创建 project
  2. 配置 project
    1. 配置 project 自动构建过程
    2. 配置 定时构建
    3. 配置 定时检查远程仓库变化构建

- sonar runner 构建配置


## 插件及相关使用

1. pipeline / Pipeline: GitHub Groovy Libraries / Pipeline: Stage Step

2. GitHub Plugin / GitHub Branch Source Plugin / Branch API Plugin

这些插件主要是版本控制相关的插件

3. SSH Plugin / SSL Slave Plugin

集群的 jenkins 需要使用到的插件。

4. Maven Integration plugin

通过 maven 构建项目的插件, 简化 Jenkins 通过 maven 构建项目的步骤。

5. Email Extension Plugin

邮箱提示插件, 配置构建后结果的提示。

6. Deploy to container Plugin

jenkins 负责发布的插件。

配置项:

  1. `WAR/EAR files`: 配置 war 包或 ear 包编译后的路径。
  2. `content path`: tomcat 的路径, 即 tomcat 在目标服务器的 tomcat 的路径。
  3. `deploy on failure`: 是发生错误的时候是否发布到 tomcat。  

 - 注意: 要在 `tomcat-users` 里的用户赋予 `manager-gui`, `manager-script`, `manager-jmx`, `manager-status` 这些权限。即修改 `<tomcat_path>/conf/tomcat-users.xml` 文件中配置 tomcat 的用户名和密码。

7. SonarQube Scanner for Jenkins

jenkins 负责代码审查的插件

## deploy 注意问题

1. 部署好执行启动命令或运行启动脚本后, 完成部署过程, 在服务器环境中无法查看到通过 Jenkins 启动的进程。
  思路:
  1. 由于 Jenkins 启动没有出错, 在服务器中启动能够成功, 因此需要排查 Jenkins 是否启动服务。  
  2. 判断方式为: 在 deploy 阶段, 启动 Tomcat 后查看其进程, 结果是 Tomcat 进程存在。  
  由以上过程判断未启动进程的原因为 Jenkins 将其启动的进程 kill。
