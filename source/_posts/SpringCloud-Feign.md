---
title: SpringCloud-Feign
categories: spring cloud
tags: Feign
---
>Feign是一个声明式的伪Http客户端，它使得写Http客户端变得更简单。使用Feign，只需要创建一个接口并注解。它具有可插拔的注解特性，可使用Feign 注解和JAX-RS注解。Feign支持可插拔的编码器和解码器。Feign默认集成了Ribbon，并和Eureka结合，默认实现了负载均衡的效果。

## 特点
* Feign 采用的是基于接口的注解
* Feign 整合了ribbon，具有负载均衡的能力
* Feign 整合了Hystrix，具有熔断的能力



## Hystrix
在分布式系统中，服务与服务之间的依赖错综复杂，一种不可避免的情况就是某些服务出现故障，导致依赖于它们的其他服务出现远程调度的线程阻塞
Hystrix是Netflix公司开源的 个项目，它提供了熔断器功能，能够阻止分布式系统中出现联动故障
Hystrix是通过隔离服务的访问点阻止联动故障的，并提供了故障的解决方案,从而提高了整个分布式系统的弹性
