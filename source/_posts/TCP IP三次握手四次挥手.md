---
title: TCP/IP三次握手四次挥手
categories: TCP
tags: TCP/IP
---

**一、TCP报文格式**

  TCP报文格式图：

![](http://dl2.iteye.com/upload/attachment/0108/8317/ef70c29e-651b-33a4-b188-d4e8e0ff9915.png) 

  上图中有几个字段需要重点介绍下：

  （1）序号：Seq序号，占32位，用来标识从TCP源端向目的端发送的字节流，发起方发送数据时对此进行标记。

  （2）确认序号：Ack序号，占32位，只有ACK标志位为1时，确认序号字段才有效，Ack=Seq+1。

  （3）标志位：共6个，即URG、ACK、PSH、RST、SYN、FIN等，具体含义如下：

  （A）URG：紧急指针（urgent pointer）有效。

  （B）ACK：确认序号有效。

  （C）PSH：接收方应该尽快将这个报文交给应用层。

  （D）RST：重置连接。

  （E）SYN：发起一个新连接。

  （F）FIN：释放一个连接。


三次握手用wireshark抓包分析
![mark](http://blog.sjjtcloud.com/blog/20191008/XjQC8Jd9LHcm.png?imageslim)


第一次握手 SYN
![mark](http://blog.sjjtcloud.com/blog/20191008/4gfW0vbr3WOE.png?imageslim)

第二次握手 SYN,ACK
![mark](http://blog.sjjtcloud.com/blog/20191008/d5nltEXPMxL5.png?imageslim)

第三次握手 
![mark](http://blog.sjjtcloud.com/blog/20191008/ppSYnlie7IQE.png?imageslim)



# TCP 三次握手 示意图

![](https://images2018.cnblogs.com/blog/701983/201803/701983-20180314102251716-1929749101.png)

## Wireshark 抓包注意事项

为了演示一个TCP三次握手建立连接的过程，我们通过 Chrome 访问一个网页。 
已知 HTTP 协议就是建立在TCP链接上的

比如访问以下的网址： 
[http://toutiao.newmedia139.net/](http://toutiao.newmedia139.net/)

通过 Cmd 的 ping 命令获取 这个网站对应的 IP地址 **183.136.236.13** 
![](https://images2018.cnblogs.com/blog/701983/201803/701983-20180314102251934-555170248.png)

确定 这个IP 有一个非常重要的好处，就是我们只需要

电脑 -> 网站 的数据包

网站->电脑 的数据包

所以，可以使用Wireshark的显示过滤规则，只显示我们需要的数据，不然你一定看着满屏幕的数据抓狂的。

### 过滤规则如下：

ip.src==183.136.236.13 or ip.dst==183.136.236.13

### 截图：

![](https://images2018.cnblogs.com/blog/701983/201803/701983-20180314102252371-1979969603.png)

## 分析TCP握手包

## 概览

![](https://images2018.cnblogs.com/blog/701983/201803/701983-20180314102252636-937376226.png)

通过图片，可以看到 先 进行了 TCP 三次传输 然后才 开始 HTTP 传输

## 第一次 客户端发送 SYN 报文 到服务器

![](https://images2018.cnblogs.com/blog/701983/201803/701983-20180314102253092-1365683710.png)

## 第二次 ，服务器接收到 客户端的SYN 报文，回复 SYN + ACK 报文

![](https://images2018.cnblogs.com/blog/701983/201803/701983-20180314102253605-1241059132.png)

## 第三次 ，客户端接收到服务端的 SYN+ACK 报文后，回复 ACK报文

![](https://images2018.cnblogs.com/blog/701983/201803/701983-20180314102254051-694072128.png)

## 注意：

这里有个坑：Wireshark 显示的 Syn Ack的数目是不准确的

![](https://images2018.cnblogs.com/blog/701983/201803/701983-20180314102254264-644298919.png)

理论上，Syn 应该初始值是个随机数的，后面的要根据初始值增加 
![](https://images2018.cnblogs.com/blog/701983/201803/701983-20180314102254468-1577667548.png)

## TCP 三次握手总结

建立一个稳定的 双向 连接，最少需要 几次 通信呢？ 
以打电话为例 
小明 给小红 打电话 
小明 ： 喂，小红 听得到么？ 
小红： 嗯，我听到你说话了，你能听到我么？ 
小明：我能听到你。

只有这三个传输都正确了，才能保障双方是 连通的

# TCP 四次挥手

由于TCP连接是全双工的，因此每个方向都必须单独进行关闭。这个原则是当一方完成它的数据发送任务后就能发送一个FIN来终止这个方向的连接。收到一个 FIN只意味着这一方向上没有数据流动，一个TCP连接在收到一个FIN后仍能发送数据。首先进行关闭的一方将执行主动关闭，而另一方执行被动关闭。

CP的连接的拆除需要发送四个包，因此称为四次挥手(four-way handshake)。客户端或服务器均可主动发起挥手动作，在socket编程中，任何一方执行close()操作即可产生挥手操作。

（1）客户端A发送一个FIN，用来关闭客户A到服务器B的数据传送。

（2）服务器B收到这个FIN，它发回一个ACK，确认序号为收到的序号加1。和SYN一样，一个FIN将占用一个序号。

（3）服务器B关闭与客户端A的连接，发送一个FIN给客户端A。

（4）客户端A发回ACK报文确认，并将确认序号设置为收到序号加1。

TCP采用四次挥手关闭连接如图2所示。

![](https://images2018.cnblogs.com/blog/701983/201803/701983-20180314102255109-43618668.png)

# 抓包截图

![](https://images2018.cnblogs.com/blog/701983/201803/701983-20180314102255513-104940885.png)

其中 183.136.236.13 是服务器的ip 
可以看到 这一次挥手是由 服务器 发起的

## 第一次挥手 FIN +ACK

![](https://images2018.cnblogs.com/blog/701983/201803/701983-20180314102255745-1296704292.png)

## 第二次挥手 ACK

![](https://images2018.cnblogs.com/blog/701983/201803/701983-20180314102255981-1871970231.png)

## 第三次挥手 FIN +ACK

![](https://images2018.cnblogs.com/blog/701983/201803/701983-20180314102256594-1733317431.png)

## 第四次挥手 ACK

![](https://images2018.cnblogs.com/blog/701983/201803/701983-20180314102257013-202026294.png)

## 总结

TCP 由于是全双工的，断开链接需要四次挥手







