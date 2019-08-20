
---
title: CenOS7.5使用crontab定时备份数据库脚本
categories: LINUX
tags:
- Mysql
---

### 1.编写备份脚本mysqlBackup.sh 
```
#!/bin/sh

DB_IP=localhost
DB_PORT=3306
DB_USER=root
#密码 #＆这些需要转义加＼
DB_PASSWORD=l\#cxhd3\&0vx
DB_NAME=business
DB_BACKUP_DIR=/home/backup/mysql

#切换目录
cd $DB_BACKUP_DIR
day=`date +%Y%m%d`

#删除
rm -rf $day
mkdir $day
cd $day

echo "================  开始备份..  ================="
tables=(tb_user tb_dept)
for table in ${tables[@]};
do
 backup_file_name="${table}.sql"
 mysqldump -h${DB_IP} -u${DB_USER} -p${DB_PASSWORD} -P${DB_PORT} $DB_NAME $table>$backup_file_name
done

echo "================  结束备份..  ================="
```
<!-- more -->

### 2.配置邮件配置(不配置的话,Crontab执行后邮件无法发送)
```
vi  /etc/postfix/main.cf
发现配置为：
inet_interfaces = localhost
inet_protocols = all

改成：
inet_interfaces = all

inet_protocols = all


重新启动
service postfix start
```

### 3.配置Crontab任务
```
$ crontab -e

SHELL=/bin/bash
PATH=/sbin:/bin:/usr/sbin:/usr/bin
MAILTO=root
HOME=/

*/2 * * * * sh /home/backup/mysqlBackup.sh
```

### 4. 查看任务执行日志
`vim /var/log/cron`

### 查看任务执行详情可以通过查看LINUX系统邮件,任务执行后Crontab会发送邮件给添加该任务的用户

mail 常用命令
```
mail 打开邮件列表
mail p 查看邮件详情
mail z 返回邮件列表
mail d 删除邮件
mail q 保存退出
```



