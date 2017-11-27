# slf4j

SLF4J 是一个更加简洁的依赖，在运行时相对于 commons-logging 更加的有效因为它使用编译时绑定来代替运行时发现其他日志框架的集成。这也意味着, 你不得不更加明确你想在运行时发生什么, 并相应的声明它或者配置它。SLF4J 提供绑定很多的常见日志框架, 因此你可以选择一个你已经使用的, 并且绑定到配置和管理。

SLF4J 提供了绑定很多的常见日志框架，包括 JCL，它也做了反向工作:是其他日志框架和它自己之间的桥梁。因此在 Spring 中使用 SLF4J 时，你需要使用 SLF4J-JCL 桥接替换掉 commons-logging 的依赖。一旦你这么做了，Spring 调用日志就会调用 SLF4J API，因此如果在你的应用程序中的其他库使用这个API，那么你就需要有个地方配置和管理日志。

与 spring 绑定 `pom.xml` 内容:

```xml
<dependencies>
    <dependency>
        <groupId>org.springframework</groupId>
        <artifactId>spring-core</artifactId>
        <version>4.3.0.RELEASE</version>
        <exclusions>
            <exclusion>
                <groupId>commons-logging</groupId>
                <artifactId>commons-logging</artifactId>
            </exclusion>
        </exclusions>
    </dependency>
    <dependency>
        <groupId>org.slf4j</groupId>
        <artifactId>jcl-over-slf4j</artifactId>
        <version>1.5.8</version>
    </dependency>
    <dependency>
        <groupId>org.slf4j</groupId>
        <artifactId>slf4j-api</artifactId>
        <version>1.5.8</version>
    </dependency>
    <dependency>
        <groupId>org.slf4j</groupId>
        <artifactId>slf4j-log4j12</artifactId>
        <version>1.5.8</version>
    </dependency>
    <dependency>
        <groupId>log4j</groupId>
        <artifactId>log4j</artifactId>
        <version>1.2.14</version>
    </dependency>
</dependencies>
```
