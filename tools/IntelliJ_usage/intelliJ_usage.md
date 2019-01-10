- [IntelliJ-IDEA-Tutorial](https://github.com/judasn/IntelliJ-IDEA-Tutorial)

- 问题:

1. IntelliJ 卡死, 无法进行任何操作。
    > 说明: IntelliJ 2018.2, JDK 1.8.0_171, OS X 操作系统, 16GB 内存。
    > 现象: CPU 飙高, 有时候能到390+。 该原因并不是导致卡死的原因。 最后通过命令行启动 IntelliJ 时发现有几个插件无法加载的警告: `Bash Support`, `docker integration`, `python`, `Scala`。 不知道是不是这个原因, 决定尝试重装这几个插件。 这几个插件重装后, IntelliJ 能够正常使用。
