# java NIO(non-blocking IO)

- 和标准 IO 比较
  > 标准 IO 一般面向字节流或字符流, NIO 面向通道和缓冲区, 数据总是从通道( channel )中读取到缓冲区( buffer )中。
  > 非阻塞的 IO 操作, 读取 buffer 的时候可以同时做其他事情, 数据读取到 buffer 中后再进行处理。
  > Selector 检测多个通道的事件状态( 链接打开, 数据到达 ), 单线程可以检测到多个通道的数据。

BIO | NIO | AIO
---|---|---
简单的 IO 操作方式 | 同步非阻塞 IO | 异步非阻塞 IO
同步 | 同步 | 异步
阻塞 | 非阻塞 | 非阻塞

- 核心组件
  1. Channel
  2. Buffer
  3. Selector

- Selector
  > `Selector` 允许单线程操作多个通道, 主要作用于 有大量链接, 且带宽不高时。
  > 需要将 channel 注册到 selector 上。
  > `Selector` 的 select() 方法进入阻塞, 直到一个 channel 的状态符合为止。

## Channel

- Channel 用于数据的读写,
  1. FileChannel: 文件数据的读写。`buffer.flip()` 读取数据。
  2. DatagramChannel: UDP 的读写。
  3. SocketChannel: TCP 的读写
  4. ServerSocketChannel: 允许监听 TCP 链接请求, 每个链接创建一个 SocketChannel。

## Buffer

- 用于和 Channel 交互, 从 channel 中读取数据到 buffer 中, 通过 buffer 将数据写入 channel。
  1. 数据写入 buffer
  2. 调用 flip() 将 buffer 从写调整为读模式, 读模式下可以读取 buffer 中所有数据。
  3. 从 buffer 中读取数据
  4. buffer.clear() | buffer.compact() clear() 清空所有数据, compact() 清空已经读取的数据。
  5. Buffer 属性:
       1. mark: 标记, 备忘 position, 通过 mark() 设置 mark = position。
       2. position: 位置, 下一个要被读写的标记。由相应的 get(), put() 更新。
       3. capacity: 容量, Buffer 能够容纳的最大数量, 在 Buffer 实例生成后不可改变。
       4. limit: 上界, Buffer 不能被读或写的数据, 即缓冲区中现存元素的计数。
       > 关系: 0 <= mark <= position <= limit <= capacity
  6. 支持级联调用
  7. flip() 方法将 Buffer 翻转成一个准备读写的 Buffer。
  8. remaining(), hasRemaining(), 释放
  9. compact() 压缩, 只从 Buffer 中释放部分数据

- buffer 属性
  1. capacity(容量): 两种模式(读写)都表示容量, 最多能写入 capacity 的字节, 整型等。 写满后需要清空以便下次继续写入新的数据。
  2. position(位置): 取决于当前 buffer 的模式。 数据写入 buffer 时需要从一个确定的位置开始。 默认 position 为0。 更改为读模式时, position 重置为 0。
  3. limit(限制): 取决于当前 buffer 的模式。 写入 buffer 的上限 | 能读取到的最大数据量。

- example 分配一个 buffer 使用 allocating(int) 方法。
  1. ByteBuffer
  2. MappedByteBuffer
  3. DoubleBuffer
  4. FloatBuffer
  5. IntBuffer
  6. LongBuffer
  7. ShortBuffer
