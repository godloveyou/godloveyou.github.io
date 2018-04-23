---
title: Mysql主从复制原理及步骤
categories:
- DataBase

tags:
- Mysql
- 主从复制
---

## Mysql主从复制的原理
>mysql内建有一种复制方式，即指定一台或多台服务器为主机master，另外一台或多台服务器作为从机slave。当master存在更新的时候，master将更新写入二进制文件，并维护了一个索引文件跟踪日志。当slave连接到主服务器之后，slave会通知master进行同步，master通过索引找日志文件上一次同步的位置，然后将这段时间内的更新数据发送给slave进行同步

### 主从配置的优缺点
优点：高可用，提高容错率，数据分布，负载均衡。

缺点：单向同步，无法解决主机宕机问题

### 主从配置先决条件
1. Mysql版本必须一致
2. 保证两台主机的mysql远程连接开启

### 目标
主库从库均创建要给名为auth的数据库
3. 主库ip:192.168.1.121 
4. 从库ip:192.168.1.107

### 主机(主库)配置
##### 1. 找到Mysql安装目录下的my.ini配置文件,在[mysqld]下增加如下配置

```sql
server-id=66 #id唯一
log-bin=E:/phpStudy/MySQL/log-bin #同步日志的文件存放路径
binlog-do-db=auth #备份哪些些数据库的二进制日志
#binlog-ignore-db=test  #也可以直接设置哪些数据库不同步
```
##### 2. 重启mysql服务,可以在上面配置的日志文件存储路径下找到索引文件,表示成功

##### 3. 登陆mysql，给从机配置登录名，登陆，密码和权限
```sql
grant replication slave,reload,super on *.*  to slave@192.168.1.121  identified by 'changpan';
```
##### 4. 查看主库状态 配置从机时需要使用file和postion两个值
```sql
show master status;
```
![](https://images.cnblogs.com/cnblogs_com/fxmemory/1028818/o_%e4%b8%bb%e6%9c%ba%e7%8a%b6%e6%80%81.png)


### 从机(从库)配置
##### 1. 找到Mysql安装目录下的my.ini配置文件,在[mysqld]下增加如下配置
```sql
server-id=88 　　#保证整数唯一
replicate-do-db=auth  #同步哪一个数据库 跟主库一致
```
注意: 配置好之后重启数据库

##### 2. 测试是否能够登陆到主库,校验之前主库的配置
```sql
mysql -uslave -h 192.168.1.121 -pchangpan
```
##### 3.配置从库的同步链接信息,登陆从库,Navicat中执行下面命令
```sql
stop slave;      关闭从机同步连接
change master to master_host = '192.168.1.121', master_user='slave', master_password ='changpan', master_log_file='mysql-bin.000001',master_log_pos=593;
```
【注意】master_log_file,master_log_pos是主库中通过命令show master status查询到的数据

##### 4.开启同步,查看从库配置的有效性
```sql
start slave；     开启同步连接
show slave status;   查看主从机连接信息
```
输入命令后查看下面的结果
![](https://images.cnblogs.com/cnblogs_com/fxmemory/1028818/o_%e5%90%8c%e6%ad%a5%e7%8a%b6%e6%80%81.png)

##### 当上面查看到的状态中出现以下 两个值都为YES时,表示成功
Slave_IO_Running:Yes
Slave_SQL_Running:Yes

##### 5. 测试,在主库中添加一个新表或表中插入新的数据，进行测试

#### 已有主库,怎样在此基础上进行主从同步呢？
##### 1.锁定或者阻断主库中的表进写操作,导出sql结构和数据
```sql
mysql> FLUSH TABLES WITH READ LOCK;
mysqldump -u root -p123456 --opt -R openser > openser20121203.sql；
mysql> UNLOCK TABLES;
```

##### 2.从库中关闭同步链接，然后导入sql、
```sql
stop slave;
```

##### 3.关闭从库链接,重启从库链接即可
```sql
start slave;
```
##### 4.进行测试OK
