---
title: centOS ssh连接 一段时间无操作连接自动断开问题
tags: linux
categories: centos
---

解决方法：

1 修改  /etc/ssh/sshd_config 文件
执行命令：vi /etc/ssh/sshd_config

中间部分有：

#### ClientAliveInterval 0

#### ClientAliveCountMax 3



改为：


ClientAliveInterval 60

ClientAliveCountMax 60

修改后如下用图：



ClientAliveInterval指定了服务器端向客户端请求消息的时间间隔, 默认是0, 不发送.而ClientAliveInterval 60表示每分钟发送一次, 然后客户端响应, 这样就保持长连接了

ClientAliveCountMax表示服务器发出请求后客户端没有响应的次数达到一定值, 就自动断开 。

2重启sshd （必须的否则无效）
执行命令：/bin/systemctl restart  sshd.service
我有在网上找这个问题的解决方法，有的只改参数，没有讲重启sshd。有的说了但是呢在我的机子上命令不能用，例如：/etc/init.d/sshdstart   /etc/rc.d/init.d/sshd restart