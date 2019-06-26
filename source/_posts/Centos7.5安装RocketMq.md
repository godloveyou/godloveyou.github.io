---
title: Centos7.5安装RocketMq
categories: RocketMq
tags:
- RocketMq
---

### 1、下载apache最新rocketmq二进制压缩文件

下载地址：https://www.apache.org/dyn/closer.cgi?path=rocketmq/4.2.0/rocketmq-all-4.2.0-bin-release.zip

下载命令：wget https://mirrors.tuna.tsinghua.edu.cn/apache/rocketmq/4.2.0/rocketmq-all-4.2.0-bin-release.zip

### 2、解压安装

解压命令：unzip -d /dirname rocketmq-all-4.2.0-bin-release.zip

### 3、环境变量

nameserver环境变量：vi /etc/profile

添加：export NAMESRV_ADDR=127.0.0.1:9876

#### 4、启动

bin目录，启动nameserver和broker：

nohup sh mqnamesrv >mqname.log &

nohup sh mqbroker -n localhost:9876 >mqbroker.log &


查看进程：jps

启动broker时会卡顿，因为虚拟机内存和broker配置内存跟不上。

设置：给虚拟机预留1g的内存，这样启动时不太会卡：虚拟机设2g，nameserver和broker共用1g


#### 5、启动失败

查看启动日志：cat nohup.out

启动失败： Native memory allocation (malloc) failed to allocate 8589934592 bytes for committing reserved memory.

因为nameserver和broker的默认配置内存超过虚拟机的内存，需根据宿主机配置调整虚拟机内存，并调整nameserver和broker的默认内存配置。

bin目录：

vi runserver.sh 
![mark](http://blog.nilaile.cn/blog/20190613/Ym57z2V1GoSG.png)

vi runbroker.sh
![mark](http://blog.nilaile.cn/blog/20190613/MAdIT4CchpyP.png)


参考文章

[https://www.jianshu.com/p/04a98ba770a4]
[https://blog.csdn.net/paincupid/article/details/81333138]
