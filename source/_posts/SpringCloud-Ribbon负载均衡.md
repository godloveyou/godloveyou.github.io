---
title: SpringCloud-Ribbon负载均衡
categories: spring cloud
tags: Ribbon
---

## Ribbon简介
Ribbon是Netflix开源的负载均衡组件, 负载均衡是指将负载分摊到多个执行单元上，常见的负载均衡有两种方式。 
1. 一种是 独立进程单元，通过负载均衡策略,将请求转发到不同的执行单元上,例如 Ngnix 
2. 另一种是将负载均衡逻辑以代码的形式封装到服务消费者的客户端上，服务消费者客户端维护了一份服务提供的信息列表，有了信息列表, 就可以通过负载均衡策略将请求分摊给多个服务提供者，从而达到负载均衡的目的, 如Ribbon


## 使用方式
方式一：使用Ribbon和RestTemplate结合
方式二：和Feign结合(Feign已经默认集成了Ribbon)