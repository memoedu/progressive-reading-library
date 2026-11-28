最近在自学营养学，顺便把库存的一些人工智能与计算机相关的藏书看了个遍。在看HTTP 1.1优化策略（我怎么知道为啥会无意翻到这种小学生教材）时，无意了解到了Brotli压缩算法，由google工程师开发设计，也是截至2023年大部分主流浏览器都支持且压缩效率最高的，感兴趣的读者可以看这篇文章：https://www.cnblogs.com/-wenli/p/13594882.html，其中开头列出了Brotli的兼容性和各大压缩算法性能对比。

既然如此好用，那我也没理由用着我那古老的gzip，遂展开实践，给我的个人网站配上更好的Brotli算法，简单记录一下我10分钟的探索经历。

由于我采用了vue.js + nginx + go/gin的技术路线，所以我直接记录vue.js和nginx中配置Brotli的方法。

---

## vue配置Brotli

vue的打包压缩算法配置在`vue.config.js`文件中，你首先需要安装`compression-webpack-plugin`，相信做过gzip压缩的朋友应该不陌。然后如下配置压缩：

```javascript
const isProduction = process.env.NODE_ENV !== 'development';
const CompressionWebpackPlugin = require('compression-webpack-plugin')

const compressResourceType = ['html', 'js', 'css'];

module.exports = {
...
...
    configureWebpack: config => {
        if (isProduction) {
            config.plugins.push(
                new CompressionWebpackPlugin({
                    algorithm: 'brotliCompress',
                    compressionOptions: { level: 11 },
                    test: new RegExp(
                        '\\.(' + compressResourceType.join('|') + ')$'
                    ),
                    threshold: 10240, 
                    minRatio: 0.8, 
                    deleteOriginalAssets: false 
                })
            )
        }
    },
...
...
}
```

运行如下指令进行打包：
```bash
$Env:NODE_OPTIONS="--openssl-legacy-provider"
npm run build
```
---

## Nginx配置Brotli

nginx从1.9.11后支持导入动态链接库，所以为了让nginx支持Brotli算法，我们需要为nginx编译Brotli压缩算法的so文件。

先安装需要的库：

```bash
sudo apt install libbrotli-dev
```



### 下载 NginxBrotli 源代码

google将算法的C实现开源了，所以我们先下载一下源代码，source 运行下方脚本

```bash
cd /usr/src
git clone https://github.com/google/ngx_brotli.git && cd ngx_brotli
git submodule update --init
```

### 下载 Nginx安装包用于编译so文件

然后编译，编译嵌入nginx的动态链接库需要nginx安装包提供的编译环境和脚本，下载对应版本的nginx源代码，比如我的是nginx 1.20.1，就如下安装

```bash
wget -c http://nginx.org/download/nginx-1.24.0.tar.gz
tar -xvf nginx-1.24.0.tar.gz
```

### 编译

紧接上文，进行编译：
```bash
cd nginx-1.24.0
./configure --with-compat --add-dynamic-module=/usr/src/ngx_brotli
make modules
```

编译结果会存放在当前目录的objs文件夹下，我们查看其中的so文件：
```bash
ls objs/*.so
```
如果出现了如下的两个文件，说明编译成功：
```
objs/ngx_http_brotli_filter_module.so  objs/ngx_http_brotli_static_module.so
```

### 修改nginx配置文件

打开nginx配置文件：
```bash
vim /etc/nginx/nginx.conf
```

在开头添加如下两句话，把刚刚编译出的两个so文件load进来：
```nginx
load_module <ngx_http_brotli_filter_module.so的绝对路径>;
load_module <ngx_http_brotli_static_module.so的绝对路径>;
```

在http中加入Brotli算法的配置：
```nginx
http {
    ...
    # Brotli 配置
    brotli on;
    brotli_comp_level 6;
    brotli_buffers 16 8k;
    brotli_min_length 20;
    brotli_types text/plain text/css application/json application/x-javascript text/xml application/xml application/xml+rss text/javascript application/javascript image/svg+xml;
    ...
}
```

在https的配置中也重复上述内容。

如果你觉得这样写有点乱，也可以把`load_module`的部分写入一个.conf文件中，然后include进来方便管理。详细可见这篇文章 https://www.cnblogs.com/-wenli/p/13594882.html，但是要注意，`load_module`后的路径最好是绝对路径。

### 重载nginx

```bash
nginx -s reload
````
没有报错说明配置成功。

---

## 测试

重新打开我的个人网站 https://kirigaya.cn，打开控制台网络，查看入口请求的响应信息：

<div align=center>
<img src="https://pica.zhimg.com/80/v2-b57d6bedf0b4c67b46d43c30e8577077_720w.png" style="width: 70%;"/>
<p class="img-caption">成功变更为br压缩算法</p>
</div>