---
title:  Mysql存储emoji表情错误问题
tags: mysql
categories: 数据库
---

问题: 插入emoji表情字符串时,数据库抛出下面的异常信息: 
```
Incorrect string value: '\xF0\x9F\x98\x81' for column 'job' at row 23  
```
解决方法 ：
1. windows服务器
修改my.ini配置文件,修改编码
```	
[mysql]  
default-character-set=utf8mb4
[mysqld]  
character-set-server=utf8mb4
```

2. 重启Mysql服务

3. 先查看数据库的编码方式：

```
show variables like '%char%';
```

4. 修改整个表的编码方式：

```
alter table user convert to character set utf8mb4 collate utf8mb4_bin;  
```

5. 修改某个字段的编码方式：
```
ALTER TABLE user_patient MODIFY COLUMN user_name varchar(255) CHARACTER SET utf8mb4 COLLATE utf8mb4_unicode_ci DEFAULT NULL COMMENT '姓名'
```