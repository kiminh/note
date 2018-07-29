# Spring 框架参考文档

原文地址: [Spring Framework Reference Documentation](https://docs.spring.io/spring/docs/4.3.19.BUILD-SNAPSHOT/spring-framework-reference/htmlsingle/)

- 目录

- [Spring 框架参考文档](#spring-%E6%A1%86%E6%9E%B6%E5%8F%82%E8%80%83%E6%96%87%E6%A1%A3)
    - [I. Spring 框架概览](#i-spring-%E6%A1%86%E6%9E%B6%E6%A6%82%E8%A7%88)
        - [1.开始](#%081%E5%BC%80%E5%A7%8B)
        - [2. Spring 框架介绍](#2-spring-%E6%A1%86%E6%9E%B6%E4%BB%8B%E7%BB%8D)
            - [2.1 DI 和 IoC](#21-di-%E5%92%8C-ioc)
            - [2.2 模块](#22-%E6%A8%A1%E5%9D%97)
                - [2.2.1 概述](#221-%E6%A6%82%E8%BF%B0)
                - [2.2.2 核心容器](#222-%E6%A0%B8%E5%BF%83%E5%AE%B9%E5%99%A8)
                - [2.2.3 AOP and Instrumentation](#223-aop-and-instrumentation)
                - [2.2.4 Messaging](#224-messaging)
                - [2.2.5 数据访问/集成](#225-%E6%95%B0%E6%8D%AE%E8%AE%BF%E9%97%AE%E9%9B%86%E6%88%90)
                - [2.2.6 Web](#226-web)
                - [2.2.7 Test](#227-test)
            - [2.3 使用场景](#23-%E4%BD%BF%E7%94%A8%E5%9C%BA%E6%99%AF)
                - [2.3.1 依赖关系管理 和 命名约定](#231-%E4%BE%9D%E8%B5%96%E5%85%B3%E7%B3%BB%E7%AE%A1%E7%90%86-%E5%92%8C-%E5%91%BD%E5%90%8D%E7%BA%A6%E5%AE%9A)
                - [2.3.2 日志](#232-%E6%97%A5%E5%BF%97)
    - [II. What's New in Spring Framework 4.x](#ii-whats-new-in-spring-framework-4x)
    - [III. 核心技术](#iii-%08%E6%A0%B8%E5%BF%83%E6%8A%80%E6%9C%AF)
        - [7. IoC 容器](#7-ioc-%E5%AE%B9%E5%99%A8)
            - [7.1 Spring IoC 容器和 Bean 的介绍](#71-spring-ioc-%E5%AE%B9%E5%99%A8%E5%92%8C-bean-%E7%9A%84%E4%BB%8B%E7%BB%8D)
            - [7.2 容器概述](#72-%E5%AE%B9%E5%99%A8%E6%A6%82%E8%BF%B0)
                - [7.2.1 配置元数据](#721-%E9%85%8D%E7%BD%AE%E5%85%83%E6%95%B0%E6%8D%AE)
                - [7.2.2 实例化容器](#722-%E5%AE%9E%E4%BE%8B%E5%8C%96%E5%AE%B9%E5%99%A8)
                - [7.2.3](#723)
            - [7.3 Bean 概述](#73-bean-%E6%A6%82%E8%BF%B0)
                - [7.3.1 Bean 命名](#731-bean-%E5%91%BD%E5%90%8D)
                - [7.3.2 初始化 Bean](#732-%E5%88%9D%E5%A7%8B%E5%8C%96-bean)
            - [7.4 Dependencies](#74-dependencies)
                - [7.4.1 依赖注入](#741-%E4%BE%9D%E8%B5%96%E6%B3%A8%E5%85%A5)
                - [7.4.2 依赖关系和配置细节](#742-%E4%BE%9D%E8%B5%96%E5%85%B3%E7%B3%BB%E5%92%8C%E9%85%8D%E7%BD%AE%E7%BB%86%E8%8A%82)
                - [7.4.4 Lazy-initialization Bean](#744-lazy-initialization-bean)
            - [7.5 Bean 范围](#75-bean-%E8%8C%83%E5%9B%B4)
            - [7.7 Bean 定义继承](#77-bean-%E5%AE%9A%E4%B9%89%E7%BB%A7%E6%89%BF)
            - [7.9 基于 Java 注解的容器配置](#79-%E5%9F%BA%E4%BA%8E-java-%E6%B3%A8%E8%A7%A3%E7%9A%84%E5%AE%B9%E5%99%A8%E9%85%8D%E7%BD%AE)
            - [7.12 基于 Java 代码的容器配置](#712-%E5%9F%BA%E4%BA%8E-java-%E4%BB%A3%E7%A0%81%E7%9A%84%E5%AE%B9%E5%99%A8%E9%85%8D%E7%BD%AE)
                - [7.12.3 使用 @Bean 注解](#7123-%E4%BD%BF%E7%94%A8-bean-%E6%B3%A8%E8%A7%A3)
        - [8. Resources](#8-resources)
        - [9. Validation, Data Binding, and Type Conversion](#9-validation-data-binding-and-type-conversion)
        - [10. Spring Expression Language](#10-spring-expression-language)
        - [11. Aspect Oriented Programming with Spring](#11-aspect-oriented-programming-with-spring)
            - [11.8 通过 AspectJ 和 Spring 来依赖注入 domain 对象](#118-%E9%80%9A%E8%BF%87-aspectj-%E5%92%8C-spring-%E6%9D%A5%E4%BE%9D%E8%B5%96%E6%B3%A8%E5%85%A5-domain-%E5%AF%B9%E8%B1%A1)
        - [12. Spring AOP APIs](#12-spring-aop-apis)
            - [12.5 Using the ProxyFactoryBean to create AOP proxies](#125-using-the-proxyfactorybean-to-create-aop-proxies)
                - [12.5.2 AOP 拦截器](#1252-aop-%E6%8B%A6%E6%88%AA%E5%99%A8)
    - [IV. Test](#iv-test)
        - [13. Introduction to Spring Testing](#13-introduction-to-spring-testing)
        - [14. Unit Test](#14-unit-test)
        - [15. Integration Test](#15-integration-test)
        - [16. Further Resources](#16-further-resources)
    - [V. Data Access](#v-data-access)
        - [17. Transaction Manager](#17-transaction-manager)
        - [18. DAO support](#18-dao-support)
        - [19. Data access with JDBC](#19-data-access-with-jdbc)
        - [20. Object Relational Mapping(ORM) Data Access](#20-object-relational-mappingorm-data-access)
            - [20.1. Introduction to ORM with Spring](#201-introduction-to-orm-with-spring)
            - [20.2. General ORM integration consideration](#202-general-orm-integration-consideration)
            - [20.3. Hibenrate](#203-hibenrate)
            - [20.4. JDO](#204-jdo)
            - [20.5. JPA](#205-jpa)
        - [21 Marshalling XML using O/X Mappers](#21-marshalling-xml-using-ox-mappers)
    - [VI. The Web](#vi-the-web)
        - [22. Web MVC framework](#22-web-mvc-framework)
        - [23. View Technologies](#23-view-technologies)
        - [24. Integrating with other web frameworks](#24-integrating-with-other-web-frameworks)
        - [25. Portlet MVC Framework](#25-portlet-mvc-framework)
        - [26. WebSocket Support](#26-websocket-support)
        - [27. CORS Support](#27-cors-support)
    - [VII. Integration](#vii-integration)
        - [28. Remoting and web service using Spring](#28-remoting-and-web-service-using-spring)
        - [29. Enterprise JavaBeans(EJB) integration](#29-enterprise-javabeansejb-integration)
        - [30. JMS](#30-jms)
        - [31. JMX](#31-jmx)
        - [32. JCA CCI](#32-jca-cci)
        - [33. Email](#33-email)
        - [34. Task Execution and Schedule](#34-task-execution-and-schedule)
        - [35. Dynamic language support](#35-dynamic-language-support)
        - [36. Cache Abstraction](#36-cache-abstraction)
    - [VIII. Appendices](#viii-appendices)
        - [37. Migrating to Spring Framework 4.x](#37-migrating-to-spring-framework-4x)
        - [38. Spring Annotation Programming Model](#38-spring-annotation-programming-model)
        - [39. Classic Spring usage](#39-classic-spring-usage)
        - [40. Classic Spring AOP usage](#40-classic-spring-aop-usage)
        - [41. XML Schema-based configuration](#41-xml-schema-based-configuration)
        - [42. Extensible XML authoring](#42-extensible-xml-authoring)
        - [43. Spring JSP Tag Library](#43-spring-jsp-tag-library)
        - [44. Spring-form JSP Tag Library](#44-spring-form-jsp-tag-library)

## [I. Spring 框架概览](https://docs.spring.io/spring/docs/4.3.19.BUILD-SNAPSHOT/spring-framework-reference/htmlsingle/#overview-getting-started-with-spring)

Spring 框架是一个轻量级的解决方案, 并可能为企业级应用提供一个一站式的服务。 尽管 Spring 是一由多部件组成的, 但是 Spring 允许只引入所需要的部分而不是必须吧其他的也一并引入。 你可以在任何 Web 框架下使用 IoC 容器, 也可以只使用 Hibernate 集成代码活 JDBC 抽象层。 Spring 支持声明式事务管理, 通过 RMI 或 Web 服务进行远程访问, 以及用于持久化保存数据的各种选项。 ~~它提供了全功能的 MVC 框架, 支持将 AOP 显示地集成软件中。??? 有点问题~~

Spring 被设计成 `non-instrusive`, 即逻辑代码一般没有依赖框架本身。 在集成这一层(如s护具访问层), 将存在一些对数据访问技术和 Spring 库的依赖, 但很容易将这些依赖与其他代码库隔离开。

This document is a reference guide to Spring Framework features. If you have any requests, comments, or questions on this document, please post them on the user mailing list. Questions on the Framework itself should be asked on StackOverflow (see https://spring.io/questions).

### 1.开始

参考指南提供关于 Spring 框架的具体细节, 包括 Spring 框架的所有特性和相关概念。

### 2. Spring 框架介绍  

Spring 框架是为 Java 应用程序提供全面的基础框架支持的一个 Java 平台, Spring 处理基础架构, 以便专注于应用程序的开发。

Spring 能够使得应用开发基于 POJOs(plain old Java objects), 并以非入侵方式将企业服务应用于 POJO。 此功能适用于 Java SE 编程模型以及完整和部分 Java EE。

Example: 
> - 使 Java 方法在数据库事务中执行, 而不必处理事物API。
> - 使本地方法称为 HTTP 端点, 无需去处理 Servlet API。
> - 使本地方法成为消息处理程序, 而无需处理 JMS API。
> - 使本地方法成为管理操作, 而无需处理JMX API。

#### 2.1 DI 和 IoC

Java应用程序 (从受限制的嵌入式应用程序到n层服务器端企业应用程序的宽松术语) 通常由协作形成应用程序的对象组成。 因此, 应用程序中的对象彼此依赖。 尽管Java平台提供了丰富的应用程序开发功能, 但它缺乏将基本构建块组织成一个连贯整体的方法, 并将该任务留给架构师和开发人员。 

~~尽管能够通过像工厂、抽象工厂、建造者、装饰者、服务定位器等设计模式来组成应用程序的各种累和对象实例, 但这些模式被描述为模式的作用、应用位置、解决的问题等只是给出名称的最佳实践。不通`Although the Java platform provides a wealth of application development functionality, it lacks the means to organize the basic building blocks into a coherent whole, leaving that task to architects and developers. Although you can use design patterns such as Factory, Abstract Factory, Builder, Decorator, and Service Locator to compose the various classes and object instances that make up an application, these patterns are simply that: best practices given a name, with a description of what the pattern does, where to apply it, the problems it addresses, and so forth. Patterns are formalized best practices that you must implement yourself in your application.`~~

Spring 框架控制反转(IoC)组件通过提供一种 将不同组件 组合成 一个可以使用的 完全工作的 应用程序的 形式化方法来解决这一问题。 Spring 框架将形式化的设计模式编码为可以集成到应用程序中的 first-class 对象。 许多组织和机构以这种方式使用Spring 框架来设计健壮且可维护的应用程序。

> Background  
> "The question is, what aspect of control are [they] inverting?" Martin Fowler posed this question about Inversion of Control (IoC) on his site in 2004. Fowler suggested renaming the principle to make it more self-explanatory and came up with Dependency Injection.

#### 2.2 模块

##### 2.2.1 概述

![Overview of the Spring Framework](imgs/Overview_of_the_Spring_Framework.png)

##### 2.2.2 核心容器

核心容器包含了 `spring-core`, `spring-beans`, `spring-context`, `spring-context-support` 和 `spring-expression(spring Expression Language)` 模块。

`spring-core` 和 `spring-beans` 模块提供了 Spring 框架的最基本的部分, 包含了 IoC 和 DI 的特性。 `BeanFactory` 是工厂模式的复杂实现, 它消除了对程序化单例的需求, 并允许从实际程序逻辑中分离依赖项的配置和规范。

Context(`spring-context`) 模块建立在 `spring-core` 和 `spring-beans` 模块提供的坚实基础之上, 这是一种类似于 `JNDI` 注册表的框架样式方式来访问对象的方法。Context 模块继承了 Bean 模块中的特性, 并且增加了对国际化的支持(如: resource bundles), 事件传播, 资源加载, 显式创建上下文(如: Servlet 容器)。 Context 模块还提供诶类似于 Java EE 的特性(如: EJB, JMX, 基本的远程处理)。 `ApplicationContext` 接口是 Context 模块的焦点, `spring-context-support` 提供集成三方依赖(如: 缓存[`EhCache`, `Guava`, `JCache`], 邮件(`JavaMail`), 任务调度[`CommonJ`, `Quartz`], 模版引擎[`FreeMarker`, `JasperReports`, `Velocity`])到 Spring Application的支持。

`spring-expression` 模块提供了一个强有力的在运行时查询和操作对象的 `Expression Language(表达语言)`。 它是 `JSP 2.1` 规范中指定的统一表达式语言(统一EL)的扩展。 EL 提供配置 `setter` 和 `getter` 的值, 分配 property, 方法调用, 访问数组、集合、索引器中的内容, 逻辑和算数运算符, 命名变量以及从 Spring 的 IoC 容器中按名称检索对象。 EL 还支持表投影和选择以及常用列表聚合。

##### 2.2.3 AOP and Instrumentation

`spring-aop` 模块提供了一个符合 AOP Alliance 标准的允许自定义的面相切面的编程实现, 如: 方法拦截器和切点干净的解耦实现应当分离的功能代码。 使用源级元数据功能, 还能够以类似 `.NET` 属性的方式将行为信息和冰岛代码中。

TODO
**`spring-aspects` 模块提供与 `AspectJ` 集成的支持。**

**`spring-instrument` 模块提供了在某些应用程序服务中使用类检测和类加载的支持的实现。**

##### 2.2.4 Messaging

Spring Framework 4 包含 `spring-messaging` 模块, 该模块是对集成 `Message`, `MessageChannel`, `MessageHandler` 的关键抽象, 可作为基于消息传递的应用程序的基础。 该模块还提供了一系列将消息映射到方法的注解(类似于 Spring MVC 注解的编程模型)。

##### 2.2.5 数据访问/集成

数据访问/集成层由 `JDBC`, `ORM`, `OXM(对象 XML 映射解析)`, `JMS(Java Message Service)`, `Transaction(事物)` 模块组成。

`spring-jdbc` 模块提 JDBC 抽象层, 无需进行繁琐的 JDBC 编码和解析数据库供应商特定的错误代码。

`spring-tx` 模块支持对实现特殊接口和所有 POJO 的类进行编程和声明事务管理。

`spring-orm` 模块为流行的 ORM API 提供集成层, 包括JPA, JDO 和 Hibernate。 通过 `spring-orm` 模块, 所有的 ORM 框架与 Spring 提供的其他功能可以结合使用(~~类似简单声明事务管理的功能???~~)。

`spring-oxm` 模块提供一个抽象层, 支持 OXM(对象/XML映射) 实现, 如 `JAXB`, `Castor`, `XMLBeans`, `JiBX` 和 `XStream`。

`spring-jms` 模块包含用于生产和消费消息的功能。 到 Spring Framework 4.1 开始提供 `spring-messaging` 模块的集成。

##### 2.2.6 Web

Web 层由 `spring-web`, `spring-webmvc`, `spring-websocket` 和 `spring-webmvc-protlet` 模块组成。

`spring-web` 模块提供了基本的面向 Web 的集成功能, 如: multipart(多部分?) 文件上传功能, 用 `Servlet` 的 `listener` 和面向 Web 的 application context(应用程序上下文) 来初始化 IoC 容器。 同时包含了一个 HTTP 客户端和 Spring 远程支持的 Web 相关部分。

`spring-webmvc` 模块(也被称为 `Web-Servlet 模块) 包含 Spring 的 MVC 和 REST Web 服务的实现。 Spring 的 MVC 框架提供了域模型代码和 Web 表单之间的清晰分离, 并于 Spring 框架的其他功能集成在一起。

`spring-webmvc-portlet` 模块(也被称为 `Web-Poerlet` 模块) 提供了在 Portlet 环境中使用 MVC 的具体实现, 包含 基于 Servlet 的 `spring-webmvc` 的基本功能。

##### 2.2.7 Test

`spring-test` 模块通过 `JUnit` 和 `TestNG`提供了 Spring 的组件的单元测试和集成测试。 该模块提供了一致的 `Spring ApplicationContext` 的加载, 并缓存这些 context。 同时, 它还提供 mock 对象用来隔离测试。

#### 2.3 使用场景

前面描述的构建块使 Spring 成为许多场景中的合理选择, 从在资源受限设备上运行的嵌入式应用程序到使用 Spring 的事务管理功能和 Web 框架集成的成熟企业应用程序。

- 典型的完整 Spring web 应用

![Typical\ full-fledged\ Spring\ web\ application
](imgs/typical_full-fledged_spring_web_application.png)

- Spring 中间层使用第三方的 Web 框架

![Spring middle-tier using a third-party web framework](imgs/spring_middle-tier_using_a_third_party_web_framework.png)

- 远程处理场景

![Remoting usage scenario](imgs/remoting_usage_scenario.png)

- EJB - 包装现有的 POJO

![EJBs - Wrapping existing POJOs](imgs/ejb_wrapping_existing_pojos.png)

##### 2.3.1 依赖关系管理 和 命名约定

依赖关系管理和依赖注入不同。 要将 Spring 的这些优秀功能集成到应用中(如: 依赖注入), 需要组装所需的所有库, 并在编译时和运行时将它们放到 `classpath` 下。 这些依赖不是被注入的虚拟的组件, 而是真实存在的系统文件。 依赖关系管理涉及定位、存储这些资源, 并将它们加载到 `classpath` 中。

- Spring Framework Articats

| GroupId             | ArtifactId               | 描述                                                                              |
| ------------------- | ------------------------ | --------------------------------------------------------------------------------- |
| org.springframework | spring-aop               | 基于代理的 AOP 支持                                                               |
| org.springframework | spring-aspects           | 基于 aspects 的 AspectJ                                                           |
| org.springframework | spring-beans             | 包括 `groovy` 的 `bean` 支持                                                     |
| org.springframework | spring-context           | 应用的运行时 context(上下文), 包括远程调用和任务调度的抽象                        |
| org.springframework | spring-context-support   | 支持类, 包括常见的三方库集成到 Spring 程序的上下文中                              |
| org.springframework | spring-core              | 核心工具, 被其他的 Spring 模块使用                                                |
| org.springframework | spring-expression        | Spring EL 表达式                                                                  |
| org.springframework | spring-instrument        | 用于 JVM 引导的检测代理程序                                                       |
| org.springframework | spring-instrument-tomcat | 用于 JVM 引导的检测 Tomcat 的代理程序                                             |
| org.springframework | spring-jdbc              | JDBC 支持包, 包括数据源配置和对数据库访问数据的支持                               |
| org.springframework | spring-jms               | JMS 支持包, 包括用于 发送/接收 JSM 消息的辅助类                                  |
| org.springframework | spring-messaging         | ~~支持通过消息传递结构和协议(Support for messaging architectures and protocols)~~ |
| org.springframework | spring-orm               | orm, 包括 JPA, Hibernate 的支持                                                   |
| org.springframework | spring-oxm               | 对象/XML的映射                                                                    |
| org.springframework | spring-test              | 提供单元测试和集成测试                                                            |
| org.springframework | spring-tx                | 提供事物管理, 包括 DAO 支持 和 JCA 集成                                           |
| org.springframework | spring-web               | 基础 Web 支持, 包括 Web 客户端和基于 Web 的远程处理                               |
| org.springframework | spring-webmvc            | 用于 Servlet stack 的 基于 HTTP 的 MVC 和 REST ~~端点(endpoints??)~~              |
| org.springframework | spring-webmvc-portlet    | 在 `Portlet` 环境中使用的 MVC 实现                                                |
| org.springframework | spring-websocket         | WebSocket 和 SockJS 的基础框架, 包括 STOMP 消息传递的支持                         |

##### 2.3.2 日志
TODO

## II. What's New in Spring Framework 4.x
TODO
1. 4.0 新特性
2. 4.1 新特性
3. 4.2 新特性
4. 4.3 新特性

## III. 核心技术

该部分涵盖了 Spring 框架的绝对不可或缺的技术。

其中最重要的是 `Inversion of Control(IoC)` 容器, 

### 7. IoC 容器

#### 7.1  Spring IoC 容器和 Bean 的介绍

该部分包含了 Spring 框架实现 IoC 的原则。 IoC 也称为依赖注入(Dependency Inject/DI)。 ~~这是一个过程，通过这个过程，对象定义它们的依赖关系，即它们使用的其他对象，只能通过构造函数参数，工厂方法的参数，或者在构造或从工厂方法返回后在对象实例上设置的属性。~~ 容器在创建 `bean` 的时候注入这些依赖。 这个过程基本是相反的, 因此名称 `Inversion of Control(IoC)`, bean 本身通过使用类的直接构造或诸如 `Service Locator` 模式之类的机制来控制其依赖关系的实例化或位置。

`org.springframework.beans` 和 `org.springframework.context` 是 Spring 框架的 IoC 容器的基础。 `BeanFactory` 接口提供能够管理任何类型对象的高级配置机制。 `ApplicationContext` 是 `BeanFactory` 的一个子接口, 它使得集成 Spring AOP、 国际化、 ~~事件发布(event publication)~~ 和 如同 `WebApplicationContext` 的应用于一个 Web 应用的特定层的上下文 的功能更容哟。

总而言之, `BeanFactory` 提供了配置框架和基本的功能, `ApplicationContext` 添加了更多的企业专有的特性。 `ApplicationContext` 是 `BeanFactory` 的更完善的一个~~超集(superset?)~~。   refer to Section 7.16, “The BeanFactory”.

在 Spring 框架中, 构成应用程序主干并由 Spring IoC 容器管理的对象成为 bean。 Bean 是由 Spring IoC 容器 实例化, 组装 和 管理的对象。 另外, Bean 只是应用程序中的众多对象之一。 Bean 及其之间的依赖关系反映在容器使用的配置元数据中。

#### 7.2 容器概述

`org.springframework.context.ApplicationContext` 接口表示 Spring IoC 容器, 负责实例化、配置和组装上述 Bean。 容器通过配置元数据获取有关要实例化、配置和组装的对象的指令。 配置元数据通过 `XML`, 注解, 或者 Java 代码表示, 它允许通过表达式来组成应用以及这些对象之间的丰富的相互依赖。

接口 `ApplicationContext` 的几个实现是与 Spring 一起提供的。在独立的应用中, 创建 `ClassPathXmlApplicationContext` 或者 `FileSystemXmlApplicationContext` 是很常见的。 虽然 XML 是定义 Bean 的传统方式, 但仍可以通过提供少量 XML 配置来声明性的支持其他元数据方式。 从而使容器使用注解或者代码的方式。

在很多应用场景下, 实例化 Spring IoC 容器的一个或多个实例不需要显示的代码。 如: 在一个 Web 应用的场景, `web.xml` 文件中的简单几行描述即可。

- Spring IoC 容器

![The Spring IoC container](imgs/spring_ioc_container.png)

##### 7.2.1 配置元数据

Spring IoC 容器从配置元数据中消费, 该配置元数据, 该配置元数据表示如何告诉 Spring 容器在应用程序中实例化, 配置 和组装对象。

配置元数据的传统配置方法是以 XML 的方式提供, 本章大部分内容用于传达 Spring IoC 容器的关键概念和功能。 

> 基于 XML 的元数据不是唯一的配置元数据的形式, Spring IoC 容器本身与实际编写的元数据格式完全分离。 [基于 Java 代码的配置](#712-%E5%9F%BA%E4%BA%8E-java-%E4%BB%A3%E7%A0%81%E7%9A%84%E5%AE%B9%E5%99%A8%E9%85%8D%E7%BD%AE)

有关在 Spring 容器中使用其他形式的元数据的信息, 请参阅: 

- [基于 Java 注解配置](#79-%E5%9F%BA%E4%BA%8E-java-%E6%B3%A8%E8%A7%A3%E7%9A%84%E5%AE%B9%E5%99%A8%E9%85%8D%E7%BD%AE): Spring 2.5 引入了对基于注解的配置元数据
- [基于 Java 代码配置](#712-%E5%9F%BA%E4%BA%8E-java-%E4%BB%A3%E7%A0%81%E7%9A%84%E5%AE%B9%E5%99%A8%E9%85%8D%E7%BD%AE): 从 Spring 3.0 开始, Spring JavaConfig 项目提供的许多功能成为 Spring 框架的一部分。 因此能够使用 Java 在应用程序类外部定义 Bean 而非 XML。 通过 `@Configuration`, `Bean`, `@Import` 和 `@DependsOn` 注解来使用这些新功能。

Spring 配置由至少一个(通常不止一个) bean 定义组成, 基于 XML 的配置元数据通过 `<bean/>` 元素展现, `<bean/>` 元素在 `<beans/>` 元素里。 Java 配置通常在 `@Configuration` 注解的类中使用 `@Bean` 注解。

这些 bean 定义对应于构成应用程序的实际对象。 通常, 定义服务层对象(如: 数据访问层/DAO, ~~presentation objects such as Struts Action instances~~, 基础设施对象如 Hibernate 的 `SessionFactories`, JMS `Queue`, 等)。 通常, 不会在容器中配置细粒度 `domain` 对象, 因为 DAO 和业务逻辑通常负责创建和加载对象。 然而, **也可以使用 `Spring` 与 `AspectJ` 集成来配置在 IoC 容器的控制之外创建的对象。** 请参阅: [通过 AspectJ 和 Spring 来依赖注入 domain 对象](#118-%E9%80%9A%E8%BF%87-aspectj-%E5%92%8C-spring-%E6%9D%A5%E4%BE%9D%E8%B5%96%E6%B3%A8%E5%85%A5-domain-%E5%AF%B9%E8%B1%A1).

- 基于 XML 配置 Spring 元数据的基本结构

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
    xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
    xsi:schemaLocation="http://www.springframework.org/schema/beans
        http://www.springframework.org/schema/beans/spring-beans.xsd">

    <bean id="..." class="...">
        <!-- collaborators and configuration for this bean go here -->
    </bean>

    <bean id="..." class="...">
        <!-- collaborators and configuration for this bean go here -->
    </bean>

    <!-- more bean definitions go here -->

</beans>
```

具体配置参见: [7.4 Dependencies](#74-dependencies)

##### 7.2.2 实例化容器

实例化 Spring IoC 容器很简单, 提供给 ApplicationContext 构造函数的位置路径实际上是资源字符串, 允许容器从各种外部资源 (如本地文件系统, Java CLASSPATH等) 加载配置元数据。

```java
ApplicationContext context = new ClassPathXmlApplicationContext("services.xml", "daos.xml");
```

> 在学习过关于 Spring IoC 容器后, 也许对 Spring 的 `Resource` 的抽象会知道更多。 详细描述见 [Resource](#8-resources) 。 ~~as described in Chapter 8, Resources, which provides a convenient mechanism for reading an InputStream from locations defined in a URI syntax. In particular, Resource paths are used to construct applications contexts as described in Section 8.7, “Application contexts and Resource paths”.~~

- Service 层的配置: 

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
    xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
    xsi:schemaLocation="http://www.springframework.org/schema/beans
        http://www.springframework.org/schema/beans/spring-beans.xsd">

    <!-- services -->

    <bean id="petStore" class="org.springframework.samples.jpetstore.services.PetStoreServiceImpl">
        <property name="accountDao" ref="accountDao"/>
        <property name="itemDao" ref="itemDao"/>
        <!-- additional collaborators and configuration for this bean go here -->
    </bean>

    <!-- more bean definitions for services go here -->

</beans>
```

- dao 层的配置:

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
    xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
    xsi:schemaLocation="http://www.springframework.org/schema/beans
        http://www.springframework.org/schema/beans/spring-beans.xsd">

    <bean id="accountDao"
        class="org.springframework.samples.jpetstore.dao.jpa.JpaAccountDao">
        <!-- additional collaborators and configuration for this bean go here -->
    </bean>

    <bean id="itemDao" class="org.springframework.samples.jpetstore.dao.jpa.JpaItemDao">
        <!-- additional collaborators and configuration for this bean go here -->
    </bean>

    <!-- more bean definitions for data access objects go here -->

</beans>
```

上述例子中, Service 层由 `PetStoreServiceImpl` 对象 和两个数据访问对象 `JpaAccountDao`, `JpaItemDao`(基于 JPA 的 ORM 标准) 组成, `property name` 元素代表 JavaBean 的名称配置, `ref` bean 定义的另一个名称。 `id` 和 `ref` 的联系表示这些类之间的相互依赖关系。具体查看[Dependencies](#74-dependencies)

- 基于 XML 的配置元数据组成

```xml
<beans>
    <import resource="services.xml"/>
    <import resource="resources/messageSource.xml"/>
    <import resource="/resources/themeSource.xml"/>

    <bean id="bean1" class="..."/>
    <bean id="bean2" class="..."/>
</beans>
```

上述例子中, 这个 bean 定义去加载另外三个文件: `service.xml`, `messageSource.xml` & `themeSource.xml`。 所有位置路径与执行导入的定义文件相关, 因此 `service.xml` 必须和 `classpath` 在同一个路径下, `messageSource.xml` & `themeSource.xml` 必须在 `resources`。 

TODO

##### 7.2.3

`ApplicationContext` 是高级工厂的接口, 能够维护不同 bean 的依赖想的注册表。 通过 `T getBean(String name, Class<T> requiredType)` 方法能够取回相应 bean 的实例。

- `ApplicationContext` 使得能够读取 bean 的定义和访问, 如下:

```java
// create and configure beans
ApplicationContext context = new ClassPathXmlApplicationContext("services.xml", "daos.xml");

// retrieve configured instance
PetStoreService service = context.getBean("petStore", PetStoreService.class);

// use configured instance
List<String> userList = service.getUsernameList();
```

- 通过 Groovy 配置, bootstrap 看起来很小: 

```groovy
ApplicationContext context = new GenericGoovyApplicationContext("services.groovy", "daos.griivy");
```

- TODO

#### 7.3 Bean 概述

IoC 容器能够管理 Bean。 Bean 是通过元数据配置来创建的, 比如: 在 XML 中定义的 `<bean/>`。

在容器内, 这些 Bean 定义表示为 `BeanDefinition` 对象, 其中包含一下元数据: 

- 包限定的类名: 通常是定义的 Bean 的时机实现类。
- Bean 行为配置元素: 说明 bean 在容器中的行为方式(范围、生命周期、回掉等)。
- 被 Bean 的操作所需的 Bean, 这些引用被称为 *collaborators* 或者 *dependencies*。
- 要在新创建的对象中设置的其他配置项, 如: 一些在一个 Bean 中用到的连接, 它们管理着一个 *pool* 或限制大小的 *pool*。

- 定义 Bean

| 属性                            | 解释                                                   |
| ------------------------------- | ------------------------------------------------------ |
| class                           | [初始化 Bean](#732-%E5%88%9D%E5%A7%8B%E5%8C%96-bean)   |
| name                            | [Bean 命名](#731-bean-%E5%91%BD%E5%90%8D)              |
| scope                           | [Bean 范围]()                                          |
| constructor arguments(构造参数) | [依赖注入](#741-%E4%BE%9D%E8%B5%96%E6%B3%A8%E5%85%A5)  |
| 属性(对象的属性)                | [依赖注入](#741-%E4%BE%9D%E8%B5%96%E6%B3%A8%E5%85%A5) |
| autoeiring modo                 | []()                                                   |
| lazy-initialization mode        | []()                                                   |
| initialization method           | []()                                                   |
| destruction method              | []()                                                   |

除了包含如何创建特定 Bean 的信息的 Bean 定义之外, `ApplicationContext` 实现还允许用户注册在容器外部创建现有的对象。 这是通过 `getBeanFactory()` 方法访问 `ApplicationContext` 的 `BeanFactory` 来完成的, 该方法返回 `BeanFactory` 的实现 `DefaultListableFeanFactory`, `DefaultListableBeanFactory` 通过 `registerSingleton(...)` 方法和 `registerBeanDefinition(...)` 来支持注册 Bean。 然而, 典型的应用程序仅适用于元数据 Bean 的定义来定义的 Bean。

>  为了使容器在自动装配和其他内省步骤中能够正确推理 Bean, Bean 的元数据和手动供给的单例实例需要尽可能早的被注册。 虽然在某种程度上支持覆盖现有元数据和现有单例实例, 但是在运行时注册新 Bean(与对工厂的实时访问同时) 未获得官方支持, 并可能导致 Bean 容器中的并发访问异常 和/或 不一致状态(可能同时发生)。

##### 7.3.1 Bean 命名

所有 Bean 都有标志符, 这些标识符在托管 Bean 的容器内必须是唯一的。 Bean 通常只有一个标识符, 如果需要多个标识符, 则额外的标识符可以被视为别名。

在基于 XML 的配置元数据, 通过 `id` 或 `name` 属性来  ~~特别说明(定义?)~~  标识符。 `id` 只允许定义一个标识符。 通常, 这些名称是字母数字, 但也可能包含特殊字符。 如果想向 Bean 中引入其他别名, 可以通过用 `,(逗号)`. `;(分号)`. ` (空格)` 分隔的 `name` 属性来定义。

TODO

- 定义 Bean 的化名

在 Bean 定义本身中, 可以为 Bean 提供多个名称, 方法是使用 `id` 属性指定的最多一个名称和 `name` 属性中的任意数量的其他名称。 这些名称可以是同一个 Bean 的等效别名, 并且在某些情况下很有用, 例如允许应用程序中的每个组件通过使用特定于该组件本身的 Bean 名称来引用公共依赖项。

但是, 指定实际定义bean的所有别名并不总是足够的。 有时需要为其他地方定义的 Bean 引入别名。 在大型系统中通常就是这种情况, 其中配置在每个子系统之间分配, 每个子系统具有其自己的一组对象定义。 在基于XML的配置元数据中, 您可以使用 `<alias/>` 元素来完成该配置。

```xml
<alias name="fromName" alias="toName"/>
```
    
在使用此别名定义之后, 同名容器中名为 `fromName的bean` 也可以称为 `toName`。

例如, 子系统A的配置元数据可以通过名称 `subsystemA-dataSource` 引用 `DataSource`, 子系统B的配置元数据可以通过名称 `subsystemB-dataSource` 引用数据源。 在编写使用这两个子系统的主应用程序时, 主应用程序通过名称 `myApp-dataSource` 引用 `DataSource`。 要使所有三个名称引用添加到 `MyApp` 配置元数据的同一对象, 需要使用以下别名定义：

```xml
<alias name="subsystemA-dataSource" alias="subsystemB-dataSource"/>
<alias name="subsystemA-dataSource" alias="myApp-dataSource"/>
```

现在, 每个组件和主应用程序都可以通过一个唯一的名称引用 `DataSource`, 并保证不与任何其他定义冲突(有效的创建命名空间), 但它们引用相同的 Bean。

Java 配置：  
`@Bean` 注解能够提供类似别名的功能, 见 [使用 @Bean 注解](#7123-%E4%BD%BF%E7%94%A8-bean-%E6%B3%A8%E8%A7%A3)

##### 7.3.2 初始化 Bean

Bean 定的的本质是用于创建一个或多个对象。 容器在需要 Bean 时查 Bean 的命名, 并使用由该 Bean 定义封装的配置元数据来创建(或获取)实例。

如果使用基于 XML 配置元数据的方式, 择要指定在 `<bean/>` 元素的 `class` 属性中实例化的对象的类型。 此类属性通常是必须的, 它在内部是 `BeanDefine` 实例上的  `class` 属性。 查看 [Bean 定义继承](#77-bean-%E5%AE%9A%E4%B9%89%E7%BB%A7%E6%89%BF)。 通过以下两种方式使用 `class` 配置:

- 通常, 在容器本身通过反向调用其构造函数直接创建 bean 的情况下指定要构造的 bean 类, 稍微等同于使用 new 的 Java 创建实例。
- 在不太常见的情况下,  容器执行一静态工厂方法来创建 Bean, 需要指定能够创建这些对象的j静态工厂方法。 从静态工厂方法的调用返回的对象类型可以完全是同一个类或另一个类。

> ~~内部类名(inner class names)~~. 如果需要定义一个嵌套的 `static` 类, 则必须使用嵌套类的 ~~二进制名称(binary name)~~. 
> 比如有一个类 `Foo`, 在包 `com.example` 下, `Foo` 又一个嵌套的 `static` 类 `Bar`, `Bar` 的 `class` 属性值为: `com.example.Foo$Bar`。
> 嵌套类和外部类名称相同时, 也通过 `$` 区分。

- 通过构造函数创建实例

当通过构造函数创建一个实例, 所有普通类都可以使用并与 Spring 兼容。 也就是说, 正在开发的类不需要实现任何特定的接口或以特定的方式编码。 简单地指定 Bean 类就足够了。 但是, 根据该特定bean使用的IoC类型, 可能需要一个默认(空)构造函数。

IoC 容器实际上管理着需要管理的所有类, 它不仅管理真正的 JavaBean, 还能够在容器中管理更多给 Bean 样式的类。 如: 不符合 JavaBean 规范的就连接池。

XML 配置如下: 

```xml
<bean id="exampleBean" class="example.ExampleBean"/>
<bean id="anotherExample" class="example.AnotherExample"/>
```

有关为构造函数提供参数的机制以及在构造对象后设置对象实例属性的详细信息, 见[依赖注入](#741-%E4%BE%9D%E8%B5%96%E6%B3%A8%E5%85%A5)

- 通过静态工厂方法实例化

定义使用静态工厂方法创建 Bean 是, 可以通过包含静态工厂方法和属性的, ~~指定属性为工厂方法本身的名称 的类(you use the class attribute to specify the class containing the static factory method and an attribute named factory-method to specify the name of the factory method itself.)~~。 可以调用该方法并返回一个活动的对象, 这种方式视为通过构造函数创建对象。 这种 Bean 定义的一个用途是在一流代码中调用静态工厂。

以下 Bean 定义指定通过调用 `factory-method` 创建 Bean。 该定义未指定返回的类型, 仅指定包含工厂方法的类。 在此示例中, `createInstance()` 方法必须是静态方法。

```xml
<bean id="clientService"
    class="example.ClientService"
    factory-method="createInstance"/>
```

```java
public class ClientService {
    private static ClientService clientService = new ClientService();
    private ClientService() {}

    public static ClientService createInstance() {
        return clientService;
    }
}
```

有关向工厂方法提供 (可选) 参数以及在从工厂返回对象后设置对象实例属性的机制的详细信息见 [依赖关系和配置细节](#742-%E4%BE%9D%E8%B5%96%E5%85%B3%E7%B3%BB%E5%92%8C%E9%85%8D%E7%BD%AE%E7%BB%86%E8%8A%82)

- 通过实例工厂方法实例化

与通过静态工厂方法实例化类似, 使用实力工厂方法进行实例化会从容器调用现有 Bean 的非静态方法来创建新的 Bean, 使用该机制的条件是: 将类属性保留为空, 并在当前(or 父/祖先)容器 `factory-bean` 属性中指定 Bean 的名称, 该容器包含要调用来创建实例的方法。 使用 `factory-method` 属性设置工厂方法本身的名称。

```xml
<!-- 包含一个名为 createInstance() 的工厂 Bean -->
<bean id="serviceLocator" class="example.DefaultServiceLocator">
    <!-- 注入所有需要的 locator 的 Bean -->
</bean>

<!-- 创建 factory bean 的 bean -->
<bean id="clientService"
    factory-bean="serviceLocator"
    factory-method="createClientServiceInstance"/>
```

```java
public class DefaultServiceLocator {
    private static ClientService clientService = new ClientServiceImpl();

    public ClientService createClientServiceInstance() {
        return clientService;
    }
}
```

一个工厂类同样能够有多个工厂方法: 

```xml
<bean id="serviceLocator" class="example.DefaultServiceLocator">
    <!-- 注入所有需要的 locator 的 Bean -->
</bean>

<bean id="clientService"
    factory-bean="serviceLocator"
    factory-method="createClientServiceMethod"/>

<bean id="accountService"
    factory-bean="serviceLocator"
    factory-method="createAccountServiceInstance"/>
```

```java
public class DefaultServiceLocator {
    private static ClientService clientService = new ClientServiceImpl();

    private static AccountService accountService = new AccountServiceImpl();

    public ClientService createClientServiceMethod() {
        return clientService;
    }

    public AccountService createAccountServiceInstance() {
        return accountService;
    }
}
```

这种方法表明可以通过依赖注入(DI) 来管理配置 工厂Bean 本身。 见: [依赖关系和配置细节](#742-%E4%BE%9D%E8%B5%96%E5%85%B3%E7%B3%BB%E5%92%8C%E9%85%8D%E7%BD%AE%E7%BB%86%E8%8A%82)

#### 7.4 Dependencies

A typical enterprise application does not consist of a single object (or bean in the Spring parlance). Even the simplest application has a few objects that work together to present what the end-user sees as a coherent application. This next section explains how you go from defining a number of bean definitions that stand alone to a fully realized application where objects collaborate to achieve a goal.

##### 7.4.1 依赖注入

依赖注入 (DI) 是一个过程, 通过这个过程, 来对象定义它们的依赖关系, 即它们使用的其他对象, 只能通过构造函数参数, 工厂方法的参数或在构造或返回对象实例后在对象实例上设置的属性或者从工厂方法返回。 容器在创建这些 Bean 的时候注入这些依赖。 这个过程基本上是相反的: Bean 本身通过使用的类直接构造器或服务[定位器模式](https://blog.csdn.net/hb0746/article/details/51123748) 来控制其依赖想的实例化或位置, 这就是 IoC(控制反转) 名称的由来。

使用依赖注入的原理使代码更清晰, 当对象提供其依赖项时, 解耦会更有效。 对象不查找其依赖项, 也不知道依赖项的位置或类, 因此, 类变得容易测试(尤其是当依赖关系在接口或抽象类上时), 在单元测试中也能使用 stub 或 mock 实现。

> 注: [单元测试之Stub和Mock](https://www.cnblogs.com/TankXiao/archive/2012/03/06/2366073.html)

依赖注入有两种主要的变体: `基于构造方法的依赖注入` 和 `基于 Setter 的依赖注入`

- 基于构造方法的依赖注入

通过构造方法的依赖注入通过容器执行含参的构造函数来完成实例化, 每个参数表示一个依赖项。 调用具有特定参数的静态工厂方法来构造 Bean 几乎是等效的, 本讨论同样处理构造函数和静态工厂方法的参数。 以下示例为一个只能通过构造函数注入的类: 

```java
public class SimpleMovieLister {
    // bbb
    private MovieFinder movieFinder;

    // a constructor so that the Spring container can inject a MovieFinder
    public SimpleMovieLister(MovieFinder movieFinder) {
        this.movieFinder = movieFinder;
    }
    // business logic that actually uses the injected MovieFinder is omitted...
}
```

- 构造函数参数解析

使用参数的类型进行构造函数参数解析匹配。 如果 Bean 定义的构造器参数不存在潜在的歧义, 那么在 Bean 定义中定义构造函数参数顺序, 是在实例化 Bean 时, 将这些参数提供给适当的构造函数的顺序。 如: 

```java
package x.y;

public class Foo {
    public Foo(Bar bar, Baz baz) {
        // ...
    }
}
```

假设 `Bar` 和 `Baz` 类无继承关系, 则不存在潜在的歧义。 因此, 以下配置工作正常, 无需在 `<constructor-args/>` 元素中显示指定构造函数参数索引和类型。

```xml
<beans>
  <bean id="foo" class="x.y.Foo">
    <constructor-args reg="bar"/>
    <constructor-args reg="baz"/>
  </bean>
  <bean id="bar" class="x.y.Bar"/>
  <bean id="baz" class="x.y.Baz/>
</beans>
```

当引用另一个 Bean 时, 类型是已知的, 并可以进行配置(与前面的示例一样)。 当使用简单类型时(例如: `<value>true</value>`), Spring 无法确定值的类型, 因此无法在没有帮助的情况下按类型进行匹配。 如: 

```java
package examples;

public class ExampleBean {
    private int years;

    private String untimateAnswer;

    public ExampleBean(int years, String untimateAnswer) {
        this.years = years;
        this.untimateAnswer = untimateAnswer;
    }
}
```

在上述场景中, 如果使用 `type` 属性显示指定构造函数参数的类型, 则容器可以使用与简单类型匹配的类型。

```xml
<bean id="exampleBean" class="examples.ExampleClass">
  <constructor-arg type="int" value="7500000"/>
  <constructor-arg type="java.lang.String" value="42"/>
</bean>
```

通过 `index` 属性来显式指定构造函数参数的索引:

```xml
<bean id="exampleBean" class="examples.ExampleClass">
  <constructor-arg index="0" value="7500000"/>
  <constructor-arg index="1" value="42"/>
</bean>
```

除了解决多个简单值的歧义之外, 指定索引还可以解决构造函数具有相同类型的两个参数的歧义。 请注意，索引基于0。

也可以使用构造函数的参数名称来消除值的歧义:

```xml
<bean id="exampleBean" class="examples.ExampleBean">
  <constructor-arg name="years" value="7500000"/>
  <constructor-arg name="ultimateAnswer" value="42"/>
</bean>
```

也可以使用 `@ConstructorProperties` 的 JDK 注解显示命名构造函数, 如下: 

```java
package examples;

public class ExampleBean {
    // Fields omitted

    @ConstructorProperties({"years", "ultimateAnswer"})
    public ExampleBean(int years, String ultimateAnswer) {
        this.years = years;
        this.ultimateAnswer = ultimateAnswer;
    }
}
```

- 基于 Setter 的依赖注入

基于 `setter` 的依赖注入是在调用无参数构造函数或无参数静态工厂方法来实例化 Bean 之后, 通过容器调用 Bean 上的 `setter` 方法来完成。

以下示例显示了一个智能使用 `setter` 注入进行依赖注入的类, 它是一个POJO，它不依赖于容器特定的接口，基类或注释。

```java
public class SimpleMovieLister {
    private MovieFinder movieFinder;

    public void setMovieFinder (MovieFinder movieFinder) {
        this.moveiFinder = movieFinder;
    }
}
```

`ApplicationContext` 支持基于构造函数和基于 `setter` 的依赖注入的方式来管理 Bean。 ~~在通过构造函数方法注入了一些依赖后, 还支持基于 `setter` 的依赖注入(It also supports setter-based DI after some dependencies have already been injected through the constructor approach.)~~。 可以通过 `BeanDefinition` 的形式配置依赖项, 可以将其与 `PropertyEditor` 实例结合使用, 将属性从一种格式转换成另一种格式。 然而, 大多数的 Spring 用户不直接使用这些累(即以编程方式), 而是使用 `XML` 定义的形式、 注解的形式(如: `@Component`, `@Controller` 等注解类)、 或者基于 Java 的 `@Configuration` 类中的 `@Bean` 方法。 然后这些源在内部转换为 `BeanDefinition` 实例, 并用于加载整个 Spring IoC 容器实例。

> **基于构造函数还是基于 `setter`?**  
> 由于可以混合使用基于构造函数和基于 `setter` 的依赖注入, 因此将构造函数用于强依赖项, `setter` 方法 或 可选依赖项的配置用于可选依赖项。 注意: 在 `setter` 方法上使用 `@Required` 注解可以使该属性称为必须的依赖项。  
> Spring 团队提倡构造函数注入, 因为它们使应用程序组件能够实现为不可变对象, 并确保所需的依赖项不为空。 此外, 构造函数注入的组件始终以完全初始化的状态返回到调用端代码。 作为旁注, 大量的构造函数参数时一个 bad small, 暗示该类可能有太多的责任, 应当重构以更好的解决关注点的正确分离。  
> `setter` 注入主要用于可在类中指定合理默认值的可选依赖项。 否则必须在代码中使用依赖项的任何位置执行非空检查。 `setter` 注入的一个好处是 `setter` 方法使该类的对象可以在以后重新配置或重新注入。 因此, 通过 `JMX MBeab` 进行管理是二次注入的一个~~很不错的用例(compelling use case)~~。  
> 使用对特定类最有意义的依赖注入样式。 有时在处理没有源的第三方类时, `setter` 注入更合适, 如果第三方类没有公开 `setter` 方法, 那么构造函数注入可能是唯一可用的依赖注形式。

- 依赖关系过程

容器通过如下方式进行 Bean 的依赖解析: 

1. `ApplicationContext` 来创建和初始化描述所有 Bean 的配置元数据, 配置元数据可以基于 `XML`, Java 代码和 Java 注解。
2. 对于每个 Bean, 如果使用 依赖普通构造函数, 那么它的依赖关系将以属性, 构造函数参数 或 静态工厂方法的参数的形式表示。 实际创建 Bean 时, 会将这些依赖项提供给 Bean。
3. 每个属性或构造函数参数都是一个要设置的实际值的定义, 或者是对容器中另一个 Bean 的引用。
4. 作为值的每个属性或构造函数参数都从其指定个数转换为该属性的实际类型。 默认情况下, Spring 可以将以字符串格式提供的值转换为所有内置类型, 如 `int`, `long`, `String`, `boolean` 等。

Spring 容器在创建容器时验证每个 Bean 的配置, 然而, 在实际创建 Bean 之前不会设置 Bean 属性本身。 创建容器时 会创建单例作用域并设置为预先实例化的Bean。`Scope` 在 [Bean 范围](#75-bean-%E8%8C%83%E5%9B%B4) 中描述。 其他情况下, Bean 只在被需要的时候在被创建。 因为 Bean 的依赖关系以及其依赖的依赖关系被创建和分配, 创建 Bean 可能会导致创建 ~~Bean Graph~~。 **注意, 这些依赖项之间的分辨不匹配可能会显示比较晚, 即首次创建受影响的 Bean 创建时。**

> **循环依赖**
> 如果主要通过构造函数来注入, 那么可能会创建一个不可依赖的循环依赖.
> 如: 类 `A` 需要一个通过构造函数注入的 `B` 的实例, 类 `B` 需要一个通过构造函数注入的 `A` 的实例, 如果配置 `A` 和 `B` 分别被其中另一个注入, IoC 容器在运行时会检查出这种循环依赖, 并抛出一个 `BeanCurrentlyInCreationException` 异常。
> 一种可行的解决方案是主要通过 `setter` 注入的方式而不是通过构造函数来配置这些注入。 或者避免构造函数注入, 只通过 `setter` 注入。 换而言之, 尽管不推荐, 但是可以通过 `setter` 来配置注入。
> 和典型的情况不同(没有循环依赖关系), Bean `A` 和 Bean `B` 之间的循环依赖迫使再完全初始化之前将其中一个 Bean 注入到另一个 Bean 中(鸡生蛋/蛋生鸡场景)。

Spring 在容器装配是检测配置问题, 比如对不存在的 Bean 和循环依赖项的引用。 在实际创建 Bean 时, Spring 尽可能晚的设置属性冰解析依赖项。 这意味着 在创建对象时, 如果该对象或其依赖项出现问题时, 正确加载的 Spring 容器稍后可以在请求对象时生成异常。 比如: 由于缺少或无效的属性, Bean 会抛出异常。 一些配置问题可能会延迟可见性, 这就是为什么 `ApplicationContext` 实现默认的预实例化是单例的 `Bean`。 在实际需要这些 Bean 之前, 需要使用一些时间和内存来创建这些 Bean, 但是在创建 `ApplicationContext` 时, 可能会发现配置问题。 也可以不使用默认的单例的 Bean 的方式, 使用 `lazy-initialize` 的方式来预加载。

如果没有循环依赖存在, 当一个协作 Bean 被注入到另一个 Bean 中, 每个协作 Bean 被注入到依赖 Bean 之前都被 完全配置。 这意味着如果 Bean `A` 依赖 Bean `B`, 那么在独爱听 Bean `A` 上的 `setter` 方法之前, Spring 容器将完全配置 Bean `B`。 换而言之, Bean 实例化(如果不是一个预先实例化的单例), 设置其依赖项, 并调用相关生命周期方法(例如配置的 `init` 方法或 `initialization` 回调方法)。

- 依赖注入的例子

下面的示例通过基于 XML 的配置元数据来通过 `setter` 来配置依赖注入。 Spring XML 配置文件的一小部分指定了一些 Bean 定义:

```xml
<bean id="exampleBean" class="examples.ExampleBean">
  <!-- 通过嵌套 ref 元素来注入 setter -->
  <property name="beanOne">
    <ref bean="anotherExampleBean"/>
  </properties>
  <property name="beanTwo" ref="yetAnotherBean"/>
  <property name="integerProperty" value="1">
</bean>

<bean id="anotherExampleBean" class="examples.AnotherBean"/>
<bean id="yetAnotherBean" class="examples.YetAnotherBean"/>
```

```java
package examples;

public class ExampleBean {
    private AnotherBean beanOne;
    private YetAnotherBean beanTow;
    private int i;
    public void setBeanOne(AnotherBean beanOne) {
        this.beanOne = beanOne;
    }

    public void setBeanTow(YetAnotherBean beanTow) {
        this.beanTow = beanTow;
    }

    pyblic void setIntegerProperty(int i) {
        this.i = i;
    }
}
```

通过构造函数注入: 

```xml
<bean id="exampleBean" class="examples.ExampleBean">
  <constructor-args>
    <ref bean="anotherExampleBean"/>
  </constructor-args>
  <constructor-args ref="yetAnotherBean"/>
  <constructoe-atgs type="int" value="1"/>
</bean>

<bean id="anotherExampleBean" class="examples.AnotherBean"/>
<bean id="yetAnotherBean" class="examples.YetAnotherBean"/>
```

```java
package examples;

public class ExampleBean {
    private AnotherBean beanOne;
    private YetAnotherBean beanTwo;
    private int i;

    public ExampleBean(AnotherBean beanOne, YetAnotherBean beanTow int i) {
        this.beanOne = beanOne;
        this.beanTwo = beanTwo;
        this.i = i;
    }
}
```

这个例子的一个变体, Spring 不是使用构造函数, 而是被告知调用静态工厂来返回对象的实例:

```xml
<bean id="exampleBean" class="examples.ExampleBean" factory-method="createInstance">
  <constructor-args ref="anotherExampleBean"/>
  <constructor-args ref="yetAnotherBean"/>
  <constructor-args value="1"/>
</bean>

<bean id="anotherExampleBean" class="examples.AnotherBean"/>
<bean id="yetAnotherBean" class="examples.YetAnotherBean"/>
```

```java
package examples;

public class ExampleBean {
    private ExampleBean(...) {
        ...
    }

    public static ExampleBean createInstance(AnotherBean beanOne, YetAnotherBean beanTow int i) {
        ExampleBean eb = new ExampleBean(...);
        return eb;
    }
}
```

##### 7.4.2 依赖关系和配置细节

如前一节所述, 可以将bean属性和构造函数参数定义为对其他托管 Bean(或协作者) 的引用, 或者定义为内联的值。 Spring 基于 XML 的配置元数据支持其 `<property/>` 和 `<constructor-arg/>` 元素中的子元素类型。

- 连续值(基础类型, 字符串等)

`<property/>` 标签的 `value` 属性将属性或构造函数参数 指定为 可读的字符串表示的形式。 Spring 的转换服务用于将这些值从字符串转换为属性或参数的实际类型。

```xml
<bean id="myDataSource" class="org.apache.commons.dbcp.BasicDataSource" destroy-method="close">
  <property name="driverClassName" value="com.mysql.jdbc.Driver"/>
  <property name="url"
            value="jdbc:mysql://xxx:xxx/mydb"/>
  <property name="username" value="username"/>
  <property name="password" value="password"/>
</bean>
```

下面的例子通过 `p-namespace` 来实现更简洁的 XML 配置:

```xml
<beans xmlns="http://www.springframework.org/schema/beans"
  xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
  xmlns:p="http://www.springframework.org/schema/p"
  xsi:schemaLocation="http://www.springframework.org/schema/beans
  http://www.springframework.org/schema/beans/spring-beans.xsd">
  <bean id="myDataSource"
        class="org.apache.commons.dbcp.BasicDataSource"
        destory-method="close"
        p:driverClassName="com.mysql.jdbc.Driver"
        p:url="jdbc:mysql://xxx:xxx/mydb"
        p:username="username"
        p:password="password"/>
</beans>
```

还可以配置 `java.util.properies` 实例:

```xml
<bean id="mappings"
      class="org.springframework.beans.factory.config.PropertyPlaceholderConfigurer">
  <!-- 配置 java.util.Properties 类型 -->
  <property name="properties">
    <value>
      jdbc.driver.classNmam=com.mysql.jdbc.Driver
      jdbc.url=jdbc:mysql://xxx:xxx/mydb
    </value>
  </property>
</bean>
```

- `iderf` 元素

`iderf` 元素只是将容器中另一个 Bean 的 id(String 值, 而不是引用), 传递给 `<constructor-arg/>` 或者 `<property/>` 标签的一种错误防范方法:

```xml
<bean id="theTargetBean" class="..."/>

<bean id="theClientBean" class="...">
  <property name="targetName">
    <idref bean="theTargetBean"/>
  </property>
<bean/>
```

(在运行时) 上面的 Bean 定义片段等价于下面的片段:

```xml
<bean id="theTargetBean" class="..."/>
<bean id="client" id="...">
  <property name="targetName" value="theTargetBean"/>
</bean>
```

第一个变体与第二个变体相比, `idref` 标记允许容器在部署是验证引用名实际存在。 第二个变体, 不会传递给 `client` Bean 的 `targetName` 属性的值执行任何验证。 只有在实例化 `client` Bean 时才会发现语法错误(最有可能出现的致命原因)。 如果 `client` Bean 是一个原型的 Bean, 这种类型和产生的异常可能只会在容器部署很久后才发现。

> `local` 属性的 `idref` 元素在 4.0 Bean xsd 中不再支持, 因为它不再提供常规 Bean 引用的值。 升级到 4.0 时, 只需更改对 `idref local` 的引用为 `idref bean` 即可。

`<idref/>` 元素带来价值指出在 `ProxyFactioryBean` 定义的 [AOP 拦截器](#1252-aop-%E6%8B%A6%E6%88%AA%E5%99%A8) 中(至少在 Spring 2.0 之前的版本如此)。 在指定拦截器名称是使用 `<idref/>` 元素可以防止错误的拼写拦截器 id。

- 其他的 Bean(协作)

`ref` 元素是 `<constructor-arg/>` 或 `<property/>` 定义元素中的最后一个元素。 在这里可以将 Bean 的置顶属性设置为对另一个 Bean(协作者) 的引用。 引用的 Bean 是 `将其设置为属性的 Bean` 的依赖项, 在设置属性之前根据需要对其进行初始化。(如果是单例的 Bean, 容器可能已经将其初始化)。 所有引用最终都是对另一个对象的引用。 范围和验证取决于是否通过 `bean`, `local` 或 `parent` 属性指定另一个对象的 id/name。

通过 `<ref/>` 标记的 Bean 属性指向目标 Bean 是最通用的形式, 并允许在相同容器或父容器中创建对任何 Bean 的引用而这些 Bean 不用必须在相同的 XML 文件中。 Bean 属性的值可能与目标 Bean 的属性 `id` 相同, 或者与目标 Bean 的 `name` 属性的值相同。

```xml
<ref bean="someBean"/>
```

通过父属性指定目标属性将创建对当前容器的父容器的 Bean 的引用。 父属性的值可能与目标 Bean 的 `id` 属性相同, 或者是目标 Bean 的 `name` 属性的一个值, 而目标 Bean 必须位于当前的父容器中。 当容器使用某种层次结构, 并且希望将一个现有的 Bean 封装在一个父容器中时, 主要使用的是这个 Bean 的引用变量。

```xml
<!-- 父容器 -->
<bean id="accountService" class="com.foo.SimpleAccountService"/>
  <!-- 必要的依赖 -->
</bean>
```

```xml
<!-- 子容器 -->
<!-- 与父容器中的 Bean 的 id 相同 -->
<bean id="accountService"
  class="org.springframework.aop.ProxyFactoryBean">
  <property name="target">
    <!-- 引用 parent bean -->
    <ref parent="accountService"/>
  </property>
</bean>
```

- 内部 Bean

一个在 `<property/>` 或 `<constroctor-args/>` 元素中的 `<bean/>` 元素定义了一个内部 `bean`。

```xml
<bean id="outer" class="...">
  <!-- 直接内联的定义目标 Bean, 代替使用对目标 Bean 的引用 -->
  <property name="target">
    <bean class="com.example.Person">
      <!-- 内部 Bean -->
      <property name="name" value="Fiona Applke"/>
      <property name="age" value="25">
    </bean>
  </propertu>
</bean>
```

一个内部 Bean 的定义不用必须包含 id 或者 name, 如果指定, 容器不会使用这个值作为标识符。 容器在创建时也忽略 `scope` 标识: 内部 Bean 总是和外部 Bean 一起创建, 且名称不公开。 除了将内部 Bean 注入到封闭意外事件, 无法将内部 Bean 注入到协作 Bean 中, 也不能独立的访问。

- 集合

可以通过 `<list/>`, `<set/>`, `<map/>`, `<props/>` 元素来配置参数是集合类型 `List`, `Set`, `Map` 或 `Properties`。

```xml
<bean id="moreConplexObject" class="example.ComplexObject">
  <property name="adminEmails">
    <props>
      <prop key="administrator">administrator@example.ort</prop>
      <prop key="support">support@example.ort</prop>
      <prop key="development">development@example.ort</prop>
    </props>
  </property>
  <property name="someList">
    <list>
      <value>a list element followed by a reference</value>
      <ref bean="myDataSource"/>
    </list>
  </property>
  <property name="someMap">
     <map>
       <entry key="an entry" value="just some string"/>
       <entry key="a ref" value-ref="myDataSource"/>
     </map>
  </property>
  <property name="someSet">
    <set>
      <value>just some string</value>
      <ref bean="myDataSource"/>
    </set>
  </property>
</bean>
```

Map 和 Set 的键值可以是以下的类型:

```text
bean | ref | idref | list | set | map | props | value | null
```

- 集合的合并

Spring 容器提供合并的集合。 通过在一个 `<list/>`, `<map/>` ,`<set/>` 或 `<props/>` 元素中定义一个  `<list/>`, `<map/>` ,`<set/>` 或 `<props/>` 的子元素来实现。

```xml
<beans>
  <bean id="parnet" abstract="true" class="com.example.ComplexObject">
    <property name="adminEmails">
      <props>
        <prop key="administrator">administrator@example.org</prop>
        <prop key="support">support@example.org</prop>
      </props>
    </property>
  </bean>
  <bean id="child" parent="parent">
    <property name="adminEmails">
      <props merge="true">
        <prop key="sales">sales@example.org</prop>
        <prop key="support">support@example.co.uk</prop>
      </props>
    </property>
  </bean>
</beans>
```

通过在 `child` Bean 的 `adminEmails` 属性的 `<props/>` 元素的 `merge=true` 属性定义的使用。 当 `child` Bean 被容器解析并实例化时, 返回的实例有一个 `adminEmails` 的 `Properties` 集合, 包括合并了 `child` 的 `adminEmails` 集合到 parent Bean 中的 `adminEmails` 集合的集合:

```text
administrator=administrator@example.org
sales=sales@example.org
support=support.example.co.uk
```

`<list/>`, `<map/>`, `<set/>` 集合类型提供和上述方式一样的合并行为。 在 `<list/>` 元素的情况下(与 `List` 集合类型相关的语义, 即有序集合的值的概念), parent `List` 的值先于所有子 `List` 的值。 对于 `Map`, `Set` 和 `Properties` 类型, 不存在排序, 对容器内部使用 `Map`, `Set` 和 `Properties` 的集合类型时, 没有有效的排序语义。

- 限制合并集合

- 强类型集合

- Null 和空字符串值

- 具有 `p-namespace` 的快捷方式

- 具有 `c-namespace` 的快捷方式

- 复合属性名

##### 7.4.4 Lazy-initialization Bean

#### 7.5 Bean 范围

#### 7.7 Bean 定义继承

#### 7.9 基于 Java 注解的容器配置

#### 7.12 基于 Java 代码的容器配置

##### 7.12.3 使用 @Bean 注解

### 8. Resources

### 9. Validation, Data Binding, and Type Conversion

### 10. Spring Expression Language

### 11. Aspect Oriented Programming with Spring

#### 11.8 通过 AspectJ 和 Spring 来依赖注入 domain 对象

### 12. Spring AOP APIs

#### 12.5 Using the ProxyFactoryBean to create AOP proxies

##### 12.5.2 AOP 拦截器

## IV. Test

### 13. Introduction to Spring Testing

### 14. Unit Test

### 15. Integration Test

### 16. Further Resources

## V. Data Access

### 17. Transaction Manager

### 18. DAO support

### 19. Data access with JDBC

### 20. Object Relational Mapping(ORM) Data Access

#### 20.1. Introduction to ORM with Spring

#### 20.2. General ORM integration consideration

#### 20.3. Hibenrate

#### 20.4. JDO

#### 20.5. JPA

### 21 Marshalling XML using O/X Mappers

## VI. The Web

### 22. Web MVC framework

### 23. View Technologies

### 24. Integrating with other web frameworks

### 25. Portlet MVC Framework

### 26. WebSocket Support

### 27. CORS Support

## VII. Integration

### 28. Remoting and web service using Spring

### 29. Enterprise JavaBeans(EJB) integration

### 30. JMS

### 31. JMX

### 32. JCA CCI

### 33. Email

### 34. Task Execution and Schedule

### 35. Dynamic language support

### 36. Cache Abstraction

## VIII. Appendices

### 37. Migrating to Spring Framework 4.x

### 38. Spring Annotation Programming Model

### 39. Classic Spring usage

### 40. Classic Spring AOP usage

### 41. XML Schema-based configuration

### 42. Extensible XML authoring

### 43. Spring JSP Tag Library

### 44. Spring-form JSP Tag Library
