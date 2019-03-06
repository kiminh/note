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

2. GitHub Plugin / GitHub Branch Source Plugin / Branch API Plugin, 这些插件主要是版本控制相关的插件

3. SSH Plugin / SSL Slave Plugin, 集群的 jenkins 需要使用到的插件。

4. Maven Integration plugin, 通过 maven 构建项目的插件, 简化 Jenkins 通过 maven 构建项目的步骤。

5. Email Extension Plugin, 邮箱提示插件, 配置构建后结果的提示。

6. Deploy to container Plugin, jenkins 负责发布的插件。 配置项:
   1. `WAR/EAR files`: 配置 war 包或 ear 包编译后的路径。
   2. `content path`: tomcat 的路径, 即 tomcat 在目标服务器的 tomcat 的路径。
   3. `deploy on failure`: 是发生错误的时候是否发布到 tomcat。
   - 注意: 要在 `tomcat-users` 里的用户赋予 `manager-gui`, `manager-script`, `manager-jmx`, `manager-status` 这些权限。即修改 `<tomcat_path>/conf/tomcat-users.xml` 文件中配置 tomcat 的用户名和密码。

7. SonarQube Scanner for Jenkins, jenkins 负责代码审查的插件

8. MultiJob plugin

https://stackoverflow.com/questions/25929774/jenkins-sharing-variables-in-multijob
https://wiki.jenkins.io/display/JENKINS/Parameterized+Build

不能很好的和 pipeline plugin 兼容。

配置 job retry 规则: 在全局配置中配置 job 重试规则

配置job:

- 需要首先配置阶段, 可以配置多个阶段, 每个阶段名称自定义；
- 对每个阶段需要配置 job, job 可以对应自工程 job。
- 根据 `BUILD_ID` 来管理 job

待验证:

1. `each phase job has a few env variables added on the multi job level. For the case in the question it would be sufficient to pass a value of BUILD_JOB_BUILD_ID (or BUILD_JOB_BUILD_NUMBER, don't remember exactly) environment variable to the Install Job`

request

- Token Macro Plugin v.1.10 (required) 1
- Maven Integration plugin v.2.6 (required) 1
- built-on-column v.1.1 (required) 1
- Conditional BuildStep v.1.3.3 (required) 1
- Jenkins Parameterized Trigger plugin v.2.25 (required)
- Environment Injector Plugin v.1.90 (required)1
- Copy Artifact Plugin v.1.31 (optional)
- Matrix Project Plugin v.1.7.1 (optional) 1

9. GitLab Hook Plugin

## usage

1. conditional steps(signal/multiple)
  1. conditional steps(signal)
    1. 配置每个 step 构建某一个子工程, 一个构建全部工程, 覆盖全部工程。
    2. 手动选择某个项目进行构建的条件。
    3. 配置结束构建后需要进行的操作。
    4. run 可选项及说明:
      1. 触发器支持以下方式触发:
        - UserCause - 构建是由手动交互式触发
        - SCMTrigger - 构建是由 SCM 更改触发
        - TimerTrigger - 构建是通过定时器触发
        - CLICause - 构建是通过 CLI 接口触发
        - RemoteCause - 构建是通过远程接口触发
        - UpstreamCause - 构建是由上游项目触发
      2. 如果安装了 XTrigger 插件, 还支持以下触发方式:
        - FSTrigger - 构建是由文件系统更改触发 (FSTrigger Plugin)
        - URLTrigger - 构建是由 URL 更改触发 (URLTrigger Plugin)
        - IvyTrigger - 构建是由 Ivy 依赖关系版本触发 (IvyTrigger Plugin)
        - ScriptTrigger - 构建由脚本触发 (ScriptTrigger Plugin)
        - BuildResultTrigger - 构建是由其他作业的结果触发 (BuildResultTrigger Plugin)

2. set build status to "pending" on GitHub commit

3. Trigger/call builds on other projects

4. Invoke top-level Maven targets
  1. 指定在 jenkins 中配置的 maven 环境变量
  2. 指定 `pom.xml` 文件在 project 中的位置(path), 可以指定子工程的 `pom.xml` 文件。
  3. goals: 配置 maven 执行的 goals, 如: `clean, install` 等
  4. Properties 指定 maven 构建的参数, 如 `-Dmaven.test.skip=true` 等 maven 参数。
  5. JVM Options: JVM 参数配置
  6. On evaluation failure: 可选项为: `Fail the build, Mark the build unstable, Run and mark the build unstable, Run, Don't run`, 来确定该构建的执行条件或是否执行。

