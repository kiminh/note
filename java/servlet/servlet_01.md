# Servlet

概念：Java Servlet 是运行在 Web 服务器或应用服务器上的程序，它是作为来自 Web 浏览器或其他 HTTP 客户端的请求和 HTTP 服务器上的数据库或应用程序之间的中间层。

- Servlet 在 Web Service 中的位置：

![image](../imgs/servlet-arch.jpg)

- Servlet 任务：  
    Servlet 主要执行以下任务：

1. 读取客户端（浏览器）发送的显式的数据。这包括网页上的 Html 表单，或者也可以是来自 applet 或自定义的 HTTP 客户端程序的表单。
2. 读取客户端（浏览器）发送的隐式的 HTTP 请求数据。这包括 cookies、媒体类型和浏览器能理解的压缩格式等等。
3. 处理数据并生成结果。这个过程可能需要访问数据库，执行 RMI 或 [CORBA](http://baike.baidu.com/link?url=5J_ptampsX1UkSP7TLj6H39h9dS26DyFnVki1xmIzotdCmqtzsdPf1PUBUA2Fk3djR_fSDdflzihBhd12idXKq) 调用，调用 Web 服务，或者直接计算得出对应的响应。
4. 发送显式的数据（即文档）到客户端（浏览器）。该文档的格式可以是多种多样的，包括文本文件(HTML 或 XML)、二进制文件(GIF 图像)、Excel 等。
5. 发送隐式的 HTTP 响应到客户端（浏览器）。这包括告诉浏览器或其他客户端被返回的文档类型（例如 HTML），设置 cookies 和缓存参数，以及其他类似的任务。

- Servlet Package

Java Servlet 是运行在带有支持 Java Servlet 规范的解释器的 web 服务器上的 Java 类。Servlet 可以使用 javax.servlet 和 javax.servlet.http 包创建，它是 Java 企业版的标准组成部分，Java 企业版是支持大型开发项目的 Java 类库的扩展版本。

## Servlet 生命周期

- Servlet 生命周期可被定义为从创建直到毁灭的整个过程：

1. Servlet 通过调用 init () 方法进行初始化。
2. Servlet 调用 service() 方法来处理客户端的请求。
3. Servlet 通过调用 destroy() 方法终止（结束）。
4. 最后，Servlet 是由 JVM 的垃圾回收器进行垃圾回收的。

Servlet 生命周期:

![image](../imgs/Servlet-LifeCycle.jpg)

- init 方法

    init() 方法被设计成只调用一次。它在第一次创建 Servlet 时被调用，在后续每次用户请求时不再调用。因此，它是用于一次性初始化。  
    Servlet 创建于用户第一次调用对应于该 Servlet 的 URL 时，也可以指定 Servlet 在服务器第一次启动时被加载。  
    当用户调用一个 Servlet 时，就会创建一个 Servlet 实例，每一个用户请求都会产生一个新的线程，适当的时候移交给 doGet 或 doPost 方法。init() 方法简单地创建或加载一些数据，这些数据将被用于 Servlet 的整个生命周期。

- service() 方法

    service() 方法是执行实际任务的主要方法。Servlet 容器（即 Web 服务器）调用 service() 方法来处理来自客户端（浏览器）的请求，并把格式化的响应写回给客户端。  
    每次服务器接收到一个 Servlet 请求时，服务器会产生一个新的线程并调用服务。service() 方法检查 HTTP 请求类型（GET、POST、PUT、DELETE 等），并在适当的时候调用 doGet、doPost、doPut，doDelete 等方法。
    service() 方法特征：

``` java
public void service(ServletRequest request, ServletResponse response) 
      throws ServletException, IOException{
}
```

- doGet() 方法：

    GET 请求来自于一个 URL 的正常请求，或者来自于一个未指定 METHOD 的 HTML 表单，它由 doGet() 方法处理。

```java
public void doGet(HttpServletRequest request,
                  HttpServletResponse response)
    throws ServletException, IOException {
    // doGet code
}
```

- doPost() 方法：

    POST 请求来自于一个特别指定了 METHOD 为 POST 的 HTML 表单，它由 doPost() 方法处理。

```java
public void doPost(HttpServletRequest request,
                   HttpServletResponse response)
    throws ServletException, IOException {
    // doPost code
}
```

- destroy() 方法

    destroy() 方法只会被调用一次，在 Servlet 生命周期结束时被调用。destroy() 方法可以让您的 Servlet 关闭数据库连接、停止后台线程、把 Cookie 列表或点击计数器写入到磁盘，并执行其他类似的清理活动。

```java
public void destroy() {
    // destroy code
}
```

## web.xml

限于表达能力，该部分为 [The Deployment Descriptor: web.xml](https://cloud.google.com/appengine/docs/standard/java/config/webxml) 翻译版本。

Java Web 应用程序使用部署描述符文件来确定 URL 如何映射到 servlet，哪些URL需要身份验证以及其他信息。 该文件名为 web.xml，驻留在 ```WEB-INF/``` 目录下的应用程序 WAR 中。 web.xml 是 Web 应用程序的 servlet 标准的一部分。

- 部署描述

Web 应用程序的部署描述符描述了应用程序的类，资源和配置以及 Web 服务器如何使用它们来提供 Web 请求。 当Web服务器接收到应用程序的请求时，它使用部署描述符将请求的URL映射到应该处理请求的代码。

部署描述符是一个名为 web.xml 的文件。 它驻留在 ```WEB-INF/``` 目录下的应用程序 WAR 中。 该文件是一个 xml 文件，其根元素是 ```<web-app>```。

web.xml 实例：

```xml
<web-app xmlns="http://xmlns.jcp.org/xml/ns/javaee"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://xmlns.jcp.org/xml/ns/javaee http://xmlns.jcp.org/xml/ns/javaee/web-app_3_1.xsd"
         version="3.1">
  <servlet>
        <servlet-name>DisplayHeader</servlet-name>
        <servlet-class>com.tangl.demo.filter.DisplayHeader</servlet-class>
    </servlet>
    <servlet-mapping>
        <servlet-name>DisplayHeader</servlet-name>
        <url-pattern>/*</url-pattern>
    </servlet-mapping>
</web-app>
```

- Servlet 和 URL 路径

web.xml 定义了 URL 路径和使用这些路径处理请求的 servlet 之间的映射。 Web 服务器使用此配置来标识处理给定请求的 servlet ，并调用与请求方法相对应的类方法（例如，用于 HTTP GET 请求的 doGet() 方法）。

要将 URL 映射到 servlet，需要使用 ```<servlet\>``` 元素声明 servlet ，然后使用 ```<servlet-mapping\>``` 元素定义从 URL 路径到 servlet 声明的映射。

```<servlet\>``` 元素声明 servlet ，包括用于引用该文件中其他元素的 servlet 的名称，用于 servlet 的类以及初始化参数。可以使用不同的初始化参数声明多个 servlet 使用相同的类。 每个 servlet 的名称在部署描述符中必须是唯一的。

```xml
<servlet>
    <servlet-name>redteam</servlet-name>
    <servlet-class>mysite.server.TeamServlet</servlet-class>
    <init-param>
        <param-name>teamColor</param-name>
        <param-value>red</param-value>
    </init-param>
    <init-param>
        <param-name>bgColor</param-name>
        <param-value>#CC0000</param-value>
    </init-param>
</servlet>

<servlet>
    <servlet-name>blueteam</servlet-name>
    <servlet-class>mysite.server.TeamServlet</servlet-class>
    <init-param>
        <param-name>teamColor</param-name>
        <param-value>blue</param-value>
    </init-param>
    <init-param>
        <param-name>bgColor</param-name>
        <param-value>#0000CC</param-value>
    </init-param>
</servlet>
```

```<servlet-mapping\>``` 元素指定一个 URL 模式和一个声明的 servlet 的名称，用于 URL 匹配该模式的请求。 URL 模式可以在模式的开头或结尾使用通配符（*）来指示零个或多个任意字符。 （该标准不支持字符串中的通配符，并且不允许在一个模式中使用多个通配符。）该模式匹配 URL 的完整路径，从域名开始并包含 "/"。

```xml
<servlet-mapping>
    <servlet-name>redteam</servlet-name>
    <url-pattern>/red/*</url-pattern>
</servlet-mapping>

<servlet-mapping>
    <servlet-name>blueteam</servlet-name>
    <url-pattern>/blue/*</url-pattern>
</servlet-mapping>
```

- JSPs

TODO 暂时不了解

- Secure URLs

TODO 暂时不了解

- The Welcome File List

TODO

- Filters

Filter 并不是一个标准的 Servlet，它不能处理用户请求，也不能对客户端生成响应。 主要用于对 HttpServletRequest 进行预处理，也可以对 HttpServletResponse 进行后处理。

过滤器类实现 javax.servlet.Filter 接口，包括 doFilter() 方法。简单示例：

```java
package mysite.server;

import java.io.IOException;
import java.util.logging.Logger;
import javax.servlet.Filter;
import javax.servlet.FilterChain;
import javax.servlet.FilterConfig;
import javax.servlet.ServletException;
import javax.servlet.ServletRequest;
import javax.servlet.ServletResponse;

public class LogFilterImpl implements Filter {

    private FilterConfig filterConfig;
    private static final Logger log = Logger.getLogger(LogFilterImpl.class.getName());

    public void doFilter(ServletRequest request, ServletResponse response, FilterChain filterChain)
        throws IOException, ServletException {
        log.warning("Log filter processed a " + getFilterConfig().getInitParameter("logType")
            + " request");

        filterChain.doFilter(request, response);
    }

    public FilterConfig getFilterConfig() {
        return filterConfig;
    }

    public void init(FilterConfig filterConfig) {
        this.filterConfig = filterConfig;
    }

    public void destroy() {}

}
```

```web.xml``` 配置：

```xml
<filter>
    <filter-name>logSpecial</filter-name>
    <filter-class>mysite.server.LogFilterImpl</filter-class>
    <init-param>
        <param-name>logType</param-name>
        <param-value>special</param-value>
    </init-param>
</filter>
```

```xml
<!-- Demo for filter-mapping element -->
<!-- Log for all URLs ending in ".special"-->
<filter-mapping>
    <filter-name>logSpecial</filter-name>
    <url-pattern>*.special</url-pattern>
</filter-mapping>

<!-- Log for all URLs that use the "comingsoon" servlet-->
<filter-mapping>
    <filter-name>logSpecial</filter-name>
    <url-pattern>comingsoon</url-pattern>
</filter-mapping>
```

- Error Handlers

TODO

## Servlet 实例
