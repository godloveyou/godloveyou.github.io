---
title: nginx配置
categories: nginx
tags: nginx
---

windows下nginx配置文件
```

#user  nobody;
worker_processes  1;

#error_log  logs/error.log;
#error_log  logs/error.log  notice;
#error_log  logs/error.log  info;

#pid        logs/nginx.pid;


events {
    worker_connections  1024;
}


http {
    include       mime.types;
    default_type  application/octet-stream;

    #log_format  main  '$remote_addr - $remote_user [$time_local] "$request" '
    #                  '$status $body_bytes_sent "$http_referer" '
    #                  '"$http_user_agent" "$http_x_forwarded_for"';

    #access_log  logs/access.log  main;

    sendfile        on;
    #tcp_nopush     on;

    #keepalive_timeout  0;
    keepalive_timeout  65;

    #gzip  on;

    
	server {
		listen 443;
		server_name www.sjjtcloud.com;
		client_max_body_size 15M;
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
	

	
		location /ylzhy-api/api/ {
			proxy_set_header Host $host;
			proxy_set_header X-Real-IP $remote_addr;
			proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
			proxy_pass http://127.0.0.1:8081;
			

		}
		
		location /yw-api/api/ {
			proxy_set_header Host $host;
			proxy_set_header X-Real-IP $remote_addr;
			proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
			proxy_pass http://127.0.0.1:8089;
			

		}
		
		
		location /admin {
			proxy_set_header Host $host;
			proxy_set_header X-Real-IP $remote_addr;
			proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
			proxy_pass http://127.0.0.1:8090/yw-admin;
		}
	
	}

	
	 server {
    	listen   80;
		server_name www.sjjtcloud.com sjjtcloud.com;  
		proxy_set_header Host $host;
		proxy_set_header X-Real-IP $remote_addr;
		#rewrite ^(.*)$  https://${server_name}$1 permanent; 
		#rewrite ^(.*)$  https://$host$1 permanent;
		return 307 https://$server_name$request_uri;
	}
	
	
	server {
        listen       8082;
        server_name  fileServer;
        charset utf-8;
        location ~ .*\.(gif|jpg|jpeg|png|mp4|pdf)$ {
            expires 24h;
            root D://fileweb/;
            proxy_store on;
            proxy_store_access user:rw group:rw all:rw;
            proxy_temp_path         d://fileweb/;#图片访问路径
            proxy_redirect          off;
            proxy_set_header        Host $host:8082;
            client_max_body_size    10m;
            client_body_buffer_size 1280k;
            proxy_connect_timeout   900;
            proxy_send_timeout      900;
            proxy_read_timeout      900;
            proxy_buffer_size       40k;
            proxy_buffers           40 320k;
            proxy_busy_buffers_size 640k;
            proxy_temp_file_write_size 640k;
        }
    }


}

```
