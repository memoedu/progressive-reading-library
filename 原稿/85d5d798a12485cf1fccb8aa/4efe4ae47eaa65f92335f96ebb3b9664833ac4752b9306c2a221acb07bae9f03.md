## 缓存优化

最近发现我的网站并没有使用缓存，于是简单研究了一下如何进行缓存。最终发现需要在 nginx 上进行配置。

先打开我的网站，随便找一个请求看它的响应头信息。可以看到，一般情况下，请求响应头的 `Cache-Control` 字段是 no-cache。

<div align=center>
<img src="https://picx.zhimg.com/80/v2-9d2a6d9f908350d719cbe2704a6f6885_1440w.jpeg" style="width: 70%;"/>
</div>

Cache-Control 字段是一个用于控制缓存机制的字段，且它只有在响应头被设置才有意义（所以你看到的所有网站的资源请求头的 Cache-Control 都是 no-cache，在请求头设置 Cache-Control 是一件没有意义的事情）。为了让网站可以使用缓存进行优化，我们可以通过配置 nginx 来达到这个效果。

> 在一些比较老的网站上，还会见到 Expires 这个字段，这个响应头是 Cache-Control 的前身，但是因为 Expires 是服务端生成的，服务器和客户端的时间可能不一致，所以从 HTTP1.1 时代后（大部分浏览器都是用的 HTTP1.1）， Expires 现在已经不怎用了。

打开我们的 nginx 的配置文件，默认在 `sudo nginx -t` 显示的位置上，然后加入下面这段代码：

```nginx
http {
    ...
        
	# 设置缓存
    map $request_uri $cache_control {
        default "no-store, no-cache, must-revalidate, proxy-revalidate";
        ~*\.html$ "no-store, no-cache, must-revalidate, proxy-revalidate";
        ~*\.(css|js|png|jpg|jpeg|webp|gif|gz|svg|mp3|wav|pdf|mp4|ogg|ogv|webm|htc|xml|ttf|woff|woff2)$ "public,max-age=31536000";
    }
    
    ...
}
```

解释一下，上面的配置中，以 `.html` 结尾的请求不会走缓存（只有 html 不走缓存的原因请看这篇文章：https://zhuanlan.zhihu.com/p/686365892），常规的静态文件（js，css，多媒体等）都使用时间为 `31536000`秒的缓存，只要浏览器请求过这个资源一次，那么在接下来的 `31536000` 秒内再次发起这个请求就不会去向服务器去请求了，而是会从缓存中抓取。

>  需要注意的是，这个抓取缓存的现象在 F12 控制台中是无法察觉的

接下来，我们把 nginx 计算得到的缓存参数赋值给你想要缓存的匹配规则：

```nginx
http{
    ...
        
    server {
        listen       443 ssl;
        server_name  kirigaya.cn;
 
        location / {
            access_log off;
            add_header Cache-Control $cache_control;

            ...
        }
    }
    
    ...
}
```

这样，`https://kirigaya.cn/` 开头的所有请求都会走缓存。如果你不希望 `https://kirigaya.cn/api/` 下的路由走缓存，那么也很简单，只需要在 `location /api` 内部不配置 `add_header Cache-Control $cache_control;` 这句就行了。

配置完成后，我们重启 `nginx`：

```bash
sudo nginx -s reload
```

此时再打开网站查看可以发现响应头的 `Cache-Control` 已经设置好了，那么接下来全部交给浏览器和 http3.0 就行了。

<div align=center>
<img src="https://pica.zhimg.com/80/v2-54f216914d7b8c79783d692e02935427_1440w.jpeg" style="width: 70%;"/>
</div>

体验上，缓存优化后，我的网站的刷新时间（第二次访问的时间）从之前的 3.2 秒，降低到了 0.6 秒，效果显著。

---

## 反向代理

nginx 最有用的一个部分就是进行代理了。代理的方式也非常简单，我个人分为代理路由和代理端口两种模式。

### 代理路由

假设我们现在希望基于 nginx 做一个文件系统，通过 `https://域名/files/` 访问服务器上 `/home/ubuntu/files` 下的文件，如何通过 nginx 实现呢？

其实非常简单，使用 `alias` 这个关键词就可以了，我们找到配置 http 中 443 端口的 server，写下如下代码：

```nginx
location /files {
    autoindex on;
    alias /home/ubuntu/files;
}
```

这样，当我们访问 `https://域名/files/hello.jpg` 时，实际上访问的就是服务器上的 `/home/ubuntu/files/hello.jpg` 上的文件。`autoindex` 的开启还能直接访问文件夹，从而看到文件夹内部的文件树。

### 代理端口

如果我们在服务器上开启了一个 7070 端口的服务，但是不希望通过 `https://域名:7070`，因为这样太丑了或者别的什么原因，而是想要通过 `https://域名/tools` 来访问，要如何配置 nginx 呢？其实也非常简单，只需要配置 https 到 7070 端口的响应准则就行了（https默认是443端口，这本质上就是一个端口映射），打开配置文件，在 443 端口的配置内部写下：

```nginx
location /tools {
	...
        
    proxy_pass https://域名:7070;
}
```

这样，当我们访问 `https://域名/tools` 时，就是在访问 `https://域名:7070`。

