---
title: Executor框架简介
categories: 多线程
tags: java多线程
---
>Java的线程既是工作单元，也是执行机制。从JDK 5开始，把工作单元与执行机制分离开来。工作单元包括Runnable和Callable，而执行机制由Executor框架提供。

![Executor框架的成员及其关系]![mark](http://blog.sjjtcloud.com/blog/20191126/7lUJFkBwikeL.png?imageslim)


### Executor框架包括3大部分：

（1）任务。也就是工作单元，包括被执行任务需要实现的接口：Runnable接口或者Callable接口；

（2）任务的执行。也就是把任务分派给多个线程的执行机制，包括Executor接口及继承自Executor接口的ExecutorService接口, Executor框架有两个关键类实现了ExecutorService接口（ThreadPoolExecutor和ScheduledThreadPoolExecutor）

（3）异步计算的结果。包括Future接口及实现了Future接口的FutureTask类。


### Executor各个类和接口简介
 Executor 它是框架基础, 将任务的提交 和任务的执行分离开来

#### （1）ThreadPoolExecutor 
它是线程池的核心实现类,用来执行被提交的任务
创建方式：ThreadPoolExecutor通常使用Executors来创建,Executors可以创建3种类型的ThreadPoolExecutor
应用场景：适用于需要保证顺序地执行各个任务；并且在任意时间点，不会有多个线程是活动的应用场景

1. SingleThreadExecutor 创建使用单个线程的SingleThreadExecutor的API
```java
public static ExecutorService newSingleThreadExecutor()
public static ExecutorService newSingleThreadExecutor(ThreadFactory threadFactory)
```


2. FixedThreadPool  用于创建使用固定线程数的FixedThreadPool的API

  ```java
public static ExecutorService newFixedThreadPool(int nThreads)
public static ExecutorService newFixedThreadPool(int nThreads,ThreadFactory threadFactory)

 ```
应用场景：适用于为了满足资源管理的需求，而需要限制当前线程数量的应用场景，它适用于负载比较重的服务器


3. CachedThreadPool  下面是Executors提供的，创建一个会根据需要创建新线程的CachedThreadPool的API。
```java
public static ExecutorService newCachedThreadPool()
public static ExecutorService newCachedThreadPool(ThreadFactory threadFactory)
```
应用场景: CachedThreadPool是大小无界的线程池，适用于执行很多的短期异步任务的小程序，或者
是负载较轻的服务器


#### (2)ScheduledThreadPoolExecutor
它是一个实现类，可以在给定的延迟后运行命令，或者定期执行命令。ScheduledThreadPoolExecutor比Timer更灵活,功能更强大

创建方式
ScheduledThreadPoolExecutor通常使用工厂类Executors来创建。Executors可以创建2种类型的ScheduledThreadPoolExecutor，如下
```java
·ScheduledThreadPoolExecutor。包含若干个线程的ScheduledThreadPoolExecutor。
·SingleThreadScheduledExecutor。只包含一个线程的ScheduledThreadPoolExecutor。
```
* 方式一: 创建固定个数线程ScheduledThreadPoolExecutor的API
应用场景: 适用于需要多个后台线程执行周期任务，同时为了满足资源管理的需求而需要限制后台线程的数量的应用场景
```JAVA
public static ScheduledExecutorService newScheduledThreadPool(int corePoolSize)
public static ScheduledExecutorService newScheduledThreadPool(int corePoolSize,
                                                              ThreadFactory threadFactory)
```
* 方式二: 创建只包含一个线程的ScheduledThreadPoolExecutor
应用场景:适用于需要单个后台线程执行周期任务，同时需要保证顺序地执行各个任务的应用场景
```java
public static ScheduledExecutorService newSingleThreadScheduledExecutor()
public static ScheduledExecutorService newSingleThreadScheduledExecutor(ThreadFactory threadFactory)
```

(3)Future接口
Futrue接口和实现Futrue接口的FutrueTask类用来表示**异步计算的结果**当我们把Runnable接口或者Callable接口的实现类提交(submit)给ThreadPoolExecutor或ScheduledThreadPoolExecutor时,ThreadPoolExecutor或ScheduledThreadPoolExecutor会向我们返回一个FutrueTask对象,下面是对应API
```java
<T> Future<T> submit(Callable<T> task)
<T> Future<T> submit(Runnable task, T result)
Future<> submit(Runnable task)
```

(4)Runnable接口和Callable接口
Runnable接口和Callable接口的实现类，都可以被ThreadPoolExecutor或ScheduledThreadPoolExecutor执行。它们之间的区别是Runnable不会返回结果，而Callable可以返回结果。

除了可以自己创建实现Callable接口的对象外，还可以使用工厂类Executors来把一个Runnable包装成一个Callable。
```java
public static Callable<Object> callable(Runnable task)
```
下面是Executors提供的，把一个Runnable和一个待返回的结果包装成一个Callable的API
```java
public static <T> Callable<T> callable(Runnable task, T result)
```





![Executor框架的使用示意图](http://blog.sjjtcloud.com/blog/20191122/jh9FyfKXLM3U.png?imageslim)


