# Concurrency

- 译自: [The Java Tutorials](https://docs.oracle.com/javase/tutorial/essential/concurrency/index.html)

- [Concurrency](#concurrency)
    - [进程与线程](#%E8%BF%9B%E7%A8%8B%E4%B8%8E%E7%BA%BF%E7%A8%8B)
    - [Thread 对象](#thread-%E5%AF%B9%E8%B1%A1)
        - [定义并且启动一个线程](#%E5%AE%9A%E4%B9%89%E5%B9%B6%E4%B8%94%E5%90%AF%E5%8A%A8%E4%B8%80%E4%B8%AA%E7%BA%BF%E7%A8%8B)
        - [Pausing Execution with Sleep](#pausing-execution-with-sleep)
        - [Interrupts](#interrupts)
            - [中断支持](#%E4%B8%AD%E6%96%AD%E6%94%AF%E6%8C%81)
            - [中断状态标识](#%E4%B8%AD%E6%96%AD%E7%8A%B6%E6%80%81%E6%A0%87%E8%AF%86)
        - [Joins](#joins)
        - [SampleThreads 示例](#samplethreads-%E7%A4%BA%E4%BE%8B)
    - [Sychronization(同步)](#sychronization%E5%90%8C%E6%AD%A5)
        - [Thread Interface](#thread-interface)
        - [Memory Consistency Errors](#memory-consistency-errors)
        - [Sychronized Methods](#sychronized-methods)
        - [Intrinsic Locks and Sychronization](#intrinsic-locks-and-sychronization)
        - [Atomic Access](#atomic-access)
    - [Liveness](#liveness)
        - [Deadlock](#deadlock)
        - [Starvation and Livelock](#starvation-and-livelock)
    - [Guarded Blocks](#guarded-blocks)
    - [Immutable Objects](#immutable-objects)
        - [A Sychronized Class Example](#a-sychronized-class-example)
        - [A Strategy for Defining Immutable Object](#a-strategy-for-defining-immutable-object)
    - [High Level Concurrent](#high-level-concurrent)
        - [Lock Object](#lock-object)
        - [Executor](#executor)
            - [Executor Interface](#executor-interface)
            - [Thread Pools](#thread-pools)
            - [Fock/Join](#fockjoin)
        - [Concurrent Collections](#concurrent-collections)
        - [Atomic Variables](#atomic-variables)
        - [Concurrent Random Number](#concurrent-random-number)
    - [For Futher Reading](#for-futher-reading)
    - [Questions and Exercises](#questions-and-exercises)

Java 平台从底层设计来支持并发编程, 在 Java 语言和 Java 类库中有基本的并发支持。 自 Java 5.0 以来, Java 平台包含了高级并发 api。 本文介绍 Java 的基本并发支持, 并总结了 `java.util.concurrent` 包下的一些高级 api。

## 进程与线程

在并发编程中, 有两种基本的处理: 进程和线程。 在 Java 编程中, 并发编程主要与线程有关, 然而进程也是很重要的。

计算机系统通常有很多活动的进程和线程。 即使在单核计算机中也很常见, 因此, 在任何给定时刻只有一个线程实际在执行。 一个内核的处理时间通过一个称为时间切片的操作系统特性在进程和线程之间共享。

计算机有多个处理器或着处理器有多核已经越来越普遍。 这极大的增强了系统并发执行进程和线程的能力。 但即使是在简单系统上, 没有多个处理器或多核的情况下, 并发也是可能的。

- 进程

进程有独立的执行环境。 进程通常有一个完整的、私有的基本运行时资源。 尤其是, 每个进程都有自己的内存空间。

进程通常是为应用程序的同义词, 然而, 用户通常看到的一个应用程序实际上是一堆进程合作的结果。 为支持进程间的通信, 大多数操作系统都支持 [IPC](https://en.wikipedia.org/wiki/Inter-process_communication) 资源, 比如管道和 socket。 IPC 不仅用于系统上的进程之间的通信, 还用于不同系统上的进程。

大多数 Java 虚拟机的实现都是作为一个单独的进程运行的。 Java 可以使用 `ProcessBuilder` 对象创建多个进程。

- 线程

线程有时被称为轻量的进程。 线程和进程都提供一个执行环境, 但是创建一个线程比创建一个进程所需要的资源更少。

线程存在于进程中, 每个进程都有至少一个线程。 线程间共享进程的资源, 包括内存和文件。 这样有助于有效的线程间的 communication, 但是可能存在问题。

多线程执行是 Java 平台的一个基本特性。 每个应用有至少一个或者更多的线程, 系统线程通常负责内存管理和信号处理。 但是对应用程序来说, 只需要一个线程来启动, 即主线程, 该线程能够创建额外的线程。

## Thread 对象

每个线程都与 `Thread` 类的一个实例相关联。 通过 `Thread` 对象创建线程有两种基本策略:

- 直接控制线程的创建和管理, 在每次应用程序需要启动异步任务时实例化线程即可。
- 应用程序的其余部分抽象线程管理, 将应用程序的任务传递给执行器。

### 定义并且启动一个线程

创建线程实例的应用程序必须提供在该线程中运行的代码。 有两种方式来实现:

- 提供一个 `Runnable` 接口, `Runnable` 接口定义了一个方法 `run()`, 表示要在线程中执行的代码。 `Runnable` 的对象传递给 `Thread` 的构造函数:

```java
public class HelloRunnable implements Runnable {
    @Override
    public void run() {
        System.out.println("Hello, world");
    }

    public static void main(String[] args) {
        new Thread(new HelloRunnable()).start();
    }
}
```

- `Thread` 的子类。 `Thread` 类b恩深实现了 `Runnable` 接口, 尽管它的 `run()` 什么都不做。 可以通过继承 `Thread` 类并重写 `run()` 方法来实现:

```java
public class HelloThread extends Thread {
    @Override
    public void run() {
        System.out.println("Hello, world");
    }

    public static void main(String[] args) {
        new HelloThread().start();
    }
}
```

注意这两种方式都通过 `start()` 的方式来启动一个线程。

使用 `Runnable` 对象的第一个习惯用法更一般, 因为 `Runnable` 对象可以子类化除`Thread` 之外的类。 第二个习惯用法在简单的应用程序中更容易使用, 但是由于任务类必须是线程的子代而受到限制。 实现 `Runnable` 将可运行任务与执行任务的线程对象分开。 这种方法不仅更加灵活, 而且适用于高级线程管理 api。

`Thread` 类定义了许多对线程管理有用的方法。 包括静态方法, 它们提供 关于或影响调用该方法的线程 的状态信息。 其他方法是从管理线程和线程对象的其他线程调用的。

### Pausing Execution with Sleep

`Thread.sleep` 会导致当前线程在指定期间暂停执行。 这是一种使程序或其他可能在计算机上运行的应用程序的其他线程可以使用处理器时间的有效方法。

提供了两个重载的 `sleep` 版本: 一个指定 `sleep` 时间的毫秒数, 另一个指定 `sleep` 时间的纳秒数。 这些睡眠不能保证精确, 因为收到底层 OS 提供的设备限制。 同时, `sleep` 时间可以通过中断来终止。 不能通过 `sleep` 将指定时间段挂起线程。

```java
public class SleepMessages {
    public static void main(String[] args) throws InterruptException{
        String importantInfo[] = {
            "Mares eat oats",
            "Does eat oats",
            "Little lambs eat ivy",
            "A kid will eat ivy too"
        };

        for (int i = 0; - < importantInfo.length; i++) {
            // sleep 4s
            Thread.sleep(4000);
            // 打印信息
            System.out.println(importantInfo[i]);
        }
    }
}
```

注意, `main` 声明抛出一个 `InterruptException`。 当 `sleep` 处于活动状态时, 另一个线程中断该线程时, `sleep` 抛出一个异常。

### Interrupts

中断是线程的指示, 表明应当停止正在做的事情。 线程通过调用 `Theead` 对象的中断来发送中断, 来被线程中断。为使中断机制正常工作, 被中断的线程必须支持它自己的中断。

#### 中断支持

线程的中断取决于它正在做什么。 如果线程调用经常抛出 `InterruptException` 的方法, 那么只会在捕获该异常后从 `run()` 方法返回。 如: 上面的 Sleep 例子中, 循环部分在可运行对象的 `run` 中运行, 可修改为如下支持:

```java
for (int i = 0; i < importantInfo.length; i++) {
    try {
        Thread.sleep(4000);
    } catch (
        return;
    )
    System.out.pringln(importantInfo[i]);
}
```

许多抛出 InterruptedException 的方法(如: sleep) 被设计为取消当前操作并在接收中断时立即返回。 如果一个线程长时间不调用抛出InterruptedException的方, 则必须定期调用 `thread.interrupt`。

```java
for (int i = 0; i < importantInfo.length; i++) {
    heavyCrunch(inputs[i]);
    if (Thread.interrupted()) {
        return;
    }
}
```

上述代码只是简单对中断进行测试, 如果收到中断, 则应退出线程, 更复杂的应用中还需要抛出 `InterruptException`。

```java
if (Thread.interrupt()) {
    throw new InterruptException();
}
```

这允许在 `catch` 子句中集中中断处理代码。

#### 中断状态标识

中断机制是使用内部标志 (称为中断状态) 实现的。 调用 `Thread.interrupt` 设置此标志。 当一个线程通过调用静态方法线程来检查中断时, 中断状态被清除。 非静态 `isinterrupt` 方法(由一个线程用于查询另一个线程的中断状态) 不会更改中断状态标志。

通过抛出 `InterruptedException` 而退出的任何方法在抛出中断时清除中断状态。 然而, 中断状态很可能会立即被另一个调用中断的线程重新设置。

### Joins

`join` 方法允许一个线程等待另一个线程的完成。 如果是线程 `t` 时当前正在执行的 `Thread` 对象, 可以通过 `t.join()` 方式使当前线程停止执行, 直到线程 t 终止。 重载连接允许指定一个等待周期。 然而, 和 `sleep` 一样, `join` 依赖操作系统来计时, 所以不应该假设 `join` 会一直等待设置的时间。 和 `sleep` 一样, `join` 通过 `InterruptException` 退出来响应中断。

### SampleThreads 示例

```java
public class SimpleThreads {

    // Display a message, preceded by
    // the name of the current thread
    static void threadMessage(String message) {
        String threadName =
            Thread.currentThread().getName();
        System.out.format("%s: %s%n",
                          threadName,
                          message);
    }

    private static class MessageLoop
        implements Runnable {
        public void run() {
            String importantInfo[] = {
                "Mares eat oats",
                "Does eat oats",
                "Little lambs eat ivy",
                "A kid will eat ivy too"
            };
            try {
                for (int i = 0;
                     i < importantInfo.length;
                     i++) {
                    // Pause for 4 seconds
                    Thread.sleep(4000);
                    // Print a message
                    threadMessage(importantInfo[i]);
                }
            } catch (InterruptedException e) {
                threadMessage("I wasn't done!");
            }
        }
    }

    public static void main(String args[])
        throws InterruptedException {

        // Delay, in milliseconds before
        // we interrupt MessageLoop
        // thread (default one hour).
        long patience = 1000 * 60 * 60;

        // If command line argument
        // present, gives patience
        // in seconds.
        if (args.length > 0) {
            try {
                patience = Long.parseLong(args[0]) * 1000;
            } catch (NumberFormatException e) {
                System.err.println("Argument must be an integer.");
                System.exit(1);
            }
        }

        threadMessage("Starting MessageLoop thread");
        long startTime = System.currentTimeMillis();
        Thread t = new Thread(new MessageLoop());
        t.start();

        threadMessage("Waiting for MessageLoop thread to finish");
        // loop until MessageLoop
        // thread exits
        while (t.isAlive()) {
            threadMessage("Still waiting...");
            // Wait maximum of 1 second
            // for MessageLoop thread
            // to finish.
            t.join(1000);
            if (((System.currentTimeMillis() - startTime) > patience)
                  && t.isAlive()) {
                threadMessage("Tired of waiting!");
                t.interrupt();
                // Shouldn't be long now
                // -- wait indefinitely
                t.join();
            }
        }
        threadMessage("Finally!");
    }
}
```

## Sychronization(同步)

线程主要通过共享对字段和引用字段的访问进行通信, 这种形式的通信非常有效, 但是会导致两种错误: 线程干扰和内存一致性错误。 `sychronization` 同步时防止这些错误的工具。

但是, 同步可能引入线程争用, 当两个线程或多个线程同时访问相同资源时, 会导致 Java 运行执行一个或多个线程变慢, 甚至暂停它们的运行。 starvation(~~饿死!!!??~~) 和 livelock(活锁) 是线程争用的形式。

### Thread Interface

```java
class Counter {
    private int c = 0;
    
    public void increment() {
        c ++;
    }

    public void decrement() {
        c--;
    }

    public int value() {
        return c;
    }
}
```

### Memory Consistency Errors

### Sychronized Methods

### Intrinsic Locks and Sychronization

### Atomic Access

## Liveness

### Deadlock

### Starvation and Livelock

## Guarded Blocks

## Immutable Objects

### A Sychronized Class Example

### A Strategy for Defining Immutable Object

## High Level Concurrent

### Lock Object

### Executor

#### Executor Interface

#### Thread Pools

#### Fock/Join

### Concurrent Collections

### Atomic Variables

### Concurrent Random Number

## For Futher Reading

## Questions and Exercises
