# 线程 & 线程池

- [线程 & 线程池](#%E7%BA%BF%E7%A8%8B--%E7%BA%BF%E7%A8%8B%E6%B1%A0)
    - [1. 线程](#1-%E7%BA%BF%E7%A8%8B)
        - [线程的几种状态](#%E7%BA%BF%E7%A8%8B%E7%9A%84%E5%87%A0%E7%A7%8D%E7%8A%B6%E6%80%81)
    - [2. 线程池](#2-%E7%BA%BF%E7%A8%8B%E6%B1%A0)
        - [创建线程池](#%1B%E5%88%9B%E5%BB%BA%E7%BA%BF%E7%A8%8B%E6%B1%A0)
            - [通过 Executors 创建](#%E9%80%9A%E8%BF%87-executors-%E5%88%9B%E5%BB%BA)

## 1. 线程

### 线程的几种状态

线程的状态通过一个内部的枚举类来维护 `java.lang.Thread.State`, 共有 6 中状态: `NEW`, `RUNNABLE`, `BLOCKED`, `WAITING`, `TIME_WAITING`, `TERMINALED`。 含义如下。

1. NEW
    > 线程刚创建好但是还未启动(即执行 `thread.start()`)时的状态为 NEW。
2. RUNNABLE
    > 正在执行的线程。 注意有可能出现的情况是: JVM中处于 RUNNABLE 状态的线程有可能正在等待一些其他资源。
3. BLOCKED
    > 正在等待 `monitor lock` 的状态。 通常以下行为会使线程进入 BLOCK 状态:  
    > 处于 `BLOCK` 状态的线程正在等待监视器在调用 `Object.wait()` 进入一个 `synchronized` 或 `reenter synchronized` 块/方法  
4. WAITING
    > 正在等待的线程的状态。
    > 以下方法可能导致进入 `WAITTING` 状态:  
        > 1. 无超时的 `Object.wait()` 方法
        > 2. 无超时的 `Thread.join()` 方法
        > 3. `LockSupport.park()` 方法

    > 处于等待状态的线程正在等待另一个线程执行特定操作。  例如: 
        > 1. 一个调用 `Object.wait()` 方法的线程会等待另一个对象调用 `Object.notify()` 或 `Object.nodifyAll()`
        > 2. 一个调用 `Thread.join()` 方法的线程会等待一个指定线程的终止
5. TIME_WAITING
    > 有等待时间(即超时时间) 的正在等待的线程。通常以下方法的调用会使线程进入 `TIME_WAITING` 的状态。
        > 1. `Thread.sleep(long)` 方法
        > 2. `Object.wait(long)` 方法
        > 3. `Thread.join(long)` 方法
        > 4. `LockSupport.partNanos(Object, long)` 方法
        > 5. `LockSupport.parkUntil(Object, long)` 方法
6. TERMINAL
    > 终止线程的线程状态。线程已经完成执行。

## 2. 线程池

### 创建线程池

`ExecutorService` 线程池的创建有以下几种方式:

1. 通过 `Executors` 工具创建。
2. 通过 `AbstractExecutorService` 的子类的构造方法进行创建。

#### 通过 Executors 创建

`Executors` 是一个实例化 `Executor`, `ExecutorService`, `ScheduledExecutorService`, `ThreadFactory`, `Callable` 这些对象的实用方法的工厂。 `Executors` 提供了一系列创建 `ExecutorService`, `ScheduledExecutorService`, `ThreadFactory`, `Callable` 的方法, 本质上还是调用这些类的构造方法来创建相应的实例。 但由于
