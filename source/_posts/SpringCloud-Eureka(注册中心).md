---
title: SpringCloud-Eureka(注册中心)
categories: spring cloud
tags: Eureka
---

## 简介
> Eureka是Netflix公司开源的服务注册与发现组件。

## Eureka中的角色
1. 服务注册中心,它是一个Eureka Server,提供服务注册于发现的功能 
2. 服务提供者,它是一个Eureka Client,提供服务 
3. 服务消费者,它是一个Eureka Client,消费服务

## 服务消费的过程
1. 首先需要启动一个服务注册中心
2. 服务提供者,向服务注册中心注册,将自己的信息(服务名/IP地址等）通过RESET API的形式提交给服务注册中心Eureka Server
3. 服务消费者,也向服务注册中心注册,同时服务消费者会获取一份服务注册列表的信息,该列表包含了所有向服务注册中心注册的服务信息
4. 获取到服务注册列表后,服务消费者就知道服务提供者的IP地址，也就可以通过Http远程调度来消费服务提供者提供的服务

## Eureka基本概念
1. Register------服务注册

2. Renew-------服务续约
    正常情况下Eureka Client会每隔30秒发送一次心跳来进行服务续约,通过服务续约来告知Eureka Server该Eureka Client仍然可用,没有出现故障,正常情况下,Eureka Server如果90秒内未收到Eureka client的心跳，会将该Eureka client从服务列表中删除

3. Fetch Registers-----获取服务注册列表信息
    Eureka Client从Eureka Server获取服务注册信息,并将其缓存在本地,该列表信息定时更新(每隔30秒),Eureka Client和Eureka Server可以通过JSON/XML数据格式进行通信

4. Cancle-------服务下线
    Eureka Client在程序关闭时可以向Eureka Server发送下线请求,发送请求后 该客户端信息将从Eureka Server的服务注册列表中删除，该请求不是自动完成的,需要在程序关闭时调用
    `DiscoveryManager . getinstance() .shutdownComponent();`

5. Evication----服务剔除
 默认情况下,Eureka Server如果90秒内未收到Eureka Client的服务续约(心跳请求),Eureka Server会将该服务实例从注册列表删除，即服务剔除

## Eureka高可用架构

