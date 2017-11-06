1. 配置 control 代码
2. IOC
3. AOP(日志)

## base

spring 框架通过反射根据 `bean`元素的 `class`属性指定的雷鸣创建了一个 java 对象, 并且以 `bean` 元素的 `Id` 属性值为 `Key` , 将该对象放入 spring 容器中, 这个java对象就成了spring容器的 `Bean.property` 子元素，执行一次 `setter`。

  > 特点及使用
    1. 通过 `classpathxmlApplicationContext` 与 `FileSystemXmlApplicationContext` 创建 spring
    2. 是程序不再使用new调用构造器创建java对象
    3. 通过无参数的构造器创建一个 `bean` 实例, 然后调用对于的 `setter` 方法注入依赖关系, 而构造注入则直接调用有参数的构造器, 当 `bean` 实例创建完成后, 已经完成了依赖关系的注入
    4. 所有组件被 `spring` 以 `bean` 的方法管理
    5. spring 配置文件必须指定 `bean` 实例的实现类

  > 大部分时候采用 `applicationContext`实例座位容器, `applicationContext` 是 `beanFatory` 接口的子接口。增强了 `beanFactory` 的功能
    1. 以声明式方式启动并创建 spring 容器
    2. 同时加载多个配置文件
    3. 初始化时会预初始化所有单例的 `bean`
    4. 支持国际化
    5. 包括后者所有的功能

  > points
   1. 调用构造器创建对象用 `<bean>`
   2. 调用 `getter` 方法, 用 `propertyPathFactoryBean` 或者 `<util:property-path/>`
   3. 调用普通方法: 用 `MethodInvokingFactoryBean` 工厂 `Bean`
   4. 获取 `Filed` 的值, 用 `FiledRetrievingFactoryBean` 工厂 `Bean`
   5. `ApplicationContext` 可以自动检测到容器中的容器后处理器, 并且自动注册容器后处理器:
   6. 容器后处理器:
    1. `propertyPlaceholderConfigurer`: 属性占位符配置器
    2. `propertyOverrideConfigurer`: 重写占位配置器 (每条属性应该保持: `beanId.property=value`)
    3. `customAutowireConfigurer`: 自定义自动装配的配置器
    4. `customScopeConfigurer`: 自定义作用域的配置器
   6. `@Resource` 作用:
    1. 为目标Bean指定写作者Bean
    2. `resource` 有一个 `name` 属性, 使用 `@Resource` 与 `<property../>` 元素的 `ref` 有相同的效果。
    3. `@Autowired` 自动装配, 默认采用 `ByType` 自动装配策略
    4. 如果正好找到一个 `Bean`, 就会执行 `set` 方法; 如果找到多个 spring 会引发异常。如果没有找到, 什么都不执行。
      - spring 需要进行资源访问时, 实际上并不需要直接使用 `Resource` 实现类, 而是调用 `ResourceLoader` 实例的 `getResource()` 方法来获取资源, `ResourceLoader` 将会负责选择 `Resource` 的实现类, 也就是确定具体的资源访问策略, 从而将应用程序和具体的资源访问策略分离开来, 这就是典型的策略模式。
   7. `ApplicationContext` 资源访问策略:
    1. 使用 `ApplicationConext` 实现类指定访问策略
    2. 使用前缀制定访问策略, 但是只对当前有效, 程序后面进行资源访问时, 还会根据 `ApplicationContext` 的实现类来选择对应的资源访问策略
   8. `web.xml` 初始化过程:
    1. 在启动 `Web` 项目时, 容器(如: Tomcat)会读 `web.xml` 配置文件中的两个节点 `<listener>` 和 `<contex-param>`
    2. 容器创建一个 `ServletContext` (上下文), 应用范围内即整个 Web 项目都能使用这个上下文
    3. 容器会将读取到 `<context-param>` 转化为 `键-值` 对, 并交给 `ServletContext`
    4. 容器创建 `<listener></listener>` 中的类实例, 即创建监听(备注: `listener` 定义的类可以是自定义的类但必须需要继承 `ServletContextListener`)
    5. 在监听的类中会有一个 `contextInitialized(ServletContextEvent event)` 初始化方法, 在这个方法中可以通过 `event.getServletContext().getInitParameter("contextConfigLocation")` 来得到 `context-param` 设定的值。在这个类中还必须有一个`contextDestroyed(ServletContextEvent event)` 来销毁方法, 用于关闭应用前释放资源, 比如说数据库连接的关闭
    6. 得到这个 `context-param` 的值之后, 就可以执行一些操作了。** 注意, 这个时候你的WEB项目还没有完全启动完成, 这个动作会比所有的Servlet都早 **

