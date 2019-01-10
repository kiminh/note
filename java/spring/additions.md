Spring4 支持 Java8 的一些特性。你可以在 Spring 的回调接口中使用 lambda 表达式 和 方法引用。支持 java.time (JSR-310) 的值类型和一些改进过的注解, 例如 `@Repeatable`。你还可以使用 Java8 的参数名称发现机制 (基于-parameters编译器标志)。

Spring 仍然兼容老版本的 Java 和 JDK: Java SE 6 (具体来说，支持JDK6 update 18) 以上版本, 我们建议新的基于 Spring4 的项目使用Java7或Java8。

Spring Http 请求客户端: `org.springframework.web.client.RestTemplate`, 相关 Http 客户端操作在 `org.springframework.web.client` package 中。
