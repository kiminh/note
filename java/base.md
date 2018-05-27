# base of java

- java7 url

```
http://www.oracle.com/technetwork/java/javase/downloads/java-archive-downloads-javase7-521261.html
```

1. command

2. build

3. java package

  1. ear 包, 企业级应用, 通常是 EJB 打成 ear 包。
  2. war 包, 做好一个 web 应用后, 通常是网站, 打成包部署到容器中。
  3. jar 包, 通常是开发时要引用的依赖, 打成包便于存放管理。也有一些可执行的 jar 包, 通过 `java -jar` 执行

4. run java project

5. filter vs interceptor

  - 区别:
    1. filter 基于函数回调, Interceptor 基于反射;
    2. filter 依赖 Servlet 容器, Interceptor 不依赖 Servlet 容器;
    3. filter 可以对所有请求起作用, Interceptor 只能对 Action 起作用;
    4. Interceptor 可以访问 Action 上下文、值栈里的对象, 而 filter 不能;
    5. 在 Action 的周期内, Interceptor 可以多次被调用, 而 filter 只能在容器初始化的时候调用一次。
  - 执行顺序
    过滤前 --> 拦截前 --> action 执行 --> 拦截后 --> 过滤后

- finally & return
  - [Java finally语句到底是在return之前还是之后执行？](https://www.cnblogs.com/lanxuezaipiao/p/3440471.html)

- final & finally & finalize()
  - [finalize作用]([https://www.cnblogs.com/Smina/p/7189427.html)
  1. final 作用:
    - final 用于修饰类、成员变量和方法。 被 final 修饰的类不可被继承, 方法不可被覆盖。

  2. finally 作用:
    - 处理异常, 不管是否出现异常, 该段语句总是执行。

  3. finalize() 作用:
    1. finalize() 是 Object 的 protected 方法, 子类可以覆盖该方法以实现资源清理工作, GC 回收回收对象之前调用。
    2. finalize() 与 C++ 中的析构函数不是对应的。 C++ 中的析构函数调用的时机是确定的(对象离开作用域或delete), Java 中的 finalize() 具有不确定性。
    3. 不建议用 finalize() 方法完成非内存资源的清理工作, 但建议用于
      > 1. 清理本地对象(通过JNI创建的对象)。
      > 2. 作为确保某些非内存资源(如: Socket、文件等)释放的一个补充, 在 finalize() 显示调用其他释放资源的方法。
