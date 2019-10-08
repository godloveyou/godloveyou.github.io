---
title: Centos7不能ping通主机,主机可以ping通虚拟机
categories:  linux
tags: vmware虚拟机
---

## 在VM中安装了一个 CentOS 7.4 系统 ，使用的是桥接模式，宿主机可以ping通虚拟机，虚拟机却ping不能宿主机。但是可以ping通外网域名，例如：www.baidu.com


## 解决办法：更改windows防火墙设置

打开windows防火墙–高级设置–入站规则： 

![](https://img-blog.csdn.net/20171206131218142?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvcXFfMzI3ODY4NzM=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/Center)
