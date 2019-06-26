---
title: Nginx配置Https证书后,API项目访问时POST请求变为GET请求
categories: Nginx

tags:
- https
---

Nginx下配置Https证书，之前的博客是: [Nginx配置Https证书](http://nilaile.win/2019/05/13/Centos7.5%E9%85%8D%E7%BD%AEHTTPS%E8%AF%81%E4%B9%A6/)


Https证书配置完成后 admin模块可以正常跳转到HTTPS, API模块部署后,后台请求登录遇到异常,查看Nginx日志后发现从http跳转到https时,请求的api方法从POST变为了GET请求，日志如下:

```
123.52.42.83 - - [05/Jun/2019:15:51:46 +0800] "POST /ylzhy-api/api/wechat/user/login HTTP/1.1" 301 185 "-" "PostmanRuntime/7.13.0"

123.52.42.83 - - [05/Jun/2019:15:51:46 +0800] "GET /ylzhy-api/api/wechat/user/login HTTP/1.1" 200 65 "http://www.sjjtcloud.com:80/ylzhy-api/api/wechat/user/login" "PostmanRuntime/7.13.0"
```

查找相关资料后, 发现是由于 301引起的。换成307问题解决

```
http {
    include       mime.types;
    default_type  application/octet-stream;
    sendfile        on;
    keepalive_timeout  65;

    #gzip  on;   
	server {
		listen 443;
		server_name www.sjjtcloud.com;
		ssl on;
		ssl_certificate  	cert/2195340_www.sjjtcloud.com.pem;
		ssl_certificate_key 	cert/2195340_www.sjjtcloud.com.key;
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
	
		location /api/ {
			proxy_set_header Host $host;
			proxy_set_header X-Real-IP $remote_addr;
			proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
			proxy_pass http://127.0.0.1:8081;
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
		server_name www.sjjtcloud.com sjjtcloud.com;  
		proxy_set_header Host $host;
		proxy_set_header X-Real-IP $remote_addr;
		#rewrite ^(.*)$  https://${server_name}$1 permanent; #这种写法是错误的
		return 307 https://$server_name$request_uri; ###改为307
	}


}
```

301 Moved Permanently
被请求的资源已永久移动到新位置，并且将来任何对此资源的引用都应该使用本响应返回的若干个 URI 之一

307 Temporary Redirect
请求的资源现在临时从不同的URI 响应请求。由于这样的重定向是临时的，客户端应当继续向原有地址发送以后的请求 

