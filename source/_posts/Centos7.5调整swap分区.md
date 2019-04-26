---
title: centos7.5增加swap分区
categories: linux
tags:
- swap
---

先用free -m查看一下swap的大小

![](https://files.jb51.net/file_images/article/201804/201841291807170.png?201831291819)

1.添加swap分区 

使用dd命令创建/home/swap这么一个分区文件。文件的大小是2048000  个block，一般情况下1个block为1K，所以这里空间是2G。

```language
dd if=/dev/zero of=/var/swapfile bs=1024 count=2048000  //添加交换文件并设置其大小为2G
```


![](https://img-blog.csdn.net/20180627095255111?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3poYnpoYnpoYmJhYnk=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70) 

2.执行完毕，对交换文件格式化并转换为swap分区： 

mkswap /var/swapfile

![](https://img-blog.csdn.net/20180627095317795?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3poYnpoYnpoYmJhYnk=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)

3.挂载并激活分区： 

```
swapon /var/swapfile
```

![](https://img-blog.csdn.net/20180627095340170?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3poYnpoYnpoYmJhYnk=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)

4 赋权限 

```language
chmod -R 0600 /var/swapfile

```

现在再用free -m命令查看一下内存和swap分区大小，就发现增加了2G的空间了。

注意当计算机重启了以后，发现swap还是原来那么大，新的swap没有自动启动，还要手动启动。那我们需要修改/etc/fstab文件

5.设置开机自动挂载该分区： 
vi /etc/fstab 
在fstab文件末尾追加如下内容后:wq!保存即可： 

```language
/var/swapfile swap swap defaults 0 0
```

