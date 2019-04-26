---
title: Docker常用命令
categories: Docker
tags:
- docker命令
---

### 镜像命令

```
查看本机的镜像,可通过该方法查看到镜像id等信息.
docker images 

获取镜像 name：镜像名  [:tag]：版本，默认为最新的（也就是会自己加上一个参数:latest）
docker pull [options] name[:tag]

删除镜像,需要删除其下所有容器
docker rmi <镜像id>

运行镜像,构建出一个容器. -d表示后台运行. 
docker run -d image 
-p 8080:80  进行端口映射，将nginx的80端口映射到主机的8080端口上，也就是别人访问8080，可以访问到自己的80

查看目前正在运行的容器
docker ps

查看所有容器
docker ps -a
```

### 容器命令

```
重要:xx表示不同的命令如，pull、run等。可以查看该命令的帮助，所有参数
docker xx --help

停止容器
docker stop <容器id>

删除容器
docker rm <容器id>

启动一个运行(run)过的容器
docker start <容器id>

在运行的容器中执行命令 
docker exec [options] container command [arg...]
例如:   
docker exec -it <容器id> bash
可以进入一个容器，和虚拟机中一样。也就是容器内部

挂载目录:将宿主机的文件共享给容器
docker run -d --name=test -v /opt/test:/usr/databases docker-test 
test是容器的名字，需唯一；
-v表示创建一个数据卷并挂载到容器里，
示例表示把宿主机的/opt/test目录挂载到容器的/usr/databases目录下；
docker-test是镜像的名字

查看容器当前信息,可在该命令的 Mounts信息中,找到挂载目录信息
docker inspect <容器id>

运行容器
-d表示后台运行 -p表示设置端口映射， jpress是镜像名
docker run -d -p  8888:8080 jpress

运行mysql镜像
docker run -d -p 3306:3306 -e MYSQL_ROOT_PASSWORD=123456 -e MYSQL_DATABASE=xxx  images(镜像名)

```