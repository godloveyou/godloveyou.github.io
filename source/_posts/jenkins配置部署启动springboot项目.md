---
title: jenkins配置部署启动springboot项目
categories: Jenkins

tags:
- jenkins
---

### 1. jenkins连接远程ssh服务器需要安装一个ssh plugin插件
![mark](http://blog.nilaile.cn/blog/20190611/sNQOB7HX5xgK.png)

### 2. 配置一个全局凭证,就是连接远程服务器的用户名密码
![mark](http://blog.nilaile.cn/blog/20190611/J2v35kKCBQAg.png)

### 3.在Jenkins系统设置--》SSH remote hosts--》下配置一个ssh服务器
![mark](http://blog.nilaile.cn/blog/20190611/FiMYqpScLXsv.png)

### 4. 新建任务,输入要执行的脚本
![mark](http://blog.nilaile.cn/blog/20190611/woWNtKXfa6V2.png)

Jenkins配置的脚本：
```
#!/bin/bash
echo "stop collector"
sh /home/deploy/stop.sh
echo "start collector"
sh /home/deploy/start.sh
```

### stop.sh脚本如下
```
#!/bin/bash
echo "Stopping tooldin-mq"
pid=`ps -ef | grep admin.jar | grep -v grep | awk '{print $2}'`
if [ -n "$pid" ]
then
   echo "kill -9 的pid:" $pid
   kill -9 $pid
fi
```


### start.sh脚本如下
```
#!/bin/bash
source /etc/profile # 注意这行代码
nohup java  -Dfile.encoding=utf-8 -jar /home/deploy/admin.jar > /usr/local/collector/admin.log &


```


### 遇到的问题
在Jenkins中执行该新建的任务时,系统一直无法启动成功,stop.sh脚本确实执行成功了
但是项目一直未在后台启动成功

#### 解决思路
1. 先是按照网上的教程更改了jenkins中配置的执行脚本,防止jenkins进程结束后杀死子进程,更改后测试无效
```
#!/bin/bash
BUILD_ID=DONTKILLME
echo "stop collector"
sh /home/deploy/stop.sh
echo "start collector"
sh /home/deploy/start.sh
```
![mark](http://blog.nilaile.cn/blog/20190611/qlrYXcTwC80C.png)

2. 然后把 start.sh 错误输出指向标准输出
```
#!/bin/bash
nohup java -jar /home/deploy/admin.jar > /home/deploy/admin.log 2>&1 &

```

3. 然后在jenkins中启动任务,然后cat admin.log中打印出了错误的日志

```
nohup: failed to run command `java': No such file or directory
```

看到这个错误，猜想多半是由于java未配置环境变量，但是服务器环境变量确实正常，输入java --version可以看到是能够正常响应的,最终只能在start.sh脚本中增加 `source /etc/profile`


4. 最后特别感谢饺子哥的帮助


