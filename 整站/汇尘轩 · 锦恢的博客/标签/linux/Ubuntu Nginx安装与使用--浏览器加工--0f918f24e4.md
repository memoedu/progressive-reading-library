## 背景

最近需要让一个私人服务器上的服务器可被外部访问，考虑到后续可能要改装成一个转发站点，干脆装个反向代理算了。于是有了这篇文章。

---

## 安装

首先进入[nginx官网](http://nginx.org/en/download.html)下载stable的源码，此处演示 `1.24.0` 这个版本号：

```bash
$ mkdir -p ~/download && cd ~/download
$ wget http://nginx.org/download/nginx-1.24.0.tar.gz
```

安装基本的编译环境：
```bash
$ sudo apt install build-essential libpcre3-dev libssl-dev
```

解压，配置，编译，安装：

```bash
$ tar -xvf nginx-1.24.0.tar.gz && cd nginx-1.24.0
$ sudo ./configure --prefix=/usr/local/nginx --with-http_stub_status_module --with-http_ssl_module --with-compat
$ sudo make
$ sudo make install
```

添加软链接：

```bash
$ sudo ln -s /usr/local/nginx/sbin/nginx /usr/local/sbin/nginx
```

重启终端，初始化nginx：

```bash
$ sudo nginx
```

然后再浏览器中输入你的服务器的 IP 或者绑定的域名，出现如下图样说明 nginx 服务启动成功：

<div align=center>
<img src="https://pic1.zhimg.com/80/v2-baff24e967bb6805ba23ac1d3a4f7182_1440w.png" style="width: 60%;"/>
</div>

> 需要注意的是，不同于Centos，Nginx位于Ubuntu上的安装位置是需要sudo权限才能进行操作的。

---

## Nginx 的配置文件目录

一般情况下， nginx 的配置目录要么是 `/etc/nginx` ，要么是 `/usr/local/nginx/conf`。

如果不确定，可以通过如下命令找到配置文件：

```bash
$ sudo nginx -t
```
out:

```
nginx: the configuration file /usr/local/nginx/conf/nginx.conf syntax is ok
nginx: configuration file /usr/local/nginx/conf/nginx.conf test is successful
```

---

## Nginx 基本指令和错误解决

重启（用得最多，用来刷新配置文件的）：

```bash
$ sudo nginx -s reload
```

关闭（重装 nginx 要关闭）：

```bash
$ sudo nginx -s stop
```

重装后首次启动：

```bash
$ sudo nginx -c <nginx.conf 绝对路径>
```



---

##  Nginx 常用配置命令

下面的都默认写在 http 的 scope 里面。即：

```nginx
http {
    include       mime.types;
    default_type  application/octet-stream;
	# 下面代码的容身之地
}
```

### 请求头 & 代理配置

具体说明懒得写了，总之，你不配置请求头可能会让别人的请求发生请求于错误；不配置代理会使得本地的服务器程序可能无法获取目标的真实 IP。

- 请求头配置： `add_header <key> <value>;`
- 代理配置：`proxy_set_header <key> <value>;`

如果你用 nginx 开了一个静态存储仓库，最好加入如下几句话，这里以  images 举例，在 http 或者 https 里面，写入：

```nginx
server {
    listen 443 ssl;
    ...
    ...

    location /images/ {
        add_header 'Access-Control-Allow-Origin' '*';
        add_header 'Access-Control-Allow-Methods' 'POST,GET,OPTIONS';
        add_header 'Access-Control-Allow-Headers' 'Authorization';
        proxy_set_header X-Forwarded-Proto  $scheme;
        proxy_set_header X-Forward-For $remote_addr;
        proxy_set_header X-real-ip $remote_addr;
        root /home/;
        autoindex on;
    }
}
```

### HTTP 配置

如果希望开网站或者起 http服务器，这个必须配置：

```nginx
server {
    listen       80;
    listen       [::]:80;
    server_name  kirigaya.cn;
    root         <index.html所在目录的绝对路径>;
    charset utf-8;
    # 重定向到https（没有https需求请去掉这句话）
    rewrite ^(.*)$ https://kirigaya.cn;        
	
    # 默认请求的去处（一般是网站首页）
    location  {
        add_header 'Access-Control-Allow-Origin' '*';
        add_header 'Access-Control-Allow-Methods' 'POST,GET,OPTIONS';
        add_header 'Access-Control-Allow-Headers' 'Authorization';
        proxy_set_header X-Forwarded-Proto  $scheme;
        proxy_set_header X-Forward-For $remote_addr;
        proxy_set_header X-real-ip $remote_addr;
        root <index.html所在目录的绝对路径>;
        index index.html;
        try_files $uri $uri/ /index.html 404;
    }
}
```

### HTTPS 配置

只配置http会显得很丑，因为会出现不安全，如果有人进行请求拦截往你的网站加小广告或者破黑水，加上你是个网络安全小白，那就跳进黄河也洗不清了。所以配一个 HTTPS 吧少年，SSL证书反正也是免费的。

首先你的云服务器平台下载 SSL 证书并绑定到你现在的域名上，请保证你的当前域名DNS可解析（ping一下就知道了），然后只需要下载带 `nginx` 字眼的压缩包，然后解压后里面应该是有四个文件（里面会有一个 `.pem` 文件和一个 `.key` 文件），把它们 scp 到服务器上。然后写入如下配置：

```nginx
server {
    listen 443 ssl;
    server_name kirigaya.cn;
    root <index.html所在目录的绝对路径>;
    index index.html index.htm;
    ssl_certificate     <xxx.pem 文件绝对路径>;
    ssl_certificate_key <xxx.key 文件绝对路径>;
    ssl_session_timeout 5m;
    ssl_ciphers ECDHE-RSA-AES128-GCM-SHA256:ECDHE:ECDH:AES:HIGH:!NULL:!aNULL:!MD5:!ADH:!RC4;
    ssl_protocols TLSv1 TLSv1.1 TLSv1.2;
    ssl_prefer_server_ciphers on;
}
```

### gzip 压缩

这个会让你的网站访问速度更快，但是会稍微稍微增加一点点服务端和客户端的计算复杂。

```nginx
gzip on;

# 启用 gzip 压缩的最小文件，小于设置值的文件将不会压缩
gzip_min_length 1k;

# gzip 压缩级别，1-9，数字越大压缩的越好，也越占用CPU时间
gzip_comp_level 3;

# 进行压缩的文件类型。javascript有多种形式，如果压缩超大文件浪费了你的服务器资源，可以考虑去除它们
gzip_types text/plain application/javascript application/x-javascript text/css application/xml text/javascript application/x-httpd-php image/jpeg image/gif image/png video/mp4;

# 是否在http header中添加Vary: Accept-Encoding，建议开启
gzip_vary on;

# 设置压缩所需要的缓冲区大小
gzip_buffers 4 16k;
```

### brotli 压缩

这个和 `gzip` 二选一， `brotli` 由 google 的小混蛋发明，性能比 `gzip` 更加优秀，可以参考我之前的博客：

### 有必要的杂项——超时、请求头大小

有的时候，对于请求参数很长的请求，可能会爆 header 的错，然后你还以为自己的 java 或者 go（抱歉，我不做 SSR）写错了，为了避免这种情况的发生，写入如下配置：

```nginx
# 定义 MIME 类型的哈希表最大为 4096
types_hash_max_size 4096;

# 客户端请求头部的超时时间为 600 秒
client_header_timeout 600;

# 客户端请求体的超时时间为 600 秒
client_body_timeout 600;

# 客户端请求体最大为 110 兆字节
client_max_body_size 110m;

# 反向代理请求头哈希表的最大尺寸为 51200
proxy_headers_hash_max_size 51200;

# 反向代理请求头哈希表的桶尺寸为 6400
proxy_headers_hash_bucket_size 6400;
```