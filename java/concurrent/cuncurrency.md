# Concurrency

- 译自: [The Java Tutorials](https://docs.oracle.com/javase/tutorial/essential/concurrency/index.html)

- [Concurrency](#concurrency)
    - [进程与线程](#%E8%BF%9B%E7%A8%8B%E4%B8%8E%E7%BA%BF%E7%A8%8B)
    - [Thread 对象](#thread-%E5%AF%B9%E8%B1%A1)
        - [定义并且启动一个线程](#%E5%AE%9A%E4%B9%89%E5%B9%B6%E4%B8%94%E5%90%AF%E5%8A%A8%E4%B8%80%E4%B8%AA%E7%BA%BF%E7%A8%8B)
        - [Pausing Execution with Sleep](#pausing-execution-with-sleep)
        - [Interrupts](#interrupts)
        - [Joins](#joins)
        - [The SampleThreads Example](#the-samplethreads-example)
    - [Sychronization](#sychronization)
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

### Pausing Execution with Sleep

### Interrupts

### Joins

### The SampleThreads Example

## Sychronization

### Thread Interface

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
