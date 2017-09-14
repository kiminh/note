# jvm monitoring

```
jvm实例
堆内存（各种内存）
```

## jps(Java Virtual Machine Process Status Tool)

查看所有的 jvm 进程，包括进程 ID，进程启动的路径等等.

语法格式：

```
jps [-q] [-mlvV] [<hostid\>]
```

参数说明：

参数 | 说明  
---|---
-q | 不输出类名、Jar 名和传入 main 方法的参数
-m | 输出传入 main 方法的参数
-l | 输出 main 类或 Jar 的全限名
-v | 输出传入 JVM 的参数

## jstack(Java Stack Trace)

1. 观察 jvm 中当前左右县城的运行情况和线程的当前状态。
2. 当系统崩溃时。如果 java 程序崩溃生成 core 文件，jstack 工具可以用来获得 core 文件的java stack 和 native stack 的信息，从而可以轻松地知道 java 程序是如何崩溃和在程序何处发生问题。
3. 当系统挂起时。jstack 工具还可以附属到正在运行的 java 程序中，看到当时运行的 java 程序的java stack 和 native stack的信息。

**需要注意的问题:**
1. 不同的 JAVA 虚机的线程 DUMP 的创建方法和文件格式是不一样的，不同的 JVM 版本， dump 信息也有差别。
2. 在实际运行中，往往一次 dump 的信息，还不足以确认问题。建议产生三次 dump 信息，如果每次 dump 都指向同一个问题，我们才确定问题的典型性(from internet)。 

- 命令格式：
```shell
jstack [ option ] pid
jstack [ option ] executable core
jstack [ option ] [server-id@]remote-hostname-or-IP
```

- 参数说明：

参数 | 说明  
---|---
-l | long listings，会打印出额外的锁信息，在发生死锁时可以用 ```jstack -l pid``` 来观察锁持有情况。
-m | mixed mode，不仅会输出Java堆栈信息，还会输出C/C++堆栈信息（比如Native方法）。

- 使用说明：

    1. 找出 Java 进程 PID；
    2. 找出该进程内最耗费 CPU 的线程。可以使用 ```ps -Lfp pid``` 或者 ```ps -mp pid -o THREAD, tid, time``` 或者 ```top -Hp pid```。(TIME 列就是各个 Java 线程耗费的 CPU 时间)。
    3. 通过 jstack 输出进程的堆栈信息。

- 语法：  

```
jstat [ generalOption | outputOptions vmid [interval[s|ms] [count]] ]
# vmid 是 Java 虚拟机 ID， 在 Linux/Unix 系统上一般就是进程 ID
# interval 是采样时间间隔
# count 是采样数目
```

- 参数说明

参数 | 说明  
---|---
-class | 统计类装载器的行为。
-compiler | 统计 HotSpot Just-in-Time 编译器的行为。
-gc | 统计堆各个分区的使用情况。
-gccapacity | 统计新生区，老年区，permanent 区的 heap 容量情况。 
-gccause | 统计最后一次 gc 和当前 gc 的原因。
-gcnew | 统计 gc 时，新生代的情况。 
-gcnewcapacity | 统计新生代大小和空间。
-gcold | 统计老年代和永久代的行为。
-gcoldcapacity | 统计老年代大小。
-gcpermcapacity | 统计永久代大小。 
-gcutil| 统计 gc 时，heap 情况。 
-printcompilation | HotSpot 编译方法统计。

- addition( jvm 相关 )：

```
堆内存 = 年轻代 + 年老代 + 永久代
年轻代 = Eden 区 + 两个 Survivor 区（ From 和 To ）
```

- 示例  

```
# gc 示例
jstat -gc 21711 <interval\> <times\>
S0C    S1C    S0U    S1U      EC       EU        OC         OU       PC     PU    YGC     YGCT    FGC    FGCT     GCT   
192.0  192.0   64.0   0.0    6144.0   1854.9   32000.0     4111.6   55296.0 25472.7    702    0.431   3      0.218    0.649

# 含义
S0C、S1C、S0U、S1U：Survivor 0/1区容量（Capacity）和使用量（Used）
EC、EU：Eden 区容量和使用量
OC、OU：年老代容量和使用量
PC、PU：永久代容量和使用量
YGC、YGT：年轻代 GC 次数和 GC 耗时
FGC、FGCT：Full GC 次数和 Full GC 耗时
GCT：GC总耗时
```

