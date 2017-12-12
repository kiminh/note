# 持续集成相关整理

## 代码管理及版本控制

- 工具选择: git
- 主要操作:
  1. `git init`: 初始化 git 仓库, 创建新的 git 仓库
  2. `git remote`: 关联仓库
  2. `git add`: 将变更提交到缓存区, 用于 commit
  3. `git commit`: 将变更提交到本地仓库
  4. `git push`: 将本地仓库内容 push 到远程仓库
  5. `git pull`: 更新本地仓库至最新改动
  6. `git branch`: 分支相关基本操作
  7. `git checkout`: 检出分支
  8. `git merge`: 合并其他分支到当前分支
  9. `git rebase`: 把一个分支的修改合并到当前分支
  10. `git clone`: clone 一个远程仓库到本地(也可以创建一个本地仓库的 clone 版)
  11. `git revert`: 是生成一个新的提交来撤销某次提交
  12. `git reset`: 回到某次提交, 提交及之前的 commit 都会被保留, 但是此次之后的修改都会被退回到暂存区

- 详解

  1. `git remote` 用法: `git remote [-v | --verbose]`
    常用: `git remote add`, `git remote remove`, `git remote rename`, `git remote -v`
    1. `remote add [-t <branch>] [-m <master>] [-f] [--tags | --no-tags] [--mirror=<fetch|push>] <local_branch> <url>`: 将本地分支关联远程分支.
      > 参数说明:
      > [-t <branch>]: 创建一个只跟踪 <branch> 的 refspec, 可以使用多个该参数来跟踪多个 <branch> 的 refspec, 无该参数表示抓取所有分支
      > [-m <master>] 指向远程的 <master> 分支
      > [-f]: 在远程信息建立之后立即进行
      > [--no-tags|--no-tags]: [迁移远程存储库中的每个标记|不迁移远程存储库中的每个标记]
      > <local_branch>： 本地分支
      > <url>: 远程仓库的 url, 通常是: http://<ip>:<port>/<user>/<project>.git
      > 补充: git 中的 refspec，是一种格式，是具体的引用的意思, 即 引用规则。
    2. `git remote rename <old> <new>`: 将名为 <old> 的本地分支重命名为 <new>, 关联的所有远程分支和配置设置都会更新。
    3. `git remote -v`: 在名称后面显示远程 url
    4. `git remote remove`: 删除本地分支和关联远程分支的配置

  2. `git add` 用法: `git add [<options>] [--] <pathspec>`
    > 补充: 这里 <pathspec> 填写具体路径, 缺省为 `.`, 即当前目录未忽略的所有内容

  3. `git commit` 用法: `git commit [<options>] [--] <pathspec>`
    常用: `git commit -m <commit_message>`: 提交到本地仓库, 同时上传提交信息
    > 补充: 这里 <pathspec> 填写具体路径, 缺省为 `.`, 即当前目录未忽略的所有内容

  4. `git push` 用法: git push `[<options>] [<repository> [<refspec>...]]`
    常用: `git push -u <local_branch> <remote_branch`, 将本地分支 push 到远程分支

  5. `git branch` 用法: `git branch [<options>] [-r | -a] [--merged | --no-merged]`
    > 常用用法:
    > `git branch <local_branch>`: 新建本地分支
    > `git branch`: 查看本地分支
    > `git branch -a`: 查看所有分支(包含本地分支和远程分支)
    > `git branch -d <local_branch>`: 删除本地分支
    > `git -u --set-upstream-to <remote_branch>`: 将本地分支重置关联到远程分支

  6. `git checkout` 用法: `git checkout [<options>] <branch>`
    > 常用用法:
    > `git checkout <local_branch>`: 切换到 <local_branch> 的本地分支
    > `git checkout -b <local_branch>`: 创建一个 <local_branch> 的本地分支并切换到 <local_branch> 分支
    > `git checkout -b <local_branch> <remote_branch>`: 创建一个本地分支关联到远程的分支

  7. `git merge` 用法: git merge [<options>] [<commit>...]
    > 1. 在工作目录中获取(fetch) 并合并到远程的改动
    > 2. 合并其他分支到当前分支
    > 以上两种情况, git 会尝试自动合并, 但并非一定能够成功, 可能导致冲突(conflicts), 经常需要手动合并这些冲突。
    > 可以通过 `git diff <source_branch> <target_branch>` 来查看两个分支的不同的部分, 使用 IDE 能够方便的处理这些问题。
    > 修改完成后需要执行 `git add <filename>` 来将其标记为合并成功(即提交到缓存区)
    > 常用用法:
    > `git merge <branch>`

  8. `git rebase` 用法: `git rebase [-i] [options] [--exec <cmd>] [--onto <newbase>] [<upstream>] [<branch>]`
    > 可以参考 [git rebase简介](http://blog.csdn.net/hudashi/article/details/7664631/)
    > 可以通过 IDE 更简洁明了的完成 `git rebase`

  9. `git clone` 用法: `git clone [<options>] [--] <repo> [<dir>]`
    > 将远程仓库 clone 到本地
    > 最后的 [<dir>] 参数为可选项, 将 clone 到本地的代码目录重命名

  10. `git revert` 用法: `git revert [<options>] <commit-ish>`
    > 撤销某次操作, 此次操作之前的 commit 及 push 都会保留。
    > 用一次新的commit来回滚之前的commit
    > <commit-ish> 为某次 commit

  11. `git reset` 用法: `git reset [--mixed | --soft | --hard | --merge | --keep] [-q] [<commit>]`
    > 参考 [git reset 简介](http://blog.csdn.net/hudashi/article/details/7664464/)
    > 文件从缓存区回退到工作区: `git reset HEAD filename`
    > 回退版本: ` git reset HEAD^`(说明: 一个 `^` 表示一个版本)

建议：结合 IDE 和 git 指令一同使用 git

## 持续集成, 自动构建

- 工具选择: Jenkins
- 该部分操作主要由项目负责人完成

### 插件介绍

  1. git client: 在 Jenkins 中配置编译环境 git 客户端路径
  2. gitlab plugin: Jenkins 的源码管理工具, 配置远程 git 仓库的插件
  3. gitlab hook plugin: 接收 gitlab 的 webhook 的插件, 这样可以不通过定时任务来更新编译环境的代码, 在提交后, 通过 git 发送编译信号到 Jenkins 来触发 Jenkins 进行编译, 部署
  4. maven plugin: 通过 maven 编译代码的插件, 同时通过该插件指定编译环境的 maven 路径
  5. publish over ssh plugin: 复制文件到工作服务器, 并执行命令

以上插件及其依赖可以完全满足 web 应用的自动化构建及部署。

#### git client

用来在 Jenkins 中配置 git 环境, 可以使用默认环境, 也可以使用自定义编译安装的 git 环境。 在触发 build 时, 通过该配置的 git 完成 clone fetch 等操作.

#### gitlab plugin

Jenkins 的源码管理插件, 用来获取工程代码。 需要配置的部分为: `Repositories` 和 `Branches to build`。 可以同时配置多个 `Repository` 和 `Branches`。 默认配置的分支为 `*/master` 分支, 即工程的主分支。

其余部分使用默认配置即可。

#### gitlab hook plugin

通过 gitlab 的提交来触发构建, 在配置的分支有提交时, 触发构建, 在其他分支有提交时, 不触发构建。 通过开发分支和发布分支分离开来实现发布分支有提交时进行自动化构建。

#### maven plugin

用来在 Jenkins 中配置 maven 环境。 在新建工程时, 可以直接建立 maven 工程, 编辑好 maven 的 `Root POM` 和 `goals and opeions` 后, 在 build 过程自动调用 maven 进行编译, 而不需要指定 maven 环境。 同时提供高级选项, 高级选项包括 `MAVEN_OPTS`, 自定义的工作空间, 选择 maven 环境, setting.xml 文件配置选择, maven 全局配置选择等。

#### publish over ssh plugin

该插件的作用时在编译完成后复制编译结果的文件到指定服务器, 完成后执行相关指令。
需要首先在 `系统管理` 中的 `系统设置` 中配置好目标服务器和远程根路径, 测试配置通过后即可使用。

### 配置工程

#### 新建工程

  1. 登陆后选择 `新建`
  2. 输入任务名称后选择 `构建一个maven项目`
  3. 下一步完成新建工程

#### 配置工程

  - 配置工程模块包含 9 个模块:
    1. General
    2. 源码管理
    3. 构建触发器
    4. 构建环境
    5. Pre Step
    6. Build
    7. Post Steps
    8. 构建设置
    9. 构建后操作

  1. General 模块配置:
    - 配置项目名称, 默认使用新建的项目配置。
    - 配置项目描述, 描述这个项目。
    - 配置 GitLab connection, 在当前环境下不用配置, 下拉框中只有一个对勾符号。
    - 丢弃旧的构建: 即构建保存时间。 即当前的构建保存的最大天数或最大个数。 高级选项中包含发布包保留天数和发布包最大保留个数。
    - 参数化构建过程: 暂时未用到。
    - 关闭构建: 选中后关闭该构建。
    - 其余选项可根据名称自行选择是否选中。

  2. 源码管理
    - 源码管理在上述 plugin 条件下只有两个选项: `None` 和 `Git`
    1. None: 即无源码管理。
    2. Git: 使用 git 作为代码管理工具。需要配置一下内容:
      1. Repositories: 代码库。 包含一下内容:
        1. Repository URL: 远程仓库地址, git clone 的地址。 可以选择多个远程仓库。
        2. Credentials: 配置证书, 即配置拥有该远程仓库相应权限的用户。
      2. Branches to build: 选择需要构建的远程分支, 配置形式: `*/<remote_branch>`, 可以选择多个远程分支。
      3. 源码库浏览器: 选择自动即可。
      4. Additional Behaviours: 配置更多的行为, 根据需要选择。 一般情况不需要选择该选项。

  3. 构建触发器
    - 构建触发器包含一下选项:
    1. Build whenever a SNAPSHOT dependency is built: 选中后  Jenkins 将检查 POM 中的 <dependency> 元素以及 POM 中使用的 <plugin> 和 <extension> 的快照依赖性。
    2. 触发远程构建(例如,使用脚本): 如果需要访问特殊的预定义 URL 来触发新的构建, 选用此选项。 需要以字符串的形式提供授权令牌。 该选项可以不选, 可以通过 gitlab hook plugin 实现类似的功能。
    3. Build after other projects are built: 在其他工程构建完成后构建。 一般不必选择该选项。
    4. Build periodically: 定期构建。 选中后填写构建日程表, Jenkins 将根据日程表对项目进行构建。
    5. Build when a change is pushed to GitLab: 当 gitlab 中有新的 commit 时构建项目, 该选项需要 gitlab 的 web hook 配合完成, 即每当 gitlab 上完成相应的操作后, 通过钩子发送一个 HTTP 请求到 Jenkins 中来触发 Jenkins 的构建。
    6. Poll SCM: Poll 日程表, 如果选择 `Build when a change is pushed to GitLab` 选项, 则该选项必须选。 日程表中可以不填写 poll 时间安排。 同时不能选中 `Ignore post-commit hooks`

  4. 构建环境
    - 构建环境统一在系统配置中设置, 在该处不进行配置。

  5. Pre Steps
    - 可以不对其进行配置,

  6. BUILD
    - 该部分是配置构建, 由于创建的是一个 maven 工程, 所以该选项会在配置项中。 该配置包含两个选项和高级选项:
    1. Root POM: 选择要构建的项目的 pom.xml 文件相对该 workspace 的路径。
    2. Goals and options: 即 maven 的	Goals and options, 如: `clean`, `install`, `package` 等。 **注意: 不要执行类似 tomcat:run, jetty:run 之类的指令!!!**
    3. 高级选项包含 maven 的选项, maven 配置选择等。

  7. Post Steps

  8. 构建设置
    - 配置邮箱提醒

  9. 构建后操作
    - 选择构建后操作步骤: 即发布过程的配置, 这里使用 `publish over ssh plugin` 这个插件, 在 `增加构建后操作步骤` 中选择 `Send build artifacts over SSH` 选项
    1. 选择配置好的 SSH server
    2. Transfers: 配置需要复制到 SSH server 的文件, 可用正则匹配, 可配置多个需要传输的文件, 可配置多个源文件和目标目录, 多个执行指令等。

  以上配置完成后, 完成 Jenkins 配置一个项目。

### 构建工程

除了自动构建, 还可以手动构建工程, 通过点击立即构建来执行构建任务(job)。 所有构建会显示在左下角的列表中, 可以进入查看编译过程, 方便检查失败的编译的错误部分。

  - job 的内容包括:
    1. 状态集: 构建的基本信息。
    2. 变更记录: 作用暂时未知, 只有一个summary, 无其他内容。
    3. Console Output: 控制台输出。
    4. 编辑编译信息: 为本次构建打一个标签和描述, 来标记本次构建。
    5. 删除本次生成: 删除本次构建。
    6. Git Build Data: 编译的代码的版本信息(即 git 的 Hash 值) 和编译的分支信息。
    7. No Tags: 为本次编译打一个 Tag, 并 commit 到代码库。
    8. Test Result: 编译时执行的测试用例情况, 针对代码中的测试用例。
    9. Redeploy Artifacts: 将编译的工程重新部署。
    10. See Fingerprints: 文件指纹档案, 包含引用的 jar。
    11. 前一次构建: 查看前一次构建的内容。
    12. 后一次生成: 查看后一次候检的内容。

  - 工作空间
    > 可以在工作空间中查看源代码和编译后生成的内容, 包括编译后生成的 jar/war 包等。
    > 工作空间的内容均为最新版本的代码及最新版本的代码的编译结果。
    > 同时提供包的编译后的内容的下载以及代码的下载。

### 系统管理

  - 系统管理包含:
    1. 系统设置: 全局设置&路径, 主要用来配置全局信息。 根据页面内容进行配置。
    2. 全局安全配置: Jenkins安全，定义谁可以访问或使用系统。
    3. Configure Credentials(配置证书): 配置凭据提供程序和类型(目前未使用)。
    4. 全局工具配置: 工具配置, 包括它们的位置和自动安装器, 包括 java 环境, maven, git 等配置在这里配置路径。
    5. 读取设置: 放弃当前内存中所有的设置信息并从配置文件中重新读取, 仅用于当您手动修改配置文件时重新读取设置。
    6. 插件管理: 添加、删除、禁用或启用Jenkins功能扩展插件。
    7. 系统信息: 显示系统环境信息以帮助解决问题。 同时可以查看配置好的环境变量, 使用的插件信息。
    8. 系统日志: Jenkins相关的日志信息。
    9. 负载统计: 检查您的资源利用情况，看看是否需要更多的计算机来帮助您构建。
    10. Jenkins CLI: 从您命令行或脚本访问或管理您的Jenkins, 需要 相应版本的 `Jenkins-cli.jar`, 执行指令为 `java -jar jenkins-cli.jar -s http://<host>:<port>/ <command>`。
    11. 脚本命令行: 执行用于管理或故障探测或诊断的任意脚本命令。 这里的脚本是 groovy 脚本。
    12. 管理节点: Jenkins 的 master-slave 模式。
    13. 关于 Jenkins: 查看版本(包括插件的版本)以及证书(开源许可)信息。
    14. Manage Old Data: 清理配置文件以从旧插件和更早版本中删除残留。
    15. 管理用户: 创建/删除/修改Jenkins用户。
    16. 准备关机: 停止执行新的构建任务以安全关闭计算机。

### 执行 deploy 需要注意的地方

  1. 如果配置了远程根目录, 默认根目录是配置的根目录, 即使是 `/`, 也是在配置的目录。
  2. 在执行脚本的时候, 写脚本的完整路径, 否则会出现找不到文件的问题。
  3. Jenkins 默认会 kill 所有由 Jenkins 启动的进程, 执行发布时, 通常需要该 程序一直运行下去, 因此需要配置 Jenkins 在执行完发布后不 kill 发布的进程。 实现方式是在执行的命令的第一行加上 `BUILD_ID=DONTKILLME`
  4. 注意在每次发布之前检查环境是否完好, 可以通过编写脚本来检查工程运行环境, 如果运行环境异常, 终止脚本执行, 并输出相关日志。

## 代码审查

- 代码审查的工具选择使用 sonarqube。
- 为方便使用, 使用 maven 的 sonar plugin 来进行代码分析而不使用 sonar-scaner 进行代码分析。

### 使用 maven 进行的 sonar plugin 进行代码分析

  - 方案:
    1. 修改 maven 的 setting.xml, 添加 sonar 支持:
      - 配置如下

```xml
<settings>
    <pluginGroups>
        <pluginGroup>org.sonarsource.scanner.maven</pluginGroup>
    </pluginGroups>
    <profiles>
        <profile>
            <id>sonar</id>
            <activation>
                <activeByDefault>true</activeByDefault>
            </activation>
            <properties>
                <!-- Optional URL to server. Default value is http://localhost:9000 -->
                <sonar.host.url>
                  http://sonar-server:sonar-port
                </sonar.host.url>
                <sonar.login>
                  xxxxxxxxxxxxxx<!-- token -->
                </sonar.login>
            </properties>
        </profile>
     </profiles>
</settings>
```
    - 执行指令: `mvn sonar:sonar`

    2. maven 构建时将参数传入到 sonar plugin 中。
      - 执行指令:

```bash
mvn sonar:sonar -Dsonar.host.url=http://sonar-server:sonar-port \
  -Dsonar.login=<token> \ # token 在配置页面获取, 通过 token 上传项目进行分析, 并关联用户。
  [opetions]
```

- 建议使用第二种, 能够灵活配置, 而不需要每次进行分析都去修改 maven 配置。
- 参数配置详情查看官方说明: https://docs.sonarqube.org/display/SONAR/Analysis+Parameters#AnalysisParameters-ProjectConfiguration.1
