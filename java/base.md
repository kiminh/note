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
