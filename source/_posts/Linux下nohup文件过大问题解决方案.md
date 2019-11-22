---
title: Linux下nohup文件过大问题解决方案
categories: linux
tags: nohup
---

### 文件过大可能对系统造成的影响
1. 文件打开非常慢
2. 系统线程被阻塞 无法正常运行（采集系统曾经碰到过）

### 解决方法1 直接指定到黑洞目录,在本地也不会保留,项目中一般都配置的有日志文件,所以这个文件其实是冗余的
```bash
#!/bin/bash
#nohup java -Dfile.encoding=utf-8 -jar $JAVA_OPTS  admin.jar > admin.log &
nohup java -Dfile.encoding=utf-8 -jar $JAVA_OPTS admin.jar > /dev/null &

```

### 解决方法2 切割日志文件

```bash
#!/bin/sh
#################启动日志切割服务#################
this_path=$(cd `dirname $0`;pwd)

cd $this_path
echo $this_path
current_date=`date -d "-1 day" "+%Y%m%d"`
echo $current_date
split -b 65535000 -d -a 4 /home/.../nohup.out   /home/.../log/log_${current_date}_  

cat /dev/null > nohup.out

```
#### split命令
 `split  -b 65535000 -d -a 4  nohup.out  ./log/log_${current_date}_` 这里使用split命令,将nouhup文件按指定大小切分(65535000b 大概60多M吧，可以自定义大小 )，并分成指定格式（-d -a 4以4位数字形式为后缀以从0000开始,具体可以百度split命令用法）,最终输出格式为log_20160610_0001