5. commit 构建触发(Poll SCM)

  > 定时检查源码变更 (根据SCM软件的版本号), 如果有更新就 checkout 最新代码下来, 然后执行构建动作。
  1. 触发规则: `*****`
    1. 第一个 `*` 代表分钟, 范围是 0~59
    2. 第二个 `*` 代表小时, 范围是 0~23
    3. 第三个 `*` 代表天, 范围是 0~31
    4. 第四个 `*` 代表月, 范围是 1~12
    5. 第五个 `*` 代表星期几, 范围是 0～7(0 & 7 代表星期日)
  2. eg:
    1. `H 17 * * * ` 表示一天的 17 点触发
    2. `H 8,12,15,16 * * 0-6` 一天的 8 点, 12 点, 15 点, 16 点, 每个星期的七天都触发
  3. Ignore post-commit hooks 选项
    > 对某些出现不需要构建的提交后不进行构建

6. build 构建触发

  > 周期进行项目构建 (不关心源码是否发生变化)
  1. 触发规则: 同 Poll SCM

## jenkins 默认 env 变量

```bash
# The following variables are available to shell scripts

BUILD_NUMBER
# The current build number, such as "153"
BUILD_ID
# The current build ID, identical to BUILD_NUMBER for builds created in 1.597+, but a YYYY-MM-DD_hh-mm-ss timestamp for older builds
BUILD_DISPLAY_NAME
# The display name of the current build, which is something like "#153" by default.
JOB_NAME
# Name of the project of this build, such as "foo" or "foo/bar".
JOB_BASE_NAME
# Short Name of the project of this build stripping off folder paths, such as "foo" for "bar/foo".
BUILD_TAG
# String of "jenkins-${JOB_NAME}-${BUILD_NUMBER}". All forward slashes (/) in the JOB_NAME are replaced with dashes (-). Convenient to put into a resource file, a jar file, etc for easier identification.
EXECUTOR_NUMBER
# The unique number that identifies the current executor (among executors of the same machine) that’s carrying out this build. This is the number you see in the "build executor status", except that the number starts from 0, not 1.
NODE_NAME
# Name of the agent if the build is on an agent, or "master" if run on master
NODE_LABELS
# Whitespace-separated list of labels that the node is assigned.
WORKSPACE
# The absolute path of the directory assigned to the build as a workspace.
JENKINS_HOME
# The absolute path of the directory assigned on the master node for Jenkins to store data.
JENKINS_URL
# Full URL of Jenkins, like http://server:port/jenkins/ (note: only available if Jenkins URL set in system configuration)
BUILD_URL
# Full URL of this build, like http://server:port/jenkins/job/foo/15/ (Jenkins URL must be set)
JOB_URL
# Full URL of this job, like http://server:port/jenkins/job/foo/ (Jenkins URL must be set)
GIT_COMMIT
# The commit hash being checked out.
GIT_PREVIOUS_COMMIT
# The hash of the commit last built on this branch, if any.
GIT_PREVIOUS_SUCCESSFUL_COMMIT
# The hash of the commit last successfully built on this branch, if any.
GIT_BRANCH
# The remote branch name, if any.
GIT_LOCAL_BRANCH
# The local branch name being checked out, if applicable.
GIT_URL
# The remote URL. If there are multiple, will be GIT_URL_1, GIT_URL_2, etc.
GIT_COMMITTER_NAME
# The configured Git committer name, if any.
GIT_AUTHOR_NAME
# The configured Git author name, if any.
GIT_COMMITTER_EMAIL
# The configured Git committer email, if any.
GIT_AUTHOR_EMAIL
# The configured Git author email, if any.
```

## deploy 注意问题

1. 部署好执行启动命令或运行启动脚本后, 完成部署过程, 在服务器环境中无法查看到通过 Jenkins 启动的进程。
  思路:
  1. 由于 Jenkins 启动没有出错, 在服务器中启动能够成功, 因此需要排查 Jenkins 是否启动服务。  
  2. 判断方式为: 在 deploy 阶段, 启动 Tomcat 后查看其进程, 结果是 Tomcat 进程存在。  
  由以上过程判断未启动进程的原因为 Jenkins 将其启动的进程 kill。

## 正常配置一个 project 流程

1. 配置 project name 和 project description
2. 选择构建的选项
3. 源码管理: 可选项为 git 和 Subversion
4. 配置构建方式, 根据创建的 project 类型来配置构建方式, 后续章节详细介绍
5. 配置发布方式, 可选方式较多, 后续章节介绍

## git 控制是否构建

参考: 官方 wiki: https://wiki.jenkins.io/display/JENKINS/Git+Plugin `Push notification from repository部分`
