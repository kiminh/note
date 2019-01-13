# lambda learning

函数式接口(functional interface) 是一个包含方法的接口。 如 `java.lang.Runnable` 和 `java.util.Comparator`。 lambda 是函数式接口的实现。

提供 `@FunctionalInterface` 作为注解, 这个注解非必须, 只要符合函数式接口的标准(只包含一个方法的接口), 虚拟机会自行判断, 但最好声明避免团队成员误操作。

## lambda 语法

1. `(parameters) -> {statements;}`&`(parameters) -> expression`: `(parameters)` 中为函数式接口方法的参数, 以逗号隔开, `->`, `{}` 是方法体, 可以是表达式和代码块。

2. 方法引用(::)

### 临时

- stream 是 terminal 操作, stream 使用一次后即回收。
