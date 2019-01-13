### JSR 阅读记录

**编译器的读写操作**

1. Sequential Consistency(顺序一致性) 
2. happen-before

- 知识点:
    1. 顺序一致的内存模型
    2. Happen-before 的内存模型

- 名次定义
1. 共享变量/堆内存(Shared variable/Heap memory)
    > 线程间共享的内存, 包括静态字段, 数组元素, 实例字段。局部变量不会存在于共享内存。
2. 线程间动作(Intra-thread Actions)
    > 由某一线程执行, 能被另一个线程探测或影响到的动作。包括变量的读写以及 `synchronzation action`, 也包括一些外部动作(external action), 线程分散动作(thread divergence action) 等。
3. 程序顺序(Program Order) 
4. 线程内语义(Intra-thread semantics)
5. 同步动作(Synchronization Action)
    > 包括: 锁, 解锁, 读写 `volatile` 变量, 用于启动线程的动作以及用于探测线程是否结束的动作。
7. 同步顺序(Synchronization Order)
8. Happen-before and Synchronization-with 

- 内存模型的正式规范
1. synchronizes-with
    > 起点: release; 终点: acquire
2. happen-before
3. suffcient sychronization edges(充分的同步边缘)
    > 如果某一组同 步边缘是这样一个最小集，利用该最小集中的边缘和程序顺序边缘上的传 递闭包就可以确定执行过程中的所有 happens-before 边缘，则这一组同步 边缘是充分的。这样的同步边缘组是唯一的。
4. restrictions of partial orders and functions(偏序和函数的限制)

- (Well-format)良构的执行过程
1. 每个对变量 x 的读都能看到一个对 x 的写。所有对 volatile 变量的读写都是 volatile 动作。
2. 同步顺序与程序顺序以及互斥是一致的。
3. 线程的运行遵守线程内(intra-thread)一致性。
4. 线程的运行遵守同步顺序一致性。
5. 线程的运行遵守 happens-before 一致性。

- 执行过程的因果(Causally)要求

#### final 字段

- final字段
1. 语义
    > 只初始化一次且不再改变。 **这种语义允许编译器在读取这类字段时进行激进优化, 同时在不需要同步时保证线程安全**
2. 目标与要求
    - 目标
        1. 字段值不会变化, 编译器不应该因为获得了一个锁，读取了一个 volatile 变量或调用了一个未知方法，而重新加载一个 final 字段。
        2. 一个对象, 仅包含 final 字段, 切构建期间没有对其他线程可见, 应该视作不可变的, 即使在这类对象传递时存线程间传递时在数据竞争。
        3. 将一个变量设置为 final, 读取时应当使用最小的 编译器/架构 代价。
        4. 该语义必须支持诸如反序列化的场景, 通常, 一个对象的 final 字段将在该对象构建完成后改变。
3. 用户 final 字段的 JVM 规则

TO READ menu: 
1. **final 字段构建后再改变**
2. **wait, sleep, yield** 
3. **finalized**
