## Centos7 安装Docker

安装参考：https://blog.csdn.net/u010046908/article/details/79553227
安装最新版本 https://blog.csdn.net/kongxx/article/details/78361048


阿里云镜像仓库地址： https://dev.aliyun.com/search.html

常用命令
docker pull centos  拉取centos
docker images 查看本地所有镜像
docker run hello-world 运行实例

docker run -i -t -v /root/sortware/:/mnt/sortware/ 75835a67d134 /bin/bash

### 退出
exit
### 关闭
docker stop mycentos

### 重启
docker start mycentos

### 重启后,在mycentos再打开/bin/bash
docker exec -ti mycentos /bin/bash

1、初步安装和启动docker
yum update -y
yum -y install docker
systemctl start docker


2、设置镜像
vi /etc/docker/daemon.json

{
  "registry-mirrors": ["https://aj2rgad5.mirror.aliyuncs.com"]
}

3、重启docker
systemctl daemon-reload
systemctl restart docker.service



4、测试docker是否正常安装和运行
docker run hello-world

5、查看结果
Hello from Docker!