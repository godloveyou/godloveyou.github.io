---
title: springboot以jar包启动,打包到测试服务器,中文乱码,数据库乱码
tags: 中文乱码
categories: springboot
---
原因
windows启动的jar的时候给Java虚拟机设置编码默认不是utf-8, 所以乱码,同时连接插入到数据库的很多数据也是乱码, linux系统的服务器编码默认是utf-8，对于是windows的服务器默认不是utf-8。所以在启动的时候需要设置编码方式。

解决方案: 启动jar包时设置编码
如下所示：java -Dfile.encoding=utf=8 -
``java -Dfile.encoding=utf-8 -jar rr-api.jar``