## jstat(Java Virtual Machine Statistics Monitoring Tool)

1. 利用 JVM 内建的指令对 Java 应用程序的资源和性能进行实时的命令行的监控，包括了对进程的 classloader ， compiler ，gc 情况
2. 监视 VM 内存内的各种堆和非堆的大小及其内存使用量，以及加载类的数量。

## jmap(Java Memory Map)
jmap 用来查看堆内存使用状况，一般结合 jhat 使用。

- 语法格式：

```
jmap [option] pid
jmap [option] executable core
jmap [option] [server-id@]remote-hostname-or-ip

# 如果运行在64位 JVM 上，可能需要指定 "J-d64 命令选项参数。
jmap -permstat pid
```

参数 | 意义  
---|---
-heap | 查看进程堆内存使用情况，包括使用的GC算法、堆配置参数和各代中堆内存使用情况。  
-histo[:live] | 查看堆内存中的对象数目、大小统计直方图，如果带上live则只统计活对象。
-dump | 用jmap把进程内存使用情况 dump 到文件中。

- 用jmap把进程内存使用情况 dump 到文件中：

```
# 语法
jmap -dump:format=b,file=dumpFileName pid

# case
jmap -dump:format=b,file=/tmp/dump.dat <pid\> 
```

dump 出来的文件可以用MAT、VisualVM等工具查看：

```shell
jhat -port <port\> <file_path\>
```



**注意如果Dump文件太大，可能需要加上-J-Xmx512m这种参数指定最大堆内存。**

## jinfo(Java Configuration Info)

观察进程运行环境参数，包括 Java System 属性和 JVM 命令行参数

//TODO




## hprof（Heap/CPU Profiling Tool）

展现CPU使用率，统计堆内存使用情况。

- 语法格式： 

```
java -agentlib:hprof[=options] ToBeProfiledClass
java -Xrunprof[:options] ToBeProfiledClass
javac -J-agentlib:hprof[=options] ToBeProfiledClass
```

- 命令选项  

选项和值 | 描述 | 缺省  
---|---|---  
heap=dump|sites|all | heap profiling | all  
cpu=samples|times|old | CPU usage | off  
monitor=y/n | monitor contention | n  
format=a/b | text(txt) or binary output | a  
file=```<file\>``` | write data to file | java.hprof[.txt]  
net=```<host\>:<port\>``` | send data over a socket | off  
depth=```<size\>``` | stack trace depth | 4  
interval=```<ms\>``` | sample interval in ms | 10  
cutoff=```<value\>``` | output cutoff point | 0.0001  
lineno=y/n | line number in traces? | y  
thread=y/n | thread in traces? | n  
doe=y/n | dump on exit? | y  
msa=y/n | Solaris micro state accounting | n  
force=y/n | force output to <file\> | y  
verbose=y/n | print messages about dumps | y  

- 示例  

```
# 每隔 20 毫秒采样 CPU 消耗信息，堆栈深度为 3，生成的 profile 文件名称是 java.hprof.txt ，在当前目录。
java -agentlib:hprof=cpu=samples,interval=20,depth=3 Hello

javac -J-agentlib:hprof=cpu=times Hello.java
javac -J-agentlib:hprof=heap=sites Hello.java
javac -J-agentlib:hprof=heap=dump Hello.java
```
**虽然在 JVM 启动参数中加入```-Xrunprof:heap=sites``` 参数可以生成 CPU/Heap Profile 文件，但对 JVM 性能影响非常大，不建议在线上服务器环境使用。**

## 参考资料

- 《Java虚拟机规范》  
- 《Java Performance》  
- [《Trouble Shooting Guide for JavaSE 6 with HotSpot VM》](http://www.oracle.com/technetwork/java/javase/tsg-vm-149989.pdf)  
- 《Effective Java》  
- [VisualVM](http://docs.oracle.com/javase/7/docs/technotes/guides/visualvm/)  
- [jConsole](http://docs.oracle.com/javase/1.5.0/docs/guide/management/jconsole.html)  
- [Monitoring and Managing JavaSE 6 Applications](http://www.oracle.com/technetwork/articles/javase/monitoring-141801.html)
