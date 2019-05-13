---
title: windows安装Nginx
categories: windows
tags:
- NGINX
---

## 1. 官方下载NGINX [download](http://nginx.org/en/download.html) 

## 2.解压运行

```
 cd c:\
 unzip nginx-1.12.2.zip
 cd nginx-1.12.2
 start nginx
```


## 3. 命令行查看

命令行运行  `tasklist` 可以看到nginx的两个进程

```language
C:\nginx-1.12.2>tasklist /fi "imagename eq nginx.exe"

 Image Name           PID Session Name     Session#    Mem Usage
=============== ======== ============== ========== ============
 nginx.exe            652 Console                 0      2 780 K
 nginx.exe           1332 Console                 0      3 112 K
```

有两个进程,一个进程是master进程 另一个是worker进程, 如果进程没有启动成功，可以查看日志文件 `logs\error.log`


Nginx常用命令

| 命令      | Model    |
| --------- | -------- |
| start nginx| 启动| 
| nginx -s stop| 迅速退出     |
| nginx -s quit| 优雅的退出 |
| nginx -s reload| 修改配置文件后重载    |



