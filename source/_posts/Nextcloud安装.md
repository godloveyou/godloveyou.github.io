---
title: 使用Nextcloud搭建私有云盘
categories: Nextcloud
tags: Nextcloud
---

## 1. 安装并运行(首次默认会通过搜索远程来拉取)
```
docker run -d --name nextcloud \
-p 8020:80 \
-v /data/nextcloud:/var/www/html \
nextcloud
```

## 2. 首次运行配置

```language
访问: http://安装的服务器ip:8020/
配置用户名密码,数据库用户名密码
注意: 实现创建好一个空的数据库,因为这里要使用到
```

## 3. 停止服务`docker stop 容器id`

## 4. 重启服务`docker start 容器id`

## App中使用nextcloud
![mark](http://blog.sjjtcloud.com/blog/20191226/97aLM514Kf0n.png?imageslim)

## 1. 下载Android/iphone市场搜索nextcloud并下载

## 2. 指定登录地址http://安装的服务器ip:8020/ 用户名,密码即可登录管理




参考博客: https://zhuanlan.zhihu.com/p/62987726