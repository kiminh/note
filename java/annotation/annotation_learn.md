# Java annotation

翻译自: [Java Annotations – Annotations in Java](https://www.journaldev.com/721/java-annotations)

Java 注解是从 Java1.5 开始引入的, 现在广泛的被用于 各种Java EE 的框架中, 如: [Spring](https://www.journaldev.com/2888/spring-tutorial-spring-core-tutorial), Hibernate, [Jersey](https://www.journaldev.com/498/jersey-java-tutorial)

Java 注解是有 关嵌入到程序本身的 程序 的元数据, 它能够被注解解析器解析和编译器解析, 还可以定义注解的作用范围和作用时间。(原文仅提到作用时间)

在java注释之前，程序元数据可以通过java注释或javadoc获得，但是注释提供了更多。 注释元数据也可以在运行时使用，注释解析器可以使用它来确定流程。

## 自定义注解

- 注意：
    1. 注解的方法不能有参数。
    2. 注解的返回类型只能是原始数据类型、String、枚举、注解或者以上这些类型的数组。
    3. 注解的方法可以有一个默认的值。
    4. 注解可以附加元注解, 元注解一般为注解提供一些关于该注解的信息。

### 元注解

- 4种类型
    1. `@Documented` 表示使用此注解的元素应由 `javadoc` 和类似工具记录。 此类型应用于注解类型的声明, 其胡姐会影响其客户端对带注解元素的使用。 如果使用 Documented 注解类型声明, 则其注解将成为带注释元素的公共API的一部分。
    2. `@Target` 表示注解适用的程序元素的种类, 如: `TYPE`、 `METHOD`、 `CONSTRUCTOR`、 `FIELD` 等。如果不存在 Target 元注解，则可以在任何程序元素上使用注解。
    3. `@Inherited` 表示自动继承注解类型。 如果用户在类声明上查询注解类型，并且类声明没有此类型的注解，则将自动查询类的超类以获取注解类型。 将重复此过程，直到找到此类型的注解，或者到达类层次结构（对象）的顶部。(???)
    4. `@Retention` 指示要保留带注解类型的注解的时间范围。 它需要 RetentionPolicy 参数, 其可能值为 `SOURCE`, `CLASS和` 和 `RUNTIME`

## Java 内建注解

1. `@Override` 在重写父类方法或者实现接口方法的时候, 应用该注解告诉编译器这个方法被重写了。 如果超类的方法被删除或改变了, 编译器将会提示错误。
2. `@Deprecated` 当我们需要告诉编译器, 一个方法过时了, 应当使用这个注解来修饰该方法, 并提供为什么这个方法过时的信息和新的用法。
3. `@SuppressWarnings` 告诉编译器忽略掉一些明确的 `warning`
