---
title: linux文件系统
categories: linux
tags:
- 文件系统
---



# Linux文件系统

### 文件管理命令
```
df -hl 查看磁盘剩余空间

df -h 查看每个根路径的分区大小

du -sh [目录名] 返回该目录的大小

du -sm [文件夹] 返回该文件夹总M数

du -h [目录名] 查看指定文件夹下的所有文件大小（包含子文件夹）

查看硬盘的分区 #sudo fdisk -l

查看IDE硬盘信息 #sudo hdparm -i /dev/hda

查看STAT硬盘信息 #sudo hdparm -I /dev/sda 或 #sudo apt-get install blktool #sudo blktool /dev/sda id

查看硬盘剩余空间 #df -h #df -H

查看目录占用空间 #du -hs 目录名

优盘没法卸载 #sync fuser -km /media/usbdisk
```


1. 查看磁盘剩余空间
```java
[root@iZ3i4n96yv24gsZ collector]# df -h
Filesystem      Size  Used Avail Use% Mounted on
/dev/vda1       158G   20G  131G  13% /
devtmpfs        7.8G     0  7.8G   0% /dev
tmpfs           7.8G     0  7.8G   0% /dev/shm
tmpfs           7.8G  656K  7.8G   1% /run
tmpfs           7.8G     0  7.8G   0% /sys/fs/cgroup
tmpfs           1.6G     0  1.6G   0% /run/user/0

```

2. 查看目录各个文件占用的大小

```
    [root@iZ3i4n96yv24gsZ usr]# du -sh *
      513M    bin
      4.0K    etc
      4.0K    games
      9.6M    include
      369M    java
      571M    lib
      378M    lib64
      48M     libexec
      2.6G    local
      464M    sbin
      306M    share
      105M    src
      0       tmp

```


