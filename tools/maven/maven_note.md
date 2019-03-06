# maven使用笔记

## pom.xml配置

pom 代表项目对象模型。它是工作在 Maven 的基本单位。是一个 xml 文件。它始终保存在该项目基本目录中的 pom.xml 文件。

pom 包含的项目是使用 Maven 来构建的，以及它也包含各种配置信息。

POM 也包含了目标和插件。在执行任务或目标时，Maven 会使用当前目录中的pom。读取 pom，得到所需要的配置信息。pom 配置：

属性名称               | 含义
------------------ | -------------------------------------------------------------------------------------------------------------
modelVersion       | 指定了当前 Maven 模型的版本号，对于 Maven2 和 Maven3 来说，它只能是4.0.0。
groupId            | 这个应该是公司名或是组织名。一般来说groupId是由三个部分组成，每个部分之间以"."分隔，第一部分是项目用途，比如用于商业的就是"com"，用于非营利性组织的就是"org"；第二部分是公司名；第三部分是你的项目名。
artifactId         | Maven构建的项目名，比如项目中有子项目，就可以使用"项目名-子项目名"的命名方式。
version            | 版本号。
install            | mvn install 是将你打好的 jar 包安装到你的本地库中，一般没有设置过是在用户目录下的 .m2/ 下面
packaging          | 项目打包的类型，可以使 jar、war、rar、ear、pom，默认是jar。
build              | 表示与构建相关的配置
dependencies       | 为了一个项目可以 build 或运行所需要的依赖。
properties         | 定义一些配置属性。
repositories       | 在project内定义其他的连接的远程仓库。
pluginRepositories | TODO

### 基本属性

通过 groupId、artifactId、version 三个属性可以定位到一个 maven project 的基本坐标。

注：version 值为 x.x.x-SNAPSHOT 表示该项目还在开发中。

### dependencies

Maven 的一个重要作用就是统一管理jar包。在一个 project 中一代的其他 jar 包在 maven 中称为 dependency。配置如下：

```xml
<dependencies\>
  <dependency\>
    <groupId\>junit</groupId\>
    <artifactId\>junit-dep</artifactId\>
    <version\>4.12</version\>
    <scope>runtime</scope>
  </dependency\>
</dependencies\>
```

- scope 标签内容及意义
  1. compile: 默认就是 `compile`, 表示被依赖项目需要参与当前项目的编译, 当然后续的测试, 运行周期也参与其中, 是一个比较强的依赖。
  2. test: 表示依赖项目仅仅参与测试相关的工作, 包括测试代码的编译, 执行。比较典型的如junit。
  3. runtime: 表示被依赖项目无需参与项目的编译, 不过后期的测试和运行周期需要其参与。与 `compile` 相比, 跳过编译而已。 `oracle jdbc` 驱动架包就是一个很好的例子, 一般 `scope` 为`runntime`。
  4. provided: `provided` 意味着打包的时候可以不用包进去, 别的设施(Web Container, 运行容器)会提供。
  5. system: 从参与度来说, 也 `provided` 相同, 不过被依赖项不会从 maven 仓库抓, 而是从本地文件系统拿, 一定需要配合 `systemPath` 属性使用。

配置好 dependencies 后， Maven 会自动去远程仓库中下载jar包到本地仓库中。

### properties

properties是用来定义一些配置属性的，例如project.build.sourceEncoding（项目构建源码编码方式），可以设置为UTF-8，防止中文乱码，也可定义相关构建版本号，便于日后统一升级。

### build

build表示与构建相关的配置，比如 build 下有 finalName ，表示的就是最终构建之后的名称。

TODO

## 常用命令

command                | 作用
---------------------- | ---------------------------------------------------------------------
mvn clean              | 清理项目生产的临时文件,一般是模块下的 target 目录
mvn compile            | 编译源代码，一般编译模块下的 src/main/Java 目录
mvn package            | 项目打包工具,会在模块下的 target 目录生成jar或war等文件, 通过指定 `mainClass` 来指定可执行 jar 的主类.
mvn test               | 测试命令,或执行 src/test/java/ 下的测试用例.
mvn install            | 将打包的 jar/war 文件复制到你的本地仓库中,供其他模块使用
mvn deploy             | 将打包的文件发布到远程参考,提供其他人员进行下载依赖
mvn site               | 生成项目相关信息的网站
mvn eclipse:eclipse    | 将项目转化为Eclipse项目
mvn dependency:tree    | 打印出项目的整个依赖树
mvn archetype:generate | 创建Maven的普通java项目
mvn tomcat:run         | 在tomcat容器中运行web应用
mvn jetty:run          | 调用 Jetty 插件的 Run 目标在 Jetty Servlet 容器中启动 web 应用
mvn versions:set -DnewVersion=\<new_version> | 更新版本

**注意：运行 maven 命令的时候，首先需要定位到 maven 项目的目录，也就是项目的 pom.xml 文件所在的目录。否则，必以通过参数来指定项目的目录。**

## parameters

1. 跳过单元测试

```bash
mvn install -DskipTests
mvn install -Dmaven.test.skip=true
```

1. mvn install 后 mvn clean 结果

编译好的结果会在被存放到本地 maven 仓库中, 不会因为执行 `mvn clean` 而清除

1. deploy

2. dependency:tree

用于查看依赖关系

## 插件

1. surefire

  - 作用

```txt
Maven通过Maven Surefire Plugin插件执行单元测试
资料:http://www.cnblogs.com/pixy/p/4718176.html
```

2. tomcat Plugin

pom.xml 内容:

