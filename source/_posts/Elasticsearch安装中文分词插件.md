---
title:  Elasticsearch安装中文分词插件ik
categories: elasticsearch

tags:
- elasticsearch
---

Elasticsearch自带standard分词只能把汉语分割成一个个字，而不能分词、分段，这就是我们需要分析器ik的地方了。

### 安装结果: 
`http:``//{ip}:9200/_analyze?analyzer=standard&pretty=true&text=sojson中华人民共和国`

![mark](http://blog.nilaile.cn/blog/20190613/PXqmfSip2ESr.png)

### 安装步骤

访问浏览器（虚拟机IP:9200）查看你elasticsearch的版本号
![mark](http://blog.nilaile.cn/blog/20190613/r7kxHm3y7Ta2.png)

**一、下载ik的相应版本**

查看版本和下载链接[点击这里](https://github.com/medcl/elasticsearch-analysis-ik)

这里提供5.0.0的ES及以前的版本对应的ik版本

![mark](http://blog.nilaile.cn/blog/20190613/5oX0v1yjC6mz.png)

**二、下载的方式**

我这里的ES是2.3.5，对应的ik版本是1.9.5

在版本里选择1.9.5，点击 【Download ZIP】右键选择 -- 复制链接地址

![](https://images2015.cnblogs.com/blog/1059579/201702/1059579-20170207145657885-2081174365.png)

`wget https://github.com/medcl/elasticsearch-analysis-ik/archive/v1.9.5.zip`

**三、解包**（如果没有unzip命令的使用  **yum install zip unzip ***  命令安装）

`unzip v1.9.5.zip `

**四、 打包**

因为是java的源码，需要用maven打包

进入解压后的目录elasticsearch-analysis-ik-1.10.1里面，输入下面命令进行打包

`cd elasticsearch-analysis-ik-1.9.5/`

`mvn clean package`

（注：如果没有mvn的可以直接使用 **yum install java*** 进行安装，把ES的环境也一起配置好）

**五、配置**

1、打包后，在当前目录下有target目录，进去，有一个releases目录，把里面的zip包复制到你安装ES目录下的plugins目录下的ik目录（ik目录需要手动添加）

查找ES插件路径可以使用命令 
`whereis elasticsearch `

ES插件默认安装路径是：
`/usr/share/elasticsearch/plugins/`


mkdir -p /usr/share/elasticsearch/plugins/ik

cd target/releases/

mv elasticsearch-analysis-ik-1.9.5.zip /usr/share/elasticsearch/plugins/ik

[![复制代码](https://common.cnblogs.com/images/copycode.gif)](javascript:void(0); "复制代码")

2、使用unzip命令解包，并把zip包删除了

```language
cd /usr/share/elasticsearch/plugins/

unzip elasticsearch-analysis-ik-1.9.5.zip

rm -rf elasticsearch-analysis-ik-1.9.5.zip

```


**六、测试**

重启ES服务，查看ES的状态为（Active: active (running)）即可。

systemctl restart elasticsearch

systemctl status elasticsearch

如果需要具体测试的话，可以在官网git的下面介绍有（[点击查看](https://github.com/medcl/elasticsearch-analysis-ik/tree/v1.10.1)），简单的方法可以使用下面命令在浏览器访问确认

```language
http://（虚拟机ip）:9200**/_analyze?analyzer=ik&pretty=true&text=helloworld,中华人民共和国"
```

![mark](http://blog.nilaile.cn/blog/20190613/PXqmfSip2ESr.png)



