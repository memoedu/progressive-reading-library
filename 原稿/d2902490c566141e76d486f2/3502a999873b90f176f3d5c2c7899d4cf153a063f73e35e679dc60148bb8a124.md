安装很简单，自然不必多说。

centos上nginx默认的配置文件在`/etc/nginx/nginx.conf`上。

## 基本指令

```bash
nginx 			 # 启动
nginx -s reload # 重启（最常用的）
nginx -s stop   # 停止
```

## 注意点

真实部署时，需要注意的技术点：

- 配置前端的跨域代理，使用`location`
- 配置HTTPS和SSL证书
- `gzip`压缩（这项服务必须前端部署代码也配置了才行）
- 客户端的请求超时设置`client_header_timeout`, `client_body_timeout`（真实TCP请求的最大等待时间是前端`ajax`配置的timeout、nginx的timeout与后端的timeout取最小，请注意）
- 客户端请求报文的body大小`client_max_body_size`（如果前端要传递大文件，这个参数一定要设大）

---

## Demo配置

一份可行的配置如下：

```
user nginx;
worker_processes auto;
error_log /var/log/nginx/error.log;
pid /run/nginx.pid;

# Load dynamic modules. See /usr/share/doc/nginx/README.dynamic.
include /usr/share/nginx/modules/*.conf;

events {
    worker_connections 1024;
}

http {
    log_format  main  '$remote_addr - $remote_user [$time_local] "$request" '
                      '$status $body_bytes_sent "$http_referer" '
                      '"$http_user_agent" "$http_x_forwarded_for"';

    access_log  /var/log/nginx/access.log  main;

    sendfile            on;
    tcp_nopush          on;
    tcp_nodelay         on;
    keepalive_timeout   60;
    types_hash_max_size 4096;
    client_header_timeout 600;
    client_body_timeout 600;
    client_max_body_size 110m;

    include             /etc/nginx/mime.types;
    default_type        application/octet-stream;

    
    # 开启gzip
    gzip on;

    # 启用gzip压缩的最小文件，小于设置值的文件将不会压缩
    gzip_min_length 1k;

    # gzip 压缩级别，1-9，数字越大压缩的越好，也越占用CPU时间，后面会有详细说明
    gzip_comp_level 2;

    # 进行压缩的文件类型。javascript有多种形式，后面的图片压缩不需要的可以自行删除
    gzip_types text/plain application/javascript application/x-javascript text/css application/xml text/javascript application/x-httpd-php image/jpeg image/gif image/png;

    # 是否在http header中添加Vary: Accept-Encoding，建议开启
    gzip_vary on;

    # 设置压缩所需要的缓冲区大小     
    gzip_buffers 4 16k;

    gzip_static on;    

    # 引入外部配置
    include /etc/nginx/conf.d/*.conf;
    include /etc/nginx/sites-enabled/*;

    server {
        listen       80;
        listen       [::]:80;
        server_name  锦恢.studio;
        root         /usr/share/nginx/html;
  	    charset utf-8;	
        # 重定向到https
	    rewrite ^(.*)$ https://锦恢.studio;       


        # Load configuration files for the default server block.
        include /etc/nginx/default.d/*.conf;
	
        location / {
            add_header 'Access-Control-Allow-Origin' '*';
            add_header 'Access-Control-Allow-Methods' 'POST,GET,OPTIONS';
            add_header 'Access-Control-Allow-Headers' 'Authorization';
            root   html;
            index  index.html index.htm;
            try_files $uri $uri/ /index.html;
        }


        location /api {
	    proxy_set_header Host $host;
	    proxy_set_header X-Real-Ip $remote_addr;
	    proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
	    proxy_set_header X-Forworded-For $http_x_forwarded_for;

            add_header 'Access-Control-Allow-Origin' '*';
            add_header 'Access-Control-Allow-Methods' 'POST,GET,OPTIONS';
            add_header 'Access-Control-Allow-Headers' 'Authorization';
            proxy_pass  http://localhost:8001;
        }
        
        error_page 403 /403.html;
        location = /403.html {
        }

        error_page 404 /404.html;
        location = /404.html {
        }
	
	    error_page 500 /500.html;
        error_page 502 503 504 /50x.html;
    }
}
```

> 如果路径中有中文，那么`charset utf-8;`一定不能少

---

## 配置SSL

现在是`2022.5.25`，目前我尝试的SSL证书配置方案仍然生效。相对于上面的配置文件，下面的文件在`/etc/nginx/sites-enabled/`中，文件名随便起。



```
server {
    listen 443 ssl;
    server_name 锦恢.studio;
    root /usr/share/nginx/html;
    index index.html index.htm;
    ssl_certificate  cert/锦恢.studio_bundle.pem;
    ssl_certificate_key cert/锦恢.studio.key;
    ssl_session_timeout 5m;
    ssl_ciphers ECDHE-RSA-AES128-GCM-SHA256:ECDHE:ECDH:AES:HIGH:!NULL:!aNULL:!MD5:!ADH:!RC4;
    ssl_protocols TLSv1 TLSv1.1 TLSv1.2;
    ssl_prefer_server_ciphers on;

    location / {
        add_header 'Access-Control-Allow-Origin' '*';
        add_header 'Access-Control-Allow-Methods' 'POST,GET,OPTIONS';
        add_header 'Access-Control-Allow-Headers' 'Authorization';
        proxy_set_header X-Forwarded-Proto  $scheme;
	root   html;
        index  index.html index.htm;
        try_files $uri $uri/ /index.html;
    }


    location /api {
        add_header 'Access-Control-Allow-Origin' '*';
        add_header 'Access-Control-Allow-Methods' 'POST,GET,OPTIONS';
        add_header 'Access-Control-Allow-Headers' 'Authorization';
        proxy_pass  http://localhost:8001;
    }


    error_page 403 /403.html;
    error_page 404 /404.html;
    error_page 500 /500.html;
}
```