# HDFS 操作

- [Hadoop Shell命令](http://hadoop.apache.org/docs/r1.0.4/cn/hdfs_shell.html)

## shell 中操作 hdfs

- 查看文件或目录

```bash
hdfs dfs -ls <path_in_hdfs>
```

- 将文件从 hdfs copy 到本地

```bash
hdfs dfs -get <path_in_hdfs> <local_path>
```

- 判断一个文件或目录是否存在

```bash
hdfs dfs -test -<parameter> <path>
echo $?
# 如果目标文件或目录存在, 返回 0; 如果不存在, 返回 1。通过 "echo $?" 查看返回值
```

- 获取帮助

```bash
hdfs dfs -help
```
