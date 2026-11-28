## 前言 & 为什么要做 SEO

最近因为一些原因（纯 SPA 在 google 和 bing 上基本搜不到 呜呜呜），我终于决定彻底地给我的网站做一次 SEO。我们都知道 Vue 做出来的 SPA 在被其他主机请求后，得到的是类似于这样的东西：

```html
<!DOCTYPE html>
<html lang="">
<head>
    <meta charset="utf-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width,initial-scale=1.0">
    <meta name="referrer" content="no-referrer" />
    
    <meta name="title" content="汇尘轩 - 锦恢的博客">
    <link rel="icon" href="/favicon.ico">
    <title>汇尘轩 - 锦恢的博客</title>
</head>

<body>
    <!-- spa -->
    <div id="app">
        <div style="visibility: hidden;width: 0;height: 0;position: absolute;"><img src="/favicon.png" alt=""></div>
    </div>
</body>

</html>
```

这也是 vue 编译完成后一般的 `index.html` 中的内容。当搜索引擎的爬虫想要爬取你的网站时，它看到的就是这样一坨，它自然无法知道这个页面里面有哪些内容。

如果你去 GET 请求一下其他的网站（知乎，CSDN，B站，塞壬唱片），你会发现，这些网站虽然也是基于 vue 写的，但是被 GET 请求后，返回的结果中存在大量反应当前这个页面的文字信息。搜索引擎就很喜欢这种含有丰富文本信息的请求首页，这样有利于它编制出符合这个网站的搜索结果，比如塞壬唱片的首页 GET 请求结果如下：

<div align=center>
<img src="https://picx.zhimg.com/80/v2-98e71e84d44c6188732879803d671c50_1440w.jpeg" style="width: 60%;"/>
</div>


它在 google 搜索界面的网页如下：

<div align=center>
<img src="https://picx.zhimg.com/80/v2-13549738f76ca17e32e94c66f22d19f4_1440w.jpeg" style="width: 60%;"/>
</div>


不同的网站的 SEO 策略和方式都不一样，因此， SEO 没有一个非常具体的教程和做法，存在的只有一些思路和方向。

比如

- 塞壬唱片是将唱片信息使用 JSONP 装载在 index.html 中的。相当于把第一次获取唱片信息的 Ajax 提前了，这样可以几乎不影响首屏加载速度的情况下还能让搜索引擎知道这个网页内的内容。
- 而 B站 则是基于 vue-meta 的 SSR 模式将网页的概况放进了 meta 里面， google 搜索引擎非常喜欢看网页的 meta，这是一种建模你的网站概况的好方法。
- CSDN 基于 JSON-LD 协议让搜索引擎知道网页的元信息。

这篇文章就简单介绍一下做 SEO 的几个点和上面这三个方法的举例介绍。

---

## 冷知识：现在的搜索引擎爬虫已经能解析出 SPA 网站的内容

事实上，根据我的测试，google 和 bing 的搜索引擎爬虫已经能够解析出 SPA 页面的内容了，比如 bing 的爬虫爬到我的网站的 html 是这样的：

<div align=center>
<img src="https://picx.zhimg.com/80/v2-00cdd8809aa08e953a7d7ac7f09ae232_1440w.png" style="width: 40%;"/>
</div>

但是它却可以正确解析出首页的内容：

<div align=center>
<img src="https://picx.zhimg.com/80/v2-dcb8f9d4316f078f8fd29657614376f9_1440w.png" style="width: 60%;"/>
</div>

google 也可以做到，毕竟无头浏览器先驱 phantomjs 的替代品 puppeteer 就是 google 维护的，他们能做到解析在浏览器环境下才能渲染出的 SPA 本身也不是什么惊讶的事情。

但是你的 SEO 仍然会对网站排名造成影响，因此，了解 SEO 技术仍然很有意义。

---

## M1. 搜索引擎的站长工具

### “毛遂自荐”

首先想到的，自然是使用各个搜索引擎自己的站长工具来主动“毛遂自荐”，将自己的网站主动暴露给这些搜索引擎。Google，百度，Bing 御三家的站长工具链接如下：

- https://search.google.com/search-console
- https://ziyuan.baidu.com/linksubmit
- https://www.bing.com/webmasters/sitemaps

> 百度今年在进行网站扫黄和废弃站点清扫工作，暂时关闭了大部分的网页提交渠道。

