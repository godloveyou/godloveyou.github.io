---
title: Centos7.5使用NGINX配置https证书
categories: LINUX
tags:
- HTTPS
---

###  1. 下载安装NGINX

### 2. 修改配置文件（nginx一般安装在/etc/nginx下,配置文件一般会有两个，一个是/etc/nginx/nginx.conf 另一个是/etc/nginx/config.d/default.conf,nginx.conf中包含了default.conf）,通常我们通过修改default.conf
```
vim /etc/nginx/config.d/defualt.conf

```
```language

#负载均衡配置
upstream webservers_api {
        #weigth
        ip_hash;
        server 127.0.0.1:8086 max_fails=1  fail_timeout=10s;
        #server 127.0.0.1:8081 max_fails=1  fail_timeout=3600s;
}

server {
    listen 443;
    server_name localhost;
    ssl on;
    ssl_certificate  /usr/local/TencentCloud/httpsCert/www.baidu.com.pem;
    ssl_certificate_key /usr/local/TencentCloud/httpsCert/www.baidu.com.key;
    ssl_session_timeout 5m;
    ssl_ciphers ECDHE-RSA-AES128-GCM-SHA256:ECDHE:ECDH:AES:HIGH:!NULL:!aNULL:!MD5:!ADH:!RC4;
    ssl_protocols TLSv1 TLSv1.1 TLSv1.2;
    ssl_prefer_server_ciphers on;
    proxy_set_header Host $host;
    proxy_set_header X-Real-IP $remote_addr;
    access_log /usr/local/web/nginxLogs;

   #静态文件
   location / {
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
        root   /usr/local/web/dist;
        index  index.html index.htm;
        try_files $uri $uri/ @router;
    }

    #下载文件时把下载文件放在该目录下
    location ~* \.(exe|apk|zip)$ {
        root /usr/local/web/download;
    }

    location @router {
            rewrite ^.*$ /index.html last;
    }

   location /hlwyy/api/ {
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_pass http://webservers_api; #跳转到api的负载均衡服务器
   }
}


server {
    listen       80;
    server_name  www.nilaie.com nilaie.com; #多个域名以空格隔开（替换为自己的域名）
    rewrite ^(.*)$  https://$host$1 permanent;
    #charset koi8-r;
    #access_log  /var/log/nginx/host.access.log  main;
    proxy_set_header Host $host;
    proxy_set_header X-Real-IP $remote_addr;
    rewrite ^(.*)$  https://${server_name}$1 permanent;

}



```