```xml
<plugin>
  <groupId>org.apache.tomcat.maven</groupId>
  <artifactId>tomcat7-maven-plugin</artifactId>
  <version>2.1</version>
  <configuration>
    <port>9090</port>
    <path>/mgr</path>
    <uriencoding>UTF-8</uriencoding>
    <finalname>mgr</finalname>
    <server>tomcat7</server>
  </configuration>
</plugin>
```

运行使用 `mvn tomcat:run`

3. mvn idea:\<options\>

```bash
mvn idea:clean # 清除 IEDA 信息
mvn idea:idea # 创建 IDEA 信息
```

4. exec-maven-plugin

```xml
<plugin>
  <artifactId>exec-maven-plugin</artifactId>
  <groupId>org.codehaus.mojo</groupId>
  <executions>

    <execution>
      <id>runfilter</id>
      <phase>package</phase>
      <goals>
        <goal>exec</goal>
      </goals>
      <configuration>
        <executable>${basedir}/deploy.sh</executable>
        <arguments>
          <argument>build</argument>
        </arguments>
      </configuration>
    </execution>
    <execution>
      <id>clean-dubbo-cache</id>
      <phase>clean</phase>
      <goals>
        <goal>exec</goal>
      </goals>
      <configuration>
        <executable>${basedir}/deploy.sh</executable>
        <arguments>
          <argument>clean</argument>
        </arguments>
      </configuration>
    </execution>
  </executions>
</plugin>
```

编译后执行 command, `arguments` 中为执行添加的参数。

5. maven-assembly-plugin

pom 文件内容是配置 build 成一个在 `manifest` 带 `mainClass` 的 jar:

```xml
<plugin>
  <artifactId>maven-assembly-plugin</artifactId>  
  <version>3.1.0</version>  
  <configuration>
    <descriptors>
      <descriptor>assembly.xml</descriptor>
    </descriptors>  
    <archive>
      <manifest>
        <mainClass>com.vidi.mrjob.GapDeduceRunner</mainClass>
      </manifest>
    </archive>
  </configuration>  
  <executions>
    <execution>
      <id>make-assembly</id>  
      <phase>package</phase>  
      <goals>
        <goal>single</goal>
      </goals>
    </execution>
  </executions>
</plugin>
```

同时需要一个 descriptor, 在上米娜的定义中是 `assembly.xml` 文件, 需要建立一个 `assembly.xml` 文件:

```xml
<assembly>
  <id>job</id>  
  <formats>
    <format>jar</format>
  </formats>  
  <includeBaseDirectory>false</includeBaseDirectory>  
  <dependencySets>
    <dependencySet>
      <unpack>false</unpack>  
      <scope>runtime</scope>  
      <outputDirectory>lib</outputDirectory>  
      <excludes>
        <exclude>${groupId}:${artifactId}</exclude>
      </excludes>
    </dependencySet>  
    <dependencySet>
      <unpack>true</unpack>
      <includes>
        <include>${groupId}:${artifactId}</include>
      </includes>
    </dependencySet>
  </dependencySets>
</assembly>
```

6. duplicate-finder-maven-plugin

用来检查依赖重复的插件。 基本配置如下:

```xml
<plugin>
  <!-- 检查依赖 duplicate -->
  <groupId>org.basepom.maven</groupId>
  <artifactId>duplicate-finder-maven-plugin</artifactId>
  <version>1.3.0</version>
  <executions>
    <execution>
      <id>default</id>
      <phase>verify</phase>
      <goals>
        <goal>check</goal>
      </goals>
    </execution>
  </executions>
  <configuration>
    <skip>false</skip>
    <quiet>false</quiet>
    <checkCompileClasspath>true</checkCompileClasspath>
    <checkRuntimeClasspath>true</checkRuntimeClasspath>
    <checkTestClasspath>true</checkTestClasspath>
    <failBuildInCaseOfDifferentContentConflict>false</failBuildInCaseOfDifferentContentConflict>
    <failBuildInCaseOfEqualContentConflict>false</failBuildInCaseOfEqualContentConflict>
    <failBuildInCaseOfConflict>false</failBuildInCaseOfConflict>
    <printEqualFiles>false</printEqualFiles>
    <preferLocal>true</preferLocal>
    <includeBootClasspath>false</includeBootClasspath>
    <bootClasspathProperty>sun.boot.class.path</bootClasspathProperty>

    <includePomProjects>false</includePomProjects>
  </configuration>
</plugin>
```

```bash
# 执行检查重复引入插件的指令
mvn duplicate-finder:check
```

详情参考:

- [工具Github地址](https://github.com/basepom/duplicate-finder-maven-plugin)
- [Wiki](https://github.com/basepom/duplicate-finder-maven-plugin/wiki)

7. scope 标签

```xml
<dependency>
  <groupId>xxx</groupId>
  <artifactid>xxx</artifactId>
  <version>xxx</version>
  <scope>compile</scope>
</dependency>
```

说明:
> 默认值: compile: 用于所有阶段, 会随着项目一起发布。
> provided: 类似compile，期望JDK, 容器或使用者会提供这个依赖。 如 Servlet等。
> runtime: 只在运行时使用, 如 JDBC。
> test: 只在测试时使用, 用于编译和运行测试代码。不会随项目发布。
> system: 类似provided, 需要显式提供包含依赖的jar, Maven不会在Repository中查找它。

8. 通过占位符传入参数

```xml
<project>
  <my.param>${param}</my.param>
</project>
```

```bash
mvn clean package -Dmy.param=<my_param>
```