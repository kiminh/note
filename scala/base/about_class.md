# Scala 中的类

- [类](#类)
- [class](#class)
  - [base](#base)
  - [abstract class](#abstract-class)
  - [anonymous class](#anonymous-class)
- [对象、Case和Trait](#对象case和trait)
  - [object](#object)
  - [case](#case)
  - [trait](#trait)

## 类

## class

### base

### abstract class

### anonymous class

## 对象、Case和Trait

### object

Object 是一个类的类型, 只能有不超过 1 个实例(即单例)。 对象不是用 *new* 关键字创建实例, 按名直接访问对象。 对象会在首次访问时会在当前运行的 JVM 中实例化, 在第一次访问之前不会实例化。 定义关键字是 *object*, 不是 *class*, 对象没有任何参数(自动实例化), 可以像常规类一样定义字段, 方法和内部类。

### case

### trait
