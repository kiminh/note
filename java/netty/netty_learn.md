# netty 学习记录

- 资料来源: 
    > [netty User guide for 4.x](http://netty.io/wiki/user-guide-for-4.x.html)  
    > [netty-4-user-guide 中文翻译](https://github.com/waylau/netty-4-user-guide/blob/master/Getting%20Started/Getting%20Started.md)  
    > [Netty 4.1.17.Final API文档](http://docs.52im.net/extend/docs/api/netty4_1/overview-summary.html)  
    > Netty in Action

- 文章结构
    > [netty 传输方式](#netty-传输方式)  
    > [核心类](#核心类)  
    > [缓冲区](#缓冲区(Buffer))  
    > [ChannelHandler](#channelhandler)  
    > [Codec](#Codec)  
    > [Bootstrap 引导服务](#bootstrap-引导服务)  
    > [reactor 模型](#reactor\ 模型)  

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

## 缓冲区(Buffer)

> [Buffer API](#Buffer API)

### Buffer API

> [ByteBuf](#ByteBuf)
> [ByteHolder](#ByteHolder)
> [对比 Java nio](#Bytebuf\ VS\ Java\ Nio)

- 基本内容
1. capacity: 容量
2. readerIndex
3. writerIndex
    > `readerIndex()` 超过 `writerIndex()` 会造成越界。  
    > `wirterIndex()` 到 `readerIndex()` 相当于`java.nio.ByteBuffer` 的 `position` 到 `limit` 之间的空间, 是可以读出的空间。  
    > 随着读写操作的增加, `readerIndex()` 和 `writerIndex()` 的值增加。  
    > `writerIndex()` 到 `capacity()` 的空间是可以写入的空间。  
    > `writerIndex()` 和 `readerIndex()` 的指针分离, 不需要对同一个位置的指针进行调整。
4. discard(丢弃):
    > 0 到 `readerIndex()` 之间的空间视为 discard, `discardReadBytes()` 释放这部分空间。  
    > 0 到 `writerIndex()` 之间的空间视为 discard, `discardWriteBytes()` 释放这部分空间。
5. 自动扩容
    > ByteBuf 拥有动态扩容的能力, 在对缓冲区进行写操作的时候, 需要对剩余的可用空间进行校验, 如果空间不足, 同时要写入的字节数小于可写的最大字节数, 会对缓冲区进行动态的扩展, 将以前的数据复制到一个新建的缓冲区中。
6. 

- 优势
    > 1. 可以自定义缓冲类型
    > 2. 通过一个内置复合缓冲类型实现零拷贝
    > 3. 扩展性好, 如 `StringBuffer`
    > 4. 不需要使用 `flip()` 来切换读写模式
    > 5. 读取和写入索引分开 
    > 6. 方法链
    > 7. 引用计数
    > 8. Pooling(池)
    
#### ByteBuf

- 分类:
    1. Heap Buffer(堆缓冲区)
    2. Direct Buffer(直接缓冲区)
    3. Composite Buffer(复合缓冲区)

1. Heap Buffer
    > - 最常用的缓冲类型, 将数据存储在 JVM 的堆空间, 数据以数组的形式存储。  
    > - 可直接获取数组
    > - 一定符合 `0 <= readerIndex <= writerIndex <= capacity`

2. Direct BUffer
    > - 在堆外直接分配内存, 不会占用堆空间, 在使用 `Socket` 传递数据时性能好。
    > - 分配内存空间和释放内存缓冲区较复杂, 但 `Netty` 提供池。

3. Composite Buffer

#### ByteHolder

`ByteHolder` 是一个辅助类, 作用是更方便的访问 `ByteBuf` 中的数据。

#### ByteBuf VS Java Nio

## ChannelHandler

> [ChannelPipeline](#ChannelPipeline)  
> [ChannelHandler](#ChannelHandler)  
> [ChannelHandlerContext](#ChannelHandlerContext)  
> [Inbound vs Outbound(入站和出站)](#Inbound\ vs\ Outbound)  
> [IdleStateHandler](#idlestatehandler)  

### ChannelPipeline

`ChannelHandler` 实例的列表, 用于处理或截获通道的接收和发送数据。

### ChannelHandler describe

### ChannelHandlerContext

每个 `ChannelHandler` 实例被添加到 `ChannelPipeline` 后会创建一个 `ChannelHandlerContext` 与之绑定.

### Inbound vs Outbound

### IdleStateHandler

- 参考: [IdleStateHandler心跳机制](https://blog.csdn.net/u013967175/article/details/78591810)

## Codec

> [Decode](#Decode)  
> [Encode](#Encode)  

Decode 负责将消息从字节或其他序列形式转换成指定消息对象; Encode 相反。

### Decode

### Encode

## Bootstrap 引导服务

> `Netty` 提供的简单统一的方法来引导 Server 和 Client。 引导是配置 `Netty` Server 和 Client 的过程。

分类: 
    1. 服务器的引导: `ServerBootstrap`
    2. 客户端的引导: `Bootstrap`
    - 特例: `DatagramChannel` 实例, 用于 `UDP` 协议, 无连接, 一个通道可以处理所有数据, 不需要依赖于连接。

## reactor 模型



