### aim

- basic
- 集合
- 设计模式
- 多线程
- IO
- JDK
- 框架
- 数据库
- 数据结构算法分析
- 虚拟机
- web
- 反射机制
- 代理类
- 字节码技术

### note

1. jar 解压缩, 打包的命令:

```bash
# testdir 目录文件（自动创建)
unzip abc.jar -d testdir

# 打包命令
jar -cvfM0 ambari-metrics-timelineservice-2.4.1.0.99.jar ./
# 注: **打包时 要在 工程根目录下**
# jar 参数说明
# -c   创建war包
# -v   显示过程信息
# -f    
# -M
# -0   这个是阿拉伯数字，只打包不压缩的意思
```

2. slf4j

difference between jcl-over-slf4j & slf4j-log4j12
