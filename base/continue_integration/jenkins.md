# jenkins

## 概述

Jenkins是一个开源软件项目, 是基于Java开发的一种持续集成工具, 用于监控持续重复的工作, 旨在提供一个开放易用的软件平台, 提供软件持续集成的功能。

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
  2. `content path`: tomcat 的发布路径, 即通过 ` ` 来访问项目。
  3. `deploy on failure`: 是发生错误的时候是否发布到 tomcat。  

 - 注意: 要在 `tomcat-users` 里的用户赋予 `manager-gui`, `manager-script`, `manager-jmx`, `manager-status` 这些权限。

7. SonarQube Scanner for Jenkins

jenkins 负责代码审查的插件
