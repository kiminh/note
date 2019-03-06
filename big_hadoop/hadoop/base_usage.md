# 基本使用

## 通过 Hadoop 执行任务

- 运行 jar 文件

```bash
hadoop jar xxx.jar <options>
```

- 引入三方 jar:

```bash
# 1 直接使用 hadoop 的 libjars 参数加载第三方 jar
hadoop fs -libjars /path/to/your/lib_jar -jar /path/to/your/run_jar

# 2 在当前环境变量中加入 HADOOP_CLASSPAPTH, 可以将其写入 hadoop-env.sh 文件中让其启动时自动扫描到所需的依赖
# 环境变量中加入方式
HADOOP_CLASSPAPTH=/path/to/your/lib_jar
# Hadoop-env.sh 文件中写入
HADOOP_CLASSPAPTH={xxx}:/path/to/your/lib_jar_path
```
