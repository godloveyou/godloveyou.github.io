---
title: jenkins安装
tags: jenkins
categories: JAVA运维工具
---

#### 安装环境
>安装环境 centos7.5,jdk1.8
>参考博客: https://www.jianshu.com/p/180fb11a5b96


#### 安装步骤
注意: 安装前需要确认一下系统是否已经安装并配置好java环境变量
1.下载依赖
```
sudo wget -O /etc/yum.repos.d/jenkins.repo https://pkg.jenkins.io/redhat-stable/jenkins.repo
```

2.导入秘钥
```
sudo rpm --import https://pkg.jenkins.io/redhat-stable/jenkins.io.key
```
3.安装
``` 
yum install jenkins  
```

4.查看安装路径
```
rpm -ql jenkins
```

5.修改默认端口号
```
vi /etc/sysconfig/jenkins
```
找到JENKINS_PORT="8080",修改为自己的端口号如8090

6.启动jenkins 
```
systemctl start jenkins
```

7.如果启动报如下错误,需要修改jenkins的配置文件
```
Starting Jenkins bash: /usr/bin/java: No such file or directory 
```
修改Jenkins启动配置文件，指定java安装路径。
```
vim /etc/init.d/jenkins
```
打开配置文件后找到candidates这行，在下面增加自己的java安装路径
```
candidates="
/usr/java/jdk1.8/bin/java ## 这里 增加自己的java安装路径，下面的不用动
/etc/alternatives/java
/usr/lib/jvm/java-1.8.0/bin/java
/usr/lib/jvm/jre-1.8.0/bin/java
/usr/lib/jvm/java-1.7.0/bin/java
/usr/lib/jvm/jre-1.7.0/bin/java
/usr/bin/java
"
```

相关目录说明
```
jenkins相关目录释义：
1. /usr/lib/jenkins/：jenkins安装目录，war包会放在这里。
2. /etc/sysconfig/jenkins：jenkins配置文件，“端口”，“JENKINS_HOME”等都可以在这里配置。
3. /var/lib/jenkins/：默认的JENKINS_HOME。
4. /var/log/jenkins/jenkins.log：jenkins日志文件。
5. /etc/init.d/jenkins： jenkins启动配置文件这里需要配置java安装目录
```

#### 常用命令

1.启动 
```
方法1: java -jar /usr/lib/jenkins/jenkins.war --httpPort=8090
方法2： service jenkins start
方法3: systemctl start jenkins
```
浏览器访问 http://localhost:8090 输入初始密码即可
初始密码路径位于 
```
/var/lib/jenkins/secrets/initialAdminPassword
```

2.停止
```
service jenkins stop
```

3.查看运行状态
```
service jenkins start
```