### 核心类和接口

1. DispatcherServlet 前置控制器。
2. HandlerMapping (interface) 处理请求的映射
  1. SimpleUrlHandlerMapping (HandlerMapping 接口的实现类) 通过配置文件, 把一个URL映射到Controller。
  2. DefaultAnnotationHandlerMapping (HandlerMapping 接口的实现类), 通过注解，把一个URL映射到Controller类上。
3. HandlerAdapter (interface) 处理请求的映射
  1. AnnotationMethodHandlerAdapter (HandlerAdapter 接口的实现类), 通过注解，把一个URL映射到Controller类的方法上。
4. Controller (interface) 控制器, 添加 ```@Controller``` 注解注解的类就可以担任控制器 (Action) 的职责。
5. HandlerInterceptor (interface) 拦截器, 实现这个接口, 来完成拦截的器的工作。
6. ViewResolver (interface)
  1. UrlBasedViewResolver (ViewResolver 接口的实现类), 通过配置文件，把一个视图名交给到一个View来处理。
  2. InternalResourceViewResolver (ViewResolver 接口的实现类), 比上面的类，加入了JSTL的支持。
7. View (interface)
  1. JstlView
8. LocalResolver
9. HandlerExceptionResolver(interface) 异常处理类
  1. SimpleMappingExceptionResolver (HandlerExceptionResolver 接口的实现类)。
10. ModelAndView

### 核心流程图

![spring](imgs/spring_core.png)

### DispatcherServlet 说明

使用 Spring MVC, 配置 DispatcherServlet 是第一步。  
DispatcherServlet 是一个 Servlet, 所以可以配置多个 DispatcherServlet。
DispatcherServlet 是前置控制器, 配置在 web.xml 文件中的。拦截匹配的请求, Servlet 拦截匹配规则要自已定义, 把拦截下来的请求, 依据某某规则分发到目标 Controller(写的 Action ) 来处理。

“某某规则”: 是根据你使用了哪个 HandlerMapping 接口的实现类的不同而不同

example:

```xml
<web-app>  
    <servlet>  
        <servlet-name>example</servlet-name>  
        <servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>  
        <load-on-startup>1</load-on-startup>  
    </servlet>  
    <servlet-mapping>  
        <servlet-name>example</servlet-name>  
        <url-pattern>*.form</url-pattern>  
    </servlet-mapping>  
</web-app>
```

`<load-on-startup>1</load-on-startup>` 是启动顺序, 让这个 `Servlet` 随 `Servletp` 容器一起启动。  
`<url-pattern>*.form</url-pattern>` 会拦截 `*.form` 结尾的请求。  
`<servlet-name>example</servlet-name>` 这个 `Servlet` 的名字是 `example`, 可以有多个 `DispatcherServlet`, 是通过名字来区分的。每一个 `DispatcherServlet` 有自己的 `WebApplicationContext` 上下文对象, 同时保存的 `ServletContext` 中和 Request 对象中。

在 `DispatcherServlet` 的初始化过程中, 框架会在 web 应用的 WEB-INF文件夹下寻找名为[servlet-name]-servlet.xml 的配置文件，生成文件中定义的bean

## 基本依赖

GroupId | ArtifactId | Description
---|---|---
org.springframework | spring-aop |
org.springframework | spring-aspects |
org.springframework | spring-bean | 包含 Groovy, 处理 bean
org.springframework | spring-context | 运行时上下文
org.springframework | spring-context-support |
org.springframework | spring-core | spring 核心模块
org.springframework | spring-expression |
org.springframework | spring-instrument |
org.springframework | spring-instrument-tomcat |
org.springframework | spring-jdbc | 提供 JDBC, 引入 JDBC
org.springframework | spring-jms | 提供 JMS 支持, 帮助获取 JMS 信息
org.springframework | spring-messaging |
org.springframework | spring-orm |
org.springframework | spring-omx |
org.springframework | spring-test |
org.springframework | spring-tx |
org.springframework | spring-web | 
org.springframework | spring-webmvc |
org.springframework | spring-webmvc-protlet |
org.springframework | spring-websocket |
