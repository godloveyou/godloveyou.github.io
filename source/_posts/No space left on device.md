---
title: No space left on device问题解决
categories: linux
tags: centos
---

### 最近采集器可以正常采集数据,但是数据一直未入库,后来试着停止数据库后启动时,mysql报错No space left on device


### 解决
1. 使用 **df -h** 命令查看磁盘使用情况如下：

![mark](http://blog.sjjtcloud.com/blog/20191025/Nh43xnANffMH.png?imageslim)
问题发生时/dev/vda1磁盘使用了100%被完全占满,这个是删除数据后截的图

2. 接着，使用 **du -sh *  **命令 在根目录查看每个文件使用情况
![mark](http://blog.sjjtcloud.com/blog/20191025/M6LphdXthEkt.png?imageslim)

3. 最后发现是myql占用了100多G,逐层使用du -sh *最后发现是业务数据库其中一个表,数据占了将近100G
4. 删除该表数据后 问题解决




