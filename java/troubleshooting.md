# troubleshooting

1. java.lang.NoClassDefFoundError 错误

     - 引起原因: 虚拟机在编译的时候能够找到合适的类, 但在运行时找不到。在运行时调用该类的方法或静态成员时, 会发现该类不可用, 此时虚拟机抛出 `NoClassDefFoundError`。

     - 与 `java.lang.ClassNotFoundException` 的区别: 这两个错误都与 `java classpath` 有关, 但完全不同
     > NoClassDefFoundError发生在JVM在动态运行时, 根据你提供的类名, 在classpath中找到对应的类进行加载, 但当它找不到这个类时, 就发生了java.lang.NoClassDefFoundError的错误。 ClassNotFoundException是在编译的时候在classpath中找不到对应的类而发生的错误。
     > `NoClassDefFoundError` 发生, 并且对应的类存在, 说明这个类对加载器来说是不可见的。

     - 发生原因:
       1. 对应的 `Class` 在 `java` 的 `classpath` 中不可用;
       2. 可能使用了 `jar` 命令运行程序, 但并没有在 `mainfirst` 文件的 `classpath` 属性中定义;
       3. 可能启动脚本覆盖了原来的 `classpath` 环境变量;
       4. 因为 `NoClassDefFoundError` 是 `java.lang.LinkageError` 的一个子类, 所以可能由于程序依赖的原生的类库不可用而导致;
       5. 检查日志文件中是否有 `java.lang.ExceptionInInitializerError` 这样的错误, `NoClassDefFoundError` 有可能是由于静态初始化失败导致的;
       6. 如果你工作在J2EE的环境, 有多个不同的类加载器, 也可能导致NoClassDefFoundError。

     - 解决方案: 参考 http://blog.csdn.net/jamesjxin/article/details/46606307
