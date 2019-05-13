---
title: windows基于NGINX配置HTTPS证书
categories: windows
tags:
- NGINX
---
1. 下载解压nginx的windows版本 （笔者使用的版本是nginx-1.12.2版)[官方下载链接](http://nginx.org/en/download.html)具体

2. 从阿里云获取那个免费的HTTPS证书

3. Nginx的config路径下创建cert文件夹，将证书解压到该目录下，证书一般是两个文件，分别是******.key和 ***.pem

4. 修改配置文件如下
```xml
	server {
		listen 443;
		server_name localhost;
		ssl on;
		ssl_certificate  	cert/2195340_www.baidu.com.pem;    #替换为自己的证书文件
		ssl_certificate_key 	cert/2195340_www.baidu.com.key;#替换为自己的证书文件
		ssl_session_timeout 5m;
		ssl_ciphers ECDHE-RSA-AES128-GCM-SHA256:ECDHE:ECDH:AES:HIGH:!NULL:!aNULL:!MD5:!ADH:!RC4;
		ssl_protocols TLSv1 TLSv1.1 TLSv1.2;
		ssl_prefer_server_ciphers on;

		proxy_set_header Host $host;
		proxy_set_header X-Real-IP $remote_addr;

	   location / {
			proxy_set_header Host $host;
			proxy_set_header X-Real-IP $remote_addr;
			root   D:/nginx-1.12.2/html;
			index  index.html index.htm;
			try_files $uri $uri/ @router;
		}
		

	   location /admin {
			proxy_set_header Host $host;
			proxy_set_header X-Real-IP $remote_addr;
			proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
			proxy_pass http://127.0.0.1:8083/admin;

	   }

	}
	
	 server {
    	listen   80;
		server_name www.nilaile.com  nilaile.com;   #这里改为自己的域名,多个域名空格分开
		proxy_set_header Host $host;
		proxy_set_header X-Real-IP $remote_addr;
		rewrite ^(.*)$  https://${server_name}$1 permanent; 
		
	}

```
