---
title: Cenos7.5环境搭建目录
tags: centos
categories: linux
---

### 1.Linux 设置服务器时区:
1、安装ntp服务软件包：yum install ntp
2、将ntp设置为缺省启动：systemctl enable ntpd
3、修改启动参数，增加-g -x参数，允许ntp服务在系统时间误差较大时也能正常工作：vi /etc/sysconfig/ntpd
4、启动ntp服务：/bin/systemctl restart ntpd
5、将系统时区改为上海时间（即CST时区）：ln -sf /usr/share/zoneinfo/Asia/Shanghai /etc/localtime
6、输入date命令查看时间是否正确



### 2. 搭建FTP(vsftp)
https://blog.csdn.net/zy517863543/article/details/80028351

  遇到的问题：
  1.安装成功后, Filezilla可以正常上传下载文件，但是java程序中使用Ftp工具类无法上传成功？
   解决方法： 程序中需要指定使用被动模式

### 3.搭建SFTP
参考博客(https://blog.csdn.net/fenglailea/article/details/78631760)


### 4.下载并安装redis
https://www.cnblogs.com/zuidongfeng/p/8032505.html

遇到的问题: https://blog.csdn.net/jexhen/article/details/76098622
复制到etc下的配置
/etc/redis/6379.conf (自动启动时生效的是这个,两个配置文件做好同步)


### 5.搭建java环境
https://blog.csdn.net/sizhezhongnian/article/details/81142976
JAVA根目录: /usr/java/jdk1.8


### 6.搭建并配置NGINX服务器
安装第三方yum源

#### 安装下载wget
yum install wget
#### 下载

wget http://www.atomicorp.com/installers/atomic

##### 安装

sh ./atomic

#### 更新yum源

yum check-update

开始安装nginx

#### 删除系统自带的软件包

yum remove httpd* php*

#### 安装nginx

yum install -y nginx

#### 设置nginx开机启动

chkconfig nginx on

#### 启动nginx

service nginx start

### 7. 搭建TOMCAT及参数优化
安装tomcat,并配置多tomcat
https://blog.csdn.net/w410589502/article/details/77988912 
https://blog.csdn.net/weixin_41004350/article/details/78492500


### 8. 安装MYSQL(5.7) 数据库

参考博客: https://www.cnblogs.com/freely/p/8087885.html
配置文件 vim /etc/my.cnf
注意更改端口号为13306 需要加入防火墙

### 9. 防火墙配置及管理
CentOS7使用firewalld打开关闭防火墙与端口

1、firewalld的基本使用
```
启动服务   systemctl start firewalld
关闭服务   systemctl stop firewalld
查看状态   systemctl status firewalld 
开机禁用   systemctl disable firewalld
开机启用   systemctl enable firewalld
```

2.systemctl是CentOS7的服务管理工具中主要的工具，它融合之前service和chkconfig的功能于一体。

```
启动一个服务：systemctl start firewalld.service
关闭一个服务：systemctl stop firewalld.service
重启一个服务：systemctl restart firewalld.service
显示一个服务的状态：systemctl status firewalld.service
在开机时启用一个服务：systemctl enable firewalld.service
在开机时禁用一个服务：systemctl disable firewalld.service
查看服务是否开机启动：systemctl is-enabled firewalld.service
查看已启动的服务列表：systemctl list-unit-files|grep enabled
查看启动失败的服务列表：systemctl --failed
```

3.配置firewalld-cmd
```
查看版本： firewall-cmd --version
查看帮助： firewall-cmd --help
显示状态： firewall-cmd --state
查看所有打开的端口： firewall-cmd --zone=public --list-ports
更新防火墙规则： firewall-cmd --reload
查看区域信息:  firewall-cmd --get-active-zones
查看指定接口所属区域： firewall-cmd --get-zone-of-interface=eth0
拒绝所有包：firewall-cmd --panic-on
取消拒绝状态： firewall-cmd --panic-off
查看是否拒绝： firewall-cmd --query-panic
```

4.怎样在防火墙中开启一个端口呢？
*  添加端口

 ```
 firewall-cmd --zone=public --add-port=80/tcp --permanent    
（--permanent永久生效，没有此参数重启后失效）
 ```

* 重新载入
	`firewall-cmd --reload`
* 查看
	`firewall-cmd --zone= public --query-port=80/tcp`

* 删除
	`firewall-cmd --zone= public --remove-port=80/tcp --permanent`




###10. centos7 常用命令

1、Apache 服务管理命令启动：
```
systemctl start httpd 启动
systemctl stop httpd 停止
systemctl restart httpd 重启
systemctl status httpd 状态
```

2、MySQL 服务管理命令启动：
```
systemctl start mysqld关闭：
systemctl stop mysqld重启：
systemctl restart mysqld状态：
systemctl status mysqld
```

3、FTP 服务管理命令启动：
```
systemctl start vsftpd   启动
systemctl stop vsftpd    停止
systemctl restart vsftpd 重启
systemctl status vsftpd  状态
```

4. Nginx服务管理命令启动
```
systemctl start nginx 启动
systemctl status nginx 查看状态
systemctl stop nginx 停止
```




