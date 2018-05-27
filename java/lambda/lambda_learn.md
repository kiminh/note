# lambda base learning

## 介绍

## 基本语法

### 常用函数式接口

函数式接口 | 参数类型 | 返回类型 | 抽象方法名 | 描述 | 其他方法
---|---|---|---|---|---
Runnable | 无 | void | run | 作为无参数或返回的动作运行 |
Supplier<T> | 无 | T | get | 提供一个 T 类型的值 |
Consumer<T> | T | void | accept | 处理一个 T 类型的值 | andThen
BiConsumer<T, U> | T, U | void | accept | 处理一个 T 和 U 类型的值 | andThen
Function<T, R> | T | R | apply | 类型参数的函数 | compose, andThenm identity
BiFunction<T, U, R> | T, U | R | apply | 类型参数的函数 | andThen
UnaryOperator<T> | T | T | apply | 类型 T 上的一元操作符 | compose, andThen, identity
BinaryOperator<T> | T, T | T | apply | 类型 T 上的二元操作符 | andThen, maxBy, minBy
Predicate<T> | T | boolean | test | 布尔值函数 | and, or, negate, isEqual
BiPredicate<T, U> | T, U | boolean | test | 有两个参数的布尔值函数 | and, or, negate

### 基本类型的函数式接口

函数式接口 | 参数类型 | 返回类型 | 示例 | 示例用例
---|---|---|---|---
Consumer<T> | T | void | s -> System.out.println(s) | 使用对象过滤值工厂方法进行转换或从对象中选择
Predicate<T> | T | boolean | s -> s.isEmpty() | 同上
Supplier<T> | 无 | T | () -> new String() | 同上
Function<T> | T | U | s -> new Integer(s) | 同上

## 内部类

### 基本概念

### 使用内部类访问对象状态

### 内部类的特殊语法规则

#### 6中上下文可以提供恰当的目标类型

- 方法或构造器参数, 该情况下, 目标类型就是恰当的参数类型。
- 变量声明与赋值, 该情况下, 目标类型是被赋值的类型。
```Java
Comparator<String> cc = (String s1, String s2) -> s1.compareToIgnoreCase(s2);
IntBinaryOperator[] calculatorOps = new IntBinaryOperator[](
  (s, y) -> x + y, (x, y) -> x - y, (x, y) -> x * y ->, (x, u)
);
```

- 返回语句, 该情况下, 目标类型是方法的返回类型
- lambda 表达式体, 在这种情况下, 目标类型是方法的返回类型
- 三元条件表达式
- 类型转换表达式

### 内部类是否可用、必要和安全

### 匿名内部类

### 静态内部类


## notice

禁止使用惯用法及初始化与变化。
