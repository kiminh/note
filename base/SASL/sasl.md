# SASL Java

简单认证和安全层 (SASL) 定义了一种用于认证以及在客户端和服务器应用之间建立安全层的协议, 其中安全层的建立是可选的。 SASL 定义了如何交换认证数据, 但没有指定这些认证数据的内容。它只是一个框架, 认证数据的内容和语义由认证机制指定。

SASL 通过 LDAP v3 和 IMAP v4 协议来确保鉴权是可插拔的。而不是硬编码一个鉴权方法到协议。

Java SASL 是 J2SE 5.0 的新增内容, 它以独立于认证机制的方式定义Java API 机制, 让使用该 API 的应用无需指定特定的 SASL 机制。该 API 为客户端和服务器应用带来了便利, 使应用可以根据所需的安全功能选择需要使用的机制, 如是否易受被动字典攻击和是否接受匿名认证。Java SASL API 还允许开发人员创建自定义的 SASL 机制。SASL 机制可以使用 JCA 安装。

SASL为网络应用程序提供可插拔的认证和安全层。 Java SE中还提供类似功能的其他功能，包括Java安全套接字扩展 (JSSE) 和 Java 通用安全服务 (Java GSS)。 JSSE为Java语言版本的SSL和TLS协议提供了框架和实现。 Java GSS 是通用安全服务应用程序编程接口（GSS-API）的Java语言绑定。目前在 Java SE 上的此 API 下支持的唯一机制是 Kerberos v5。

与 JSSE 和 Java GSS 相比，SASL相对较轻，在最近的协议中受欢迎。它还具有几个流行的，轻量的（在基础设施支持方面）SASL机制的定义。另一方面，主 JSSE 和 Java GSS 机制具有相对较重的机制，需要更复杂的基础设施（公钥基础设施和 Kerberos）。

SASL，JSSE 和 Java GSS 通常一起使用。 例如, 一个常见的模式是使应用程序使用 JSSE 建立安全通道, 并将 SASL 用于客户端, 基于用户名/密码的身份验证。 还有 GSS-API 机制之上分层的 SASL 机制; 一个流行的例子是与 LDAP 一起使用的 SASL GSS-API/Kerberos v5 机制。

除了从头开始定义和构建协议之外, 通常决定要使用哪个API的最大因素是协议定义。 例如, LDAP 和 IMAP 被定义为使用 SASL, 因此与这些协议相关的软件应该使用 Java SASL API。 在构建 Kerberos 应用程序和服务时, 要使用的 API 是 Java GSS。 当构建使用 SSL/TLS 作为协议的应用程序和服务时, 使用的 API 是 JSSE。 有关何时使用 JSSE 与 Java GSS 的更多详细信息, 请参阅Java安全性文档。




参考: 
- [The Java SASL API Programming and Deployment Guide](https://docs.oracle.com/javase/8/docs/technotes/guides/security/sasl/sasl-refguide.html)


名词解释: 

- JSSE: 即Java Secure Socket Extension, 是基于安全算法和握手机制之上的合成体。参考 [Java Secure Socket Extension wikipedia](https://en.wikipedia.org/wiki/Java_Secure_Socket_Extension)
- LDAP
- IMPA
