# Java 中的反射机制

## 基本概念

- 概念  
程序可以访问, 检测, 和修改它本身状态和行为的一种能力。并根据自身行为的状态和结果, 调整或修改应用所描述行为的状态和相关语义。

- 作用(介绍)  
方便编写灵活的代码, 这些代码可以在运行时装配, 无需在组件之间进行源代码链接。

**注意: 反射不当成本会很高。**

## 反射机制的作用

1. 反编译: .class->.java
2. 通过反射机制访问 Java 对象的属性, 方法和构造方法等。

- sun 提供的反射机制的类
    1. java.lang.reflect.Class  
    2. java.lang.reflect.Constructor  
    3. java.lang.reflect.Method  
    4. java.lang.reflect.Modifier  
    
    很多反射中的方法, 属性, 等操作可以从这四个类中查询。