这些站长工具站点都提供了不少武器装备，特别是 google 和 bing，黑科技比较多（甚至连我的纯 SPA 网页都能扫描解析）。来给你的网站提供部分的信息，下图是我的网站在 google 和 bing 1这两个站点的统计信息（因为最近才认证了网站，所以点击量非常少，如果可以的话，希望大家可以通过 bing 或者 google 进入我的网站看看）：

<div align=center>
<img src="https://pic1.zhimg.com/80/v2-d2e79c8a5695aad5db76629f53697ae5_1440w.jpeg" style="width: 60%;"/>
</div>


<div align=center>
<img src="https://picx.zhimg.com/80/v2-e164cead1f197926a56ce75db45b27ea_1440w.jpeg" style="width: 60%;"/>
</div>


### 批量提交自己的站点链接——sitemap-webpack-plugin

那么如何批量提交自己的网站链接呢？如果你的网站是静态的，或者是 SSR，那么，只需要提交主页网址，这些搜索引擎爬虫会根据网站上的超链接逐步挖掘你整个网站。

但是如果你像我一样，使用 SPA 构建了 CSR 网站，大部分搜索引擎是无法解析你的网站的，那么，就需要手动提交所有的你的网站上可能的网址+路由。

网站上所有地址的集合被称为网站地图 sitemap，比如 [苹果官网的 sitemap](https://www.apple.com/sitemap.xml) 就是一个例子，它是基于 xml 组织的一个数组，每一个条目记录了一个可以被访问的网站链接。手动编写 sitemap 非常累，一个很好用的 webpack 插件 `npm install -D sitemap-webpack-plugin` 能够帮助你在 `npm run build` 时自动构建 sitemap。下面以 vue3 工程为例，讲解如何使用 sitemap-webpack-plugin。

先安装插件：

```bash
npm install -D sitemap-webpack-plugin
```

然后在 index.html 同级的目录新建一个 `sitemap.json`，里面写上你的网站根网址和所有潜在路由（这一步可以通过 nodejs 或者 python 脚本扫描你的 router 结合后端数据库直接生成）：

```json
{   
    "base": "https://kirigaya.cn",
    "routes": [
        "/",
        "/home",
        "/blog",
        "/blog/article",
        ...
    ]
}
```

在 `vue.config.js` 中：

```js
const SitemapPlugin = require('sitemap-webpack-plugin');
const sitemapJSON = require('./public/sitemap.json');

module.exports = {
    ...
    configureWebpack: config => {
        ...
        config.plugins.push(
            new SitemapPlugin.default({
                base: sitemapJSON.base,
                paths: sitemapJSON.routes,
                options: {
                    filename: 'sitemap.xml',
                    skipgzip: true,
                    changefreq: 'daily',
                }
            })
        )
        ...
    }
    ...
}
```

然后运行 `npm run build` 就会自动生成 `sitemap.xml` 了。

然后通过各个搜索引擎的站长工具提交你的 sitemap 即可。

---

## M2. 服务端渲染 SSR

除了站长工具，将网站改造成静态或者SSR是解决问题的根本途径。所谓 SSR，也就是服务端渲染，与常规的静态页面不同的是，用户可以定义最终 index.html 形态的自由度，哪些页面需要提前渲染，哪些子功能可以使用 CSR。比如对于 知乎文章 而言，文章的主体部分（标题，文字，封面图片链接）是在服务器渲染好的，而一些不太重要的东西（LateX 渲染工具包，CSS，代码高亮包，生成目录的 JS 代码）则是在客户端渲染的，这也就是为什么我们在网络环境较差的环境下打开知乎，会发现虽然文章刷新出来了，但是部分格式存在问题，比如是数学公式的地方都是 LaTeX 源代码。但是这么做对于大部分没有代码和公式的文章来说够用了，而且知乎的受众中，习惯看代码和公式的群体也不是非常多。

最关键的是，只有文本的情况下对于搜索引擎非常友好，而且搜索引擎一看到这么多文本和站内外链接，会提升该站点的权重，让用户在搜索相关知识的时候更容易搜到知乎站点的文章。如果你经常在 google 上搜偏技术和数学的文章，能经常搜到知乎的，这代表知乎的 SEO 做得确实很好。

常用 SPA 框架对应的 SSR 框架如下：

- vue 项目可以使用 nuxt 进行 SSR 改造
- react 项目可以使用 next 进行 SSR 改造

接下来，我们来简单看一下常见网站的 SEO 是怎么做的。

---

### B站 —— vue-meta

我们随便在 google 上搜一下 B站的视频：

<div align=center>
<img src="https://picx.zhimg.com/80/v2-4de74b5f657f5ee53490c037e3174d33_1440w.jpeg" style="width: 60%;"/>
</div>


然后我们复制第一个视频的链接，直接请求看看首次请求的 html，通过比对 google 给出的关于这个网站的信息和 html ，我抽离出了 google 如此渲染 B站搜索结果的依据： 

```html
...
...
  <meta data-vue-meta="true" itemprop="author" name="author" content="兔叭咯">
  <meta data-vue-meta="true" itemprop="name" name="title" content="【硬核科普】一张图片坑害了一代人！| 腰不好怎么睡？_哔哩哔哩_bilibili">
  <meta data-vue-meta="true" itemprop="url" content="https://www.bilibili.com/video/BV1MD421L7oT/">
  <meta data-vue-meta="true" itemprop="image"
    content="//i2.hdslb.com/bfs/archive/45db23ba82419587bd29dd261c21e3a8a3d958b2.jpg@100w_100h_1c.png">
  <meta data-vue-meta="true" itemprop="thumbnailUrl"
    content="//i2.hdslb.com/bfs/archive/45db23ba82419587bd29dd261c21e3a8a3d958b2.jpg@100w_100h_1c.png">
  <meta data-vue-meta="true" itemprop="uploadDate" content="2024-03-25 17:43:44">
  <meta data-vue-meta="true" itemprop="datePublished" content="2024-03-25 17:04:23">
  <meta data-vue-meta="true" property="og:type" content="video">
  <meta data-vue-meta="true" property="og:title" content="【硬核科普】一张图片坑害了一代人！| 腰不好怎么睡？_哔哩哔哩_bilibili">
  <meta data-vue-meta="true" property="og:image"
    content="//i2.hdslb.com/bfs/archive/45db23ba82419587bd29dd261c21e3a8a3d958b2.jpg@100w_100h_1c.png">
...
...
```

从 `data-vue-meta` 中可以看出来 B站 使用了 SSR 渲染，并且使用了 `vue-meta` 根据视频的BV号或者视频链接生成了描述视频的meta信息，google搜索引擎也会根据爬取到的 meta 信息计算出描述这个网页的图片、标题、描述。

`vue-meta` 是一个 vue 内用于修改网页 meta 标签信息的库，但是如果是纯 CSR 模式下， `vue-meta` 只能在 vue 被挂载后发挥作用，但是当爬虫爬到你的 vue 网页的 index.html，和 vue 被挂载差得远了， vue 只有在浏览器环境下才能被挂载。因此，想要达到 B站 这个效果（首次请求就改了 meta），就必须要使用 vue-meta 的 SSR 模式，也就是在 SSR 服务器上使用 `vue-meta`，而不是在客户端使用。具体请移步 `vue-meta` 和 `nuxt` 的官网：https://vue-meta.nuxtjs.org/

---

### index.php —— 一种极简的方法

将原本是 CSR 的项目迁移到 SSR 本就是一件不轻松的事情，更何况你迁移过去后，还需要定义 SSR 模式下网站哪些内容是真的需要在服务端渲染的，如果只是希望修改 meta 信息或者少量信息，使用 php 是一个非常不错的方案。

我最近探索了一种古老而简约的做法。那就是CGI技术，具体说，就是把 index.html 换成 index.php。将原本写死的定义 meta 的 html 替换成 php 表达式，其中 php 可以执行服务器本地的程序（比如用 curl 去访问后端程序）来获取对应路由的一些信息，然后结合这些信息计算出对应的 meta 信息（比如文章标题，文章简介，时间，作者等等），然后将对应的信息整合后生成 index.html 返回给 nginx 或者 apache 反向代理给客户端。

下面是 StackOverflow 上的一个网友给的例子：

```php
<!DOCTYPE html>
<html lang="">

<head>
<?php

 $current_url = $_SERVER['REQUEST_URI'];
$url = explode('/',$current_url);
if($url[1] == 'course'){
    $response = shell_exec("curl yourapi.com/course/find?slug=$url[2]");
    $decoded = json_decode($response ,true);
} 
?>
    <meta charset="utf-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width,initial-scale=1">
    <link rel="icon" href="/favicon.ico">
    <?php 
    if(!$decoded['courseName']){
        $decoded['courseName'] = "default title";
    }
    ?>
    <meta name="title" content='<?php echo $decoded['courseName'] ?>'>
      <meta name="description" content='<?php echo $decoded['snippet']?>'>
    <meta property="og:type" content="website">
      <meta property="og:url" content="http://yoursite.com/<?php echo $current_url?>">
      <meta property="og:title" content="<?php echo $decoded['courseName']?>">
      <meta property="og:description" content="<?php echo $decoded['snippet']?>">
      <meta property="og:image" content="<?php echo $decoded['thumbnail'] ?>">
      <meta property="og:image:width" content="800" />
<meta property="og:image:height" content="400" />
    <meta itemprop="name">
    <meta property="og:title">
    <meta name="twitter:title">
    <meta name="description">
    <meta itemprop="description">
    <meta property="og:description">
    <meta name="twitter:description">
    <link rel="stylesheet" href="/assets/fonts/fontawesome-webfont.woff2">
    <link rel="stylesheet" href="/assets/fonts/Edustdy.whtt">
    <link rel="stylesheet" href="/assets/fonts/fontawesome-webfont.woff">
    <link rel="stylesheet" href="/assets/fonts/fontawesome-webfont3e6e.eot">

    <title>default title</title>

    <link href="/css/course.1d10f9e6.css" rel="prefetch">
    <link href="/js/about.b5c0f3ce.js" rel="prefetch">
    <link href="/js/course.d8d75b7d.js" rel="prefetch">
    <link href="/css/app.203f8281.css" rel="preload" as="style">
    <link href="/css/chunk-vendors.e8860705.css" rel="preload" as="style">
    <link href="/js/app.c39400af.js" rel="preload" as="script">
    <link href="/js/chunk-vendors.4cc0d445.js" rel="preload" as="script">
    <link href="/css/chunk-vendors.e8860705.css" rel="stylesheet">
    <link href="/css/app.203f8281.css" rel="stylesheet">
</head>

<body><noscript><strong>We're sorry but this site doesn't work properly without JavaScript enabled. Please enable it to
            continue.</strong></noscript>
    <div id="app"></div>
    <script src="/js/chunk-vendors.4cc0d445.js"></script>
    <script src="/js/app.c39400af.js"></script>

</body>

</html>
```

这个方法的优点就是完全不需要修改 vue 内的，也不需要迁移到 SSR，只需要手动编写 `index.php` 并结合后端程序计算出对应的 meta 标签。缺点就是效率，与成熟 SSR 框架相比， index.php 每次被请求都会重新-请求后端-生成 index.html，而 SSR 框架大多都是有缓存的，缺失现代 server 优化的 index.php 在应付海量请求时会出现性能上的问题。

---

### JSON-LD 协议

我们随便在 google 上搜一下 CSDN 的文章：

<div align=center>
<img src="https://pic1.zhimg.com/80/v2-a1f061595a9f83d2ddb2d2ebcf61eb47_1440w.jpeg" style="width: 60%;"/>
</div>




上面类似，我们请求第一个网页的链接得到 raw html，通过比对 google 给出的关于这个网站的信息和 html ，抽离出 google 如此渲染 CSDN 搜索结果的依据：

```html
    <script type="application/ld+json">
{"@context":"https://ziyuan.baidu.com/contexts/cambrian.jsonld","@id":"https://blog.csdn.net/jiaohuizhuang6019/article/details/135029171","appid":"1638831770136827","pubDate":"2023-12-16T10:00:20","title":"真正可行的vue3迁移到nuxt3方法（本人亲测，完全避坑）_nuxt3 vuex-CSDN博客","upDate":"2023-12-16T13:07:19"}
    </script>
```

从代码中可以看到，CSDN 使用了 `application/ld+json` 这种 MIME type 进行了脚本挂载。熟悉知识图谱和 web 4.0 的朋友对这玩意儿应该不陌生，它是 JSON-LD (JSON for Linking Data) 格式的脚本标签。在语义网构建中常被使用。

上面的键值，比如 `upDate,pubDate`  都是固定的，可以在 google 搜索引擎指南中找到所有支持：https://developers.google.com/custom-search/docs/structured_data?hl=zh-cn#using-json-ld

更加详细的编写 JSON-LD 可以移步 JSON-LD 官网 https://json-ld.org/ ，他们定义了 JSON-LD 的相关标准与协议。

---

## 总结

关于 SEO 我的总结大概就是这么做了，根据 28 定律，我有必要在结尾说一下哪一个 SEO 方法是 “8”。那就是——在项目开头使用 SSR。

我当年玩 vue 的时候怎么就没人告诉我这些 ┭┮﹏┭┮

<div align=center>
<img src="https://pic1.zhimg.com/80/v2-1900d7ce2df0c4fa36a9c5d26a9c4d6f_1440w.jpeg" style="width: 200px;"/>
</div>