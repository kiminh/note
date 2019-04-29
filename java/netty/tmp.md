# 临时笔记

## Server 端的 boss 和 worker

`io.netty.channel.nio.NioEventLoopGroup`

- bossGroup

用于接受 client 请求。

- workerGroup

用于处理 client 具体的读写操作。 未指定线程数时, 启动的线程数为 CPU 核数 * 2。

```java
protected MultithreadEventLoopGroup(int nThreads, Executor executor, Object... args) {
    super(nThreads == 0 ? DEFAULT_EVENT_LOOP_THREADS : nThreads, executor, args);
}
```

```java
private static final int DEFAULT_EVENT_LOOP_THREADS = Math.max(1, SystemPropertyUtil.getInt("io.netty.eventLoopThreads", Runtime.getRuntime().availableProcessors() * 2));
```
