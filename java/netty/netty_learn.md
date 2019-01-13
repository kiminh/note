# netty 学习记录

- [netty 传输方式](#netty-传输方式)
- [核心类](#核心类)
- [Reactor 模式](#reactor-模式)

- 资料来源:
    > [netty User guide for 4.x](http://netty.io/wiki/user-guide-for-4.x.html)  
    > [netty-4-user-guide 中文翻译](https://github.com/waylau/netty-4-user-guide/blob/master/Getting%20Started/Getting%20Started.md)  
    > [Netty 4.1.17.Final API文档](http://docs.52im.net/extend/docs/api/netty4_1/overview-summary.html)  
    > Netty in Action

## netty 传输方式

1. NIO: `io.netty.channel.socket.nio`, 基于 `java.nio.channels`, 使用选择器作为基础的方法
    > 高连接数时使用
2. OIO: `io.netty.channel.socket.oio`, 基于 `java.net`, 使用阻塞流
    > 低连接数时使用
3. Local: `io.netty.channel.local`, 在虚拟机之间本地通信
    > 同一个 `JVM` 内通信时使用
4. Embedded: `io.netty.channel.embeded`, 嵌入传输, 允许在没有网络的传输中使用 `ChannelHandler`, 可以用来测试 `ChannelHandler` 的实现
    > 测试 `ChannelHandler`

## 核心类

- Bootstrap or ServerBootstrap
- EventLoop
- EventLoopGroup
- ChannelPipeline
- Channel
- Future or ChannelFuture
- ChannelInitializer
- ChannelHandler

## Reactor 模式
