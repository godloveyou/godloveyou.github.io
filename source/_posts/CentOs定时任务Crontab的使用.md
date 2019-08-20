---
title: CentOs定时任务Crontab的使用
categories: LINUX
tags:
- crontab
---
>  Linux 下通过crontab来执行定时任务,当安装完成操作系统之后，默认便会启动此任务调度命令。crond命令每分锺会定期检查是否有要执行的工作，如果有要执行的工作便会自动执行该工作。


## linux任务调度的工作主要分为以下两类：

- 1、系统执行的工作：系统周期性所要执行的工作，如备份系统数据、清理缓存
- 2、个人执行的工作：某个用户定期要做的工作，例如每隔10分钟检查邮件服务器是否有新信，这些工作可由每个用户自行设置

## 语法
crontab [ -u user ] file
或
crontab [ -u user ] { -l | -r | -e }

## 参数说明
* -u user：用来设定某个用户的crontab服务,这个前提是你必须要有其权限(比如说是 root)才能够指定他人的时程表。如果不使用 -u user 的话，就是表示设定自己的时程表。

* file：file是命令文件的名字,表示将file做为crontab的任务列表文件并载入crontab。如果在命令行中没有指定这个文件，crontab命令将接受标准输入（键盘）上键入的命令，并将它们载入crontab

* -e：编辑某个用户的crontab文件内容。如果不指定用户，则表示编辑当前用户的crontab文件。
* -l：显示某个用户的crontab文件内容，如果不指定用户，则表示显示当前用户的crontab文件内容。

* -r：从/var/spool/cron目录中删除某个用户的crontab文件，如果不指定用户，则默认删除当前用户的crontab文件.

## crontab的文件格式
![mark](http://blog.sjjtcloud.com/blog/20190806/6dVEtGXqD2FO.png?imageslim)

<!-- more -->

分 时 日 月 星期 要运行的命令

第1列分钟0～59
第2列小时0～23（0表示子夜）
第3列日1～31
第4列月1～12
第5列星期0～7（0和7表示星期天）
第6列要运行的命令

## 常用方法

列出crontab文件
`$ crontab -l `

编辑crontab文件
`$ crontab -e`
可以像使用vi编辑其他任何文件那样修改crontab文件并退出。如果修改了某些条目或添加了新的条目，那么在保存该文件时， cron会对其进行必要的完整性检查。如果其中的某个域出现了超出允许范围的值，它会提示你。 我们在编辑crontab文件时，没准会加入新的条目

删除crontab文件
`$crontab -r`

注解
最好在crontab文件的每一个条目之上加入一条注释，这样就可以知道它的功能、运行时间，更为重要的是，知道这是哪位用户的定时作业。

```
实例1：每1分钟执行一次myCommand
 * * * myCommand

实例2：每小时的第3和第15分钟执行
	3,15 * * * * myCommand
	
实例3：在上午8点到11点的第3和第15分钟执行
	3,15 8-11 * * * myCommand

实例4：每隔两天的上午8点到11点的第3和第15分钟执行
	3,15 8-11 */2  *  * myCommand

实例5：每周一上午8点到11点的第3和第15分钟执行
	3,15 8-11 * * 1 myCommand

实例6：每晚的21:30重启smb
	30 21 * * * /etc/init.d/smb restart

实例7：每月1、10、22日的4 : 45重启smb
	45 4 1,10,22 * * /etc/init.d/smb restart

实例8：每周六、周日的1 : 10重启smb
	10 1 * * 6,0 /etc/init.d/smb restart

实例9：每天18 : 00至23 : 00之间每隔30分钟重启smb
	0,30 18-23 * * * /etc/init.d/smb restart

实例10：每星期六的晚上11 : 00 pm重启smb
	0 23 * * 6 /etc/init.d/smb restart

实例11：每一小时重启smb
	* */1 * * * /etc/init.d/smb restart

实例12：晚上11点到早上7点之间，每隔一小时重启smb
	0 23-7 * * * /etc/init.d/smb restart

```


## 案例(每2分钟执行一次test.sh中脚本)
```
SHELL=/bin/bash
PATH=/sbin:/bin:/usr/sbin:/usr/bin
MAILTO=root
HOME=/

*/2 * * * * sh /home/backup/test.sh

```
### 查看cron执行的日志
```
vim /var/log/cron 看到如下记录日志,发现linux在执行任务后向管理员发送了邮件,但是这个邮件没有发送成功
```

```
Aug  6 10:44:01 iZ3i4n96yv24gsZ CROND[2868]: (root) CMD (sh /home/backup/test.sh)
Aug  6 10:44:01 iZ3i4n96yv24gsZ CROND[2867]: (root) MAIL (mailed 2802 bytes of output but got status 0x004b#012)
Aug  6 10:45:01 iZ3i4n96yv24gsZ CROND[3976]: (root) CMD (sh /home/backup/test.sh)
Aug  6 10:45:01 iZ3i4n96yv24gsZ CROND[3975]: (root) MAIL (mailed 2802 bytes of output but got status 0x004b#012)
Aug  6 10:46:01 iZ3i4n96yv24gsZ CROND[5031]: (root) CMD (sh /home/backup/test.sh)
Aug  6 10:46:01 iZ3i4n96yv24gsZ CROND[5030]: (root) MAIL (mailed 2802 bytes of output but got status 0x004b#012)
```

###　问题：如果想要查看详细的任务执行日志,要查看登录用户的邮箱,本地测试时使用的root用户, 查看发现无root用户的任何邮件

### 解決方式
1. 查看mail的发送日志 `vim /var/log/maillog`  原因是 parameter inet_interfaces: no local interface found for ::1
```

Aug  6 10:42:01 iZ3i4n96yv24gsZ postfix/sendmail[584]: fatal: parameter inet_interfaces: no local interface found for ::1
Aug  6 10:43:01 iZ3i4n96yv24gsZ postfix/sendmail[1817]: fatal: parameter inet_interfaces: no local interface found for ::1
Aug  6 10:44:01 iZ3i4n96yv24gsZ postfix/sendmail[2870]: fatal: parameter inet_interfaces: no local interface found for ::1
Aug  6 10:45:01 iZ3i4n96yv24gsZ postfix/sendmail[3978]: fatal: parameter inet_interfaces: no local interface found for ::1
Aug  6 10:46:01 iZ3i4n96yv24gsZ postfix/sendmail[5033]: fatal: parameter inet_interfaces: no local interface found for ::1

```

2. 通过google发现解决方式为

   ```xml
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

3. 再等待一个周期后,收到了定时任务推送的邮件,查看邮件内容,一切正常
