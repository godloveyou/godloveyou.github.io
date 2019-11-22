---
title: Executor
categories: Executor框架
tags: java多线程
---
### 简介
Java的线程既是工作单元，也是执行机制。从JDK 5开始，把工作单元与执行机制分离开来。工作单元包括Runnable和Callable，而执行机制由Executor框架提供。

![Executor框架的成员及其关系](http://blog.sjjtcloud.com/blog/20191122/lELRVLBzyvMY.png?imageslim)


### Executor框架包括3大部分：

（1）任务。也就是工作单元，包括被执行任务需要实现的接口：Runnable接口或者Callable接口；

（2）任务的执行。也就是把任务分派给多个线程的执行机制，包括Executor接口及继承自Executor接口的ExecutorService接口, Executor框架有两个关键类实现了ExecutorService接口（ThreadPoolExecutor和ScheduledThreadPoolExecutor）

（3）异步计算的结果。包括Future接口及实现了Future接口的FutureTask类。


### 各个类和接口简介
* Executor 它是框架基础, 将任务的提交 和任务的执行分离开来
* ThreadPoolExecutor 是线程池的核心实现类,用来执行被提交的任务
* Future接口及实现FutureTask 代表异步计算的结果
* Runnable接口和Callable接口的实现类 都可以被ThreadPoolExecutor和ScheduledThreadPoolExecutor执行

![Executor框架的使用示意图](http://blog.sjjtcloud.com/blog/20191122/jh9FyfKXLM3U.png?imageslim)


