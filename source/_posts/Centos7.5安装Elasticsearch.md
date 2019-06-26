---
title:  Centos7.5安装Elasticsearch2.3.5
categories: elasticsearch

tags:
- elasticsearch
---

>  Elasticsearch是一个基于Lucene的搜索服务器,它需要依赖于java环境,所以需要先安装了JDK



### 安装方法一（下载rpm包方式）

** 1.百度elasticsearch,点击官网网址**

** ![](https://images2015.cnblogs.com/blog/1059579/201702/1059579-20170207111519713-1843270100.png)**

 2.点击Downloads，选择past releases

![](https://images2015.cnblogs.com/blog/1059579/201702/1059579-20170207111941963-554816516.png)

3.找到2.3.5的版本，（由于elasticsearch更新太快，插件更新进度跟不上，2.3.5的版本插件最齐全）

![](https://images2015.cnblogs.com/blog/1059579/201702/1059579-20170207112356557-1829265054.png)

4.右键点击RPM选择 -- 复制连接地址

![](https://images2015.cnblogs.com/blog/1059579/201702/1059579-20170207112633619-1394926966.png)

5.在根目录下载rmp包

`wget https://download.elastic.co/elasticsearch/release/org/elasticsearch/distribution/rpm/elasticsearch/2.3.5/elasticsearch-2.3.5.rpm`


6.执行下面的命令,安装完成后按照提示操作,即可

`rpm -ivh elasticsearch-2.3.5.rpm `

7.启动并查看安装状态

```
systemctl start elasticsearch

systemctl status elasticsearch

```

8.本地查看版本信息

`curl  http://127.0.0.1:9200`

![](https://images2015.cnblogs.com/blog/1059579/201702/1059579-20170207113314791-1988726404.png)

9.查找elasticsearch 安装路径与配置路径，并且配置

whereis elasticsearch

![](https://images2015.cnblogs.com/blog/1059579/201702/1059579-20170207113451932-347145346.png)

`vim /etc/elasticsearch/elasticsearch.yml`

修改：
  ![mark](http://blog.nilaile.cn/blog/20190613/38Ej7AxkGxTd.png)
  
 这样就可以在window系统用浏览器通过访问虚拟机的ip:port

保存退出后重启elasticsearch

`systemctl restart elasticsearch`

在浏览器访问虚拟机ip:9200
![mark](http://blognilaile.cn/blog/20190613/3HR3guW0ew98.png)


### 安装方法二 (yum方式安装)

一、ES的yum安装配置

由于ES不在yum的本地源，所以我们需要添加ES的yum配置。

1、下载并安装ES的yum公钥

`rpm --import https:``//packages.elastic.co/GPG-KEY-elasticsearch`

2、配置ES的yum源
`vim /etc/yum.repos.d/elasticsearch.repo`

 输入下面的内容：
```
[elasticsearch-2.x]
name=Elasticsearch repository for 2.x packages
baseurl=http://packages.elastic.co/elasticsearch/2.x/centos
gpgcheck=1
gpgkey=http://packages.elastic.co/GPG-KEY-elasticsearch
enabled=1
```


二、yum安装ES

1、更新yum的缓存
`yum makecache`

2、安装ES
`yum install elasticsearch`


三、测试ES

1、配置和启动ES服务器进程

`/sbin/chkconfig --add elasticsearch`

`systemctl start elasticsearch`

2、查看状态

`systemctl status elasticsearch`

3、运行测试
`curl -X GET localhost:9200`

返回的json结果如下：
```
{
  "name" : "Live Wire",
  "cluster_name" : "elasticsearch",
  "version" : {
    "number" : "2.3.5",
    "build_hash" : "90f439ff60a3c0f497f91663701e64ccd01edbb4",
    "build_timestamp" : "2016-07-27T10:36:52Z",
    "build_snapshot" : false,
    "lucene_version" : "5.5.0"
  },
  "tagline" : "You Know, for Search"
}

```


四、通过IP访问ES的配置（同方法一配置方法一样）
1、打开/etc/elasticsearch/elasticsearch.yml
`vim /etc/elasticsearch/elasticsearch.yml`

 network.host-> 把后面改为0.0.0.0或者虚拟机ip地址，
 http.port-> 去掉注释
  ![mark](http://blognilaile.cn/blog/20190613/38Ej7AxkGxTd.png)
  
 这样就可以在window系统用浏览器通过访问虚拟机的ip:port
![mark](http://blognilaile.cn/blog/20190613/3HR3guW0ew98.png)


### 安装中文分词插件参考[https://www.cnblogs.com/shifu204/p/6374234.html](https://www.cnblogs.com/shifu204/p/6374234.html)


