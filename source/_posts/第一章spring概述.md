---
title: 为什么要使用spring?
categories: java

tags:
- spring
---

> spring是一个开源的应用框架,目的是帮我们简化企业级应用程序的开发,spring帮助我们创建对象，管理对象之间的依赖,spring还有框架粘合剂的作用，帮我们整合各种我们需要的框架如hibernate,mybatis，struts2等. 



## Spring能帮我们做什么？
* sping帮助我们通过配置文件或者注解创建对象，管理对象之间的依赖关系.
* spring的AOP（面向切面编程）帮我们无耦合的实现日志记录，权限控制，性能统计等功能
* spring帮助我们管理数据库事物，获取连接，关闭连接的工作交由spring来完成
* spring提供了第三方数据访问框架的无缝集成(hibernate,mybatis,JPA)SPRING-JDBCB本身还提供了一套jdbc访问模板
* spring提供了第三方web框架的无缝集成(struts2,JSF),spring本身也提供了一套spring-mvc框架方便web层搭建
* spring能够帮我们更好的与JAVA EE整合(JAVA MAIL,任务调度,缓存)等


## 如何学好
要学好Spring，首先要明确Spring是个什么东西，能帮我们做些什么事情，知道了这些然后做个简单的例子，这样就基本知道怎么使用Spring了。Spring核心是IoC容器，所以一定要透彻理解什么是IoC容器，以及如何配置及使用容器，其他所有技术都是基于容器实现的；理解好IoC后，接下来是面向切面编程，首先还是明确概念，基本配置，最后是实现原理，接下来就是数据库事务管理，其实Spring管理事务是通过面向切面编程实现的，所以基础很重要，IoC容器和面向切面编程搞定后，其余都是基于这俩东西的实现，学起来就更加轻松了。要学好Spring不能急，一定要把基础打牢，基础牢固了，这就是磨刀不误砍柴工。