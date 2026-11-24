## 前言

在上一篇文章中，我们介绍了第一种无头浏览器  `phantom`，它可以做到解析模拟浏览器，并解析 CSR 网站。在上一篇文章的末尾，我也提到了其中的一些缺点：

1. 稳定版本无法完整支持 ES6，导致很多现代构建的 CSR 网站都无法解析（比如我的）。
2. 生成的节点存在大量重复节点。

比如对于我的个人网站 https://kirigaya.cn 中的某篇文章：https://kirigaya.cn/blog/article?seq=229 ，它是通过客户端渲染的，事实上，我在我的网站上自己手搓了一个 markdown 渲染器。对于这种稍微复杂一些的例子，phantom 就无法成功渲染出正确的文字。

因为一些历史原因， google 也不再维护 phantom，转而开发了一款新的无头浏览器 puppeteer。它完全基于 nodejs ，完全支持 ES6 特性。我们之前使用的 phantom 是运行在 phantom 自己的运行时内的，如果想要把它嵌入到某个 js 或者别的编程语言的项目中，就只能通过操作系统的消息队列或者通过文件系统来交互数据了。但是 puppeteer 是一个真正的 nodejs 库，它基于 chrome 内核和与之对应的 js 接口层开发。我个人用下来，简直太舒服了！它完美解决了 phantom 的几个严重的痛点。

puppeteer 的官方文档：[https://puppeteer.bootcss.com/api](https://puppeteer.bootcss.com/api)

---



## 安装

puppeteer 的安装相当简单，直接在当前 nodejs 项目中安装即可：

```bash
npm install puppeteer
```

因为很多部署场景下没有浏览器内核，所以 `puppeteer` 的安装还会附带一个 chrome 内核。

如此一来，就安装就完成了。而且因为是一个 nodejs 的库，所以 puppeteer 的使用在 vscode 内是有自动补全的。因此，你在熟悉基本的操作后就不需要去看文档了。自动补全和定义跳转足以解决大部分的问题。



### 先来爬一个简单的页面

静态页面爬取没啥意思，我们来试试一个稍微有点意思的吧！

先来爬取我的网站，如果你正常进行 get 请求我的网站的文章，得到的 html 如下：

```html
<!DOCTYPE html>
<html lang="">

<head>
	...
</head>

<body>
    <div id="app">
        <div style="visibility: hidden;width: 0;height: 0;position: absolute;"><img src="/favicon.png" alt=""></div>
    </div>
    <div id="loading-layer" class="loading-layer">
        <div class="arc"></div>
        <h1 class="loading-text"><span>LOADING</span></h1>
    </div>
    <script src="/js/npm.webpack.8627a089.js"></script>
    <script src="/js/npm.axios.c640c9b8.js"></script>
    <script src="/js/npm.vue.41b79098.js"></script>
    <script src="/js/npm.live2d-render.ff740f97.js"></script>
    <script src="/js/npm.node-libs-browser.fa54cd1f.js"></script>
    <script src="/js/npm.path-browserify.e386d969.js"></script>
    <script src="/js/npm.vue-loader-v16.e8922b13.js"></script>
    <script src="/js/npm.vue-router.5b89fa1e.js"></script>
    <script src="/js/npm.vuex.1891bd14.js"></script>
    <script src="/js/app.42de4938.js"></script>
</body>

</html>
```

如果你用 phantom 也差不多，因为我的网站不仅是 CSR 渲染，而且为了丝滑的载入，我在 vue 外层加了一个自定义的脚本载入和加载代码。但是使用 puppeteer 就能很好地爬取我的网站：

```js
const puppeteer = require('puppeteer');

(async () => {
    const browser = await puppeteer.launch();
    const page = await browser.newPage();
    page.setViewport({width: 1920, height: 1080});

    // networkidle0 代表所有网络请求都完成了
    await page.goto('https://kirigaya.cn/blog/article?seq=229', { waitUntil: 'networkidle0' });
    
    const category = await page.$('#category-0');
    const title = await page.evaluate(el => {
        if (el) {
            return el.innerHTML;   
        }
    }, category);
    
    console.log(title);

    await page.screenshot({ path: 'kirigaya.png' });
    await browser.close();
})();
```

运行后，输出：

```
无头浏览器（一）phantomjs 的安装与使用
```

生成的截图：

<div align=center>
<img src="https://picx.zhimg.com/80/v2-98419b8236c7bd3de720b8a7420c2c13_1440w.png" style="max-width: 60%;"/>
</div>

可以看到，效果相当完美！



### 爬取知乎上的图片

我们都知道知乎的 SSR 做得挺不错的，因此，知乎的爬取一直不是一件简单的事情，但是有了 puppeteer 后，这一切就很简单了，我们下面爬取一篇我之前写的文章的前三张图片：

```js
const fs = require('fs');
const fspath = require('path');

const puppeteer = require('puppeteer');
const axios = require('axios');

(async () => {
    const browser = await puppeteer.launch();
    const page = await browser.newPage();
    page.setViewport({width: 1920, height: 1080});

    await page.goto('https://zhuanlan.zhihu.com/p/699229249', { waitUntil: 'networkidle0' });

    // 关闭提醒登录的按钮
    const closeButton = await page.$('.Modal-closeButton');
    await closeButton.click();

    const images = await page.$$('img');
    let count = 0;
    for (const img of images) {
        const src = await img.evaluate(el => el.src);
        if (!src || !src.startsWith('http')) {
            continue;
        }

        const filename = fspath.basename(src).split('?')[0];
        const response = await axios({method: 'get', url: src, responseType: 'stream'});
        const writer = fs.createWriteStream(filename);
        response.data.pipe(writer);
        
        if (count ++ > 2) {
            break;
        }
    }

    await browser.close();
})();
```

<div align=center>
<img src="https://pic2.zhimg.com/v2-db67db58065276be39700373af41df0d_r.jpg" style="max-width: 60%;"/>
</div>



效果不错哟！



### 爬取B站视频的截图并停留在 02:30

```js
const puppeteer = require('puppeteer');

(async () => {
    const browser = await puppeteer.launch();
    const page = await browser.newPage();
    page.setViewport({width: 1920, height: 1080});

    const seconds = 2 * 60 + 3;
    await page.goto('https://www.bilibili.com/video/BV1R1421i7dw/?share_source=copy_web&vd_source=5c7883370b8ac639e64c6b403ee2f3aa&t=' + seconds, { waitUntil: 'networkidle0' });

    await page.screenshot({ path: 'bilibili.png' });
    await browser.close();
})();
```

效果

<div align=center>
<img src="https://picx.zhimg.com/80/v2-ffda4753ebc359cdb5c9a6ba44c20cd6_1440w.png" style="max-width: 60%;"/>
</div>



写到这里，相信很多读者应该理解了为什么很多网站的登录需要验证码和一些奇怪的验证方式了，因为单纯的填入账号和密码的方式在 puppeteer 的加持下变得太容易基于代码进行模拟登录了，想要破解一些平台的验证，需要一些计算机视觉的技术，篇幅有限，我暂时不在这篇文章写，如果各位感兴趣，欢迎在评论区留言。如果这篇反响不错，我会考虑做一期用 CV 做爬虫破解登录验证的文章。