---
title: Docker远程连接配置
categories: Docker
tags:
- docker远程访问
---

## 无CA认证方式(不安全,建议用于测试环境）

1. 修改服务器配置，开放Docker的远程连接访问
```
[root@localhost ~]# vim /usr/lib/systemd/system/docker.service 
将ExecStart属性value值改为
/usr/bin/dockerd -H tcp://0.0.0.0:2375 -H unix://var/run/docker.sock
```

2. 重启docker
```
[root@localhost ~]# systemctl daemon-reload 
[root@localhost ~]# systemctl restart docker
```

3. 开启阿里云和本地的防火墙中 放行2375 端口

4. 远程通过IDEA测试OK

## CA 认证方式(安全访问方式适用于生产环境)

### 文件总览

├── ca-key.pem       # 妥善保管，连接时用不到
├── ca.pem           # clent & server
├── ca.srl           # 用不到
├── cert.pem         # client
├── client.csr       # 请求文件
├── extfile.cnf      # 配置文件
├── key.pem          # client
├── server-cert.pem  # server
├── server.csr       # 请求文件
└── server-key.pem   # server

### 生成步骤

1. 创建ca文件夹，存放CA私钥和公钥

```
[root@localhost ~]# mkdir -p /usr/local/ca
[root@localhost ~]# cd /usr/local/ca/

```


2. 生成 CA 私钥 (需要连续输入两次相同的密码)

```
[root@localhost ca]# openssl genrsa -aes256 -out ca-key.pem 4096
```


3. 生成 CA 公钥.输入上一步中设置的密码,然后输入(国家、省、市、组织名称等)

```
[root@localhost ca]# openssl req -new -x509 -days 365 -key ca-key.pem -sha256 -out ca.pem
```

4. 生成服务器私钥 `server-key.pem`

```
[root@localhost ca]# openssl genrsa -out server-key.pem 4096
```


5. `用私钥生成证书请求文件(重点)`

```
[root@localhost ca]# openssl req -subj "/CN=47.105.199.230" -sha256 -new -key server-key.pem -out server.csr
```

把下面命令中的$Host换成你自己服务器外网的IP或者域名
`openssl req -subj "/CN=$HOST" -sha256 -new -key server-key.pem -out server.csr`

比如
* IP:`openssl req -subj "/CN=192.168.1.106" -sha256 -new -key server-key.pem -out server.csr`
* 域名: `openssl req -subj "/CN=www.baidu.com" -sha256 -new -key server-key.pem -out server.csr`


6. 用 CA 来签署证书(也就是配置白名单)

```
[root@localhost ca]# echo subjectAltName = IP:192.168.1.106,IP:0.0.0.0 >> extfile.cnf
```

注意事项: 
这里就是配置白名单, 制定你接下来要允许那些ip可以连接到服务器的docker，因为已经是ssl连接，所以我推荐配置0.0.0.0,也就是所有ip都可以连接(但只有拥有证书的才可以连接成功)，这样配置好之后公司其他人也可以使用。如果你不想这样，那你可以配置ip，用逗号分隔开。下面的$Host依旧是你服务器外网的IP或者域名，请自行替换。

如果你填写的是ip地址的话命令如下

* `echo subjectAltName = IP:$HOST,IP:0.0.0.0 >> extfile.cnf`
如果你填写的是域名的话命令如下 
* `echo subjectAltName = DNS:$HOST,IP:0.0.0.0 >> extfile.cnf`
我这里使用局域网进行测试



7. 执行命令, 将Docker守护程序密钥的扩展使用属性设置为仅用于服务器身份验证

```
[root@localhost ca]# echo extendedKeyUsage = serverAuth >> extfile.cnf
```


8. 执行命令，并输入之前设置的密码，生成签名证书

```
[root@localhost ca]# openssl x509 -req -days 365 -sha256 -in server.csr -CA ca.pem -CAkey ca-key.pem \-CAcreateserial -out server-cert.pem -extfile extfile.cnf
Signature ok
subject=/CN=192.168.1.106
Getting CA Private Key
Enter pass phrase for ca-key.pem:
```

* * *

9. 生成客户端私钥(key.pem) 到时候把生成好的几个公钥私钥拷出去即可

```
[root@localhost ca]# openssl genrsa -out key.pem 4096

```

10. 用私钥生成证书请求文件

```
[root@localhost ca]# openssl req -subj '/CN=client' -new -key key.pem -out client.csr
```

11. 执行命令，要使密钥适合客户端身份验证，请创建扩展配置文件

```
[root@localhost ca]# echo extendedKeyUsage = clientAuth >> extfile.cnf
```


12. 用 CA 来签署证书,生成cert.pem(需要输入前面设置的密码,生成签名证书)

```
[root@localhost ca]# openssl x509 -req -days 365 -sha256 -in client.csr -CA ca.pem -CAkey ca-key.pem \-CAcreateserial -out cert.pem -extfile extfile.cnf
```


13. 删除不需要的文件，两个证书签名请求

```
[root@localhost ca]# rm -v client.csr server.csr
```

14. 修改权限，要保护您的密钥免受意外损坏，请删除其写入权限。要使它们只能被您读取，更改文件模式

```
$ chmod -v 0400 ca-key.pem key.pem server-key.pem
$ chmod -v 0444 ca.pem server-cert.pem cert.pem
```


15. 归集服务器证书

把 `ca.pem`  `server-cert.pem`  `server-key.pem` 三个文件移动到服务端 `/etc/docker/` 文件夹中。

16. 修改Docker配置(使Docker守护程序仅接受来自提供CA信任的证书的客户端的连接()

* 修改docker.service `[root@localhost ca]# vim /lib/systemd/system/docker.service` 修改该文件Service标签下的`ExecStart`

```
[Service]
Type=notify
# the default is not to use systemd for cgroups because the delegate issues still
# exists and systemd currently does not support the cgroup feature set required
# for containers run by docker

#重点
#ExecStart=/usr/bin/dockerd -H fd:// --containerd=/run/containerd/containerd.sock（原来的）
ExecStart=/usr/bin/dockerd（改为这样）
# -H tcp://0.0.0.0:2375 -H unix://var/run/docker.sock （上面行从-H开始换行注释起来）

```

* 修改/etc/docker/daemon.json

```json
{
"registry-mirrors": ["https://26t763ap.mirror.aliyuncs.com"], 
  "tlsverify": true,
  "tlscert": "/etc/docker/server-cert.pem",
  "tlskey": "/etc/docker/server-key.pem",
  "tlscacert": "/etc/docker/ca.pem",
  "hosts":[
    "unix:///var/run/docker.sock",
    "tcp://0.0.0.0:2376"
  ]
}
~            
```

17. 重新加载daemon并重启docker

```shell
[root@localhost ~]# systemctl daemon-reload 
[root@localhost ~]# systemctl restart docker
```

18. 阿里云或本地防火墙要放行2376 端口

19. 通过FTP把客户端需要的 pem文件下载到本地

  将 `ca.pem`  `cert.pem`  `key.pem` 三个文件通过 `scp` 下载到本地

20. IDEA 中配置证书及路径（如下图）

 ![20181018162510941](images/20181018162510941.png)
 
注意: 改为 2376 端口以后，要使用**https**地址,不能再使用 tcp 地址

* 正确的: `https://47.105.199.230:2376`
* 错误的: `tcp://47.105.199.230:2376`





[^参考文章][^参考文章]: https://blog.csdn.net/ChineseYoung/article/details/83107353