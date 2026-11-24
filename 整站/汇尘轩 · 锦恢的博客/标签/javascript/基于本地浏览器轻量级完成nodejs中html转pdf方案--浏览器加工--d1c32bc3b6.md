## 摘要
我知道搜到这篇文章的朋友大多都是在寻找适合自己的nodejs下html转pdf方案，为了节省时间，我就简单说一下最终实现的效果。

这个方案只需要下载puppeteer-core这个库，大小只有3MB左右，但是用户在使用前需要先填入edge浏览器或者chrome浏览器的启动路径，比如edge的默认路径：C:/Program Files (x86)/Microsoft/Edge/Application/msedge.exe

> 很多项目中为了“让作品的完成度更高”，会把一个阉割版的chrome内核包裹进软件中，从而让用户不需要再填入edge或者chrome浏览器启动路径，比如Typora，vscode插件markdown-pdf。

> 诚然，这么做能够达到更好的用户体验，但是我比较喜欢轻的东西。
因为我的插件中的文档化导出用了本文的方法，所以我以正在开发的vscode插件为例，看一下最终的效果。

图一的右侧是HTML在vscode的webview中的渲染效果：

<div align=center>
<img src="https://pic4.zhimg.com/v2-72244d99ebf4d6d18d077e7a299ba103_r.jpg" style="width: 90%;"/>
</div>


HDL代码文档化
插件导出的pdf如下：

<div align=center>
<img src="https://pic4.zhimg.com/v2-ecaca419b5204b9caa986604de23c7a7_r.jpg" style="width: 60%;"/>
</div>

<div align=center>
<img src="https://pic2.zhimg.com/v2-41a738cf0c341d1b52b3d641e45a1835_r.jpg" style="width: 60%;"/>
</div>



可以看到效果非常好，简直就是一模一样。我的花里胡哨样式和svg都成功导出了。

题记
这两天，手感非常好，我想，可能是我和我的键盘建立起了不错的“友谊”。言归正传，自动文档化的开发已经进入尾声，只剩一个千年老难题——导出pdf尚未解决了，这个问题我在开发我的个人网站的在线markdown编辑器时就遇到了，后来通过DOM的打印pdf接口优雅解决了，现在在vscode中可没有DOM或者常规的浏览器，那么如何导出pdf呢？

我在google用中文搜了半天，无非都是markdown-pdf，html-pdf这些库，这些库都无法正常下载，因为它们其实都是基于phantomjs沙盒实现的，说白了，他们都是基于一个模拟的沙盒浏览器实现的pdf打印，而phantomjs的下载是和平台相关的，作为跨平台的vscode插件，这些库都是无法使用的。而且他们在npm install时，都会从github上去下载phantomjs，所以非常慢。这些所有的方法都需要下载一个你只会用到一小部分功能的库，太不优雅了，很难在开发完后有一个好的心情。

既然这群货都是基于浏览器内核进行pdf导出的，那么为什么我们不用自己的浏览器呢？我想大部分人的PC上总归是有edge或者chrome，倘若没有，我只能说，我们可能没有缘分吧。。。

下面的代码就是从我的插件系统中剥离出来的一部分逻辑，可以单独食用。

---

## Step1. 安装puppeteer-core
首先，我们需要安装puppeteer-core，这是一个用于浏览器自动化的工具，任何支持DevTool的玩意儿，你都可以在这个库中找到对应的操作的API。直白来说，基于puppeteer-core，我们可以在nodejs运行时中直接操作浏览器，而且还不会占用系统渲染浏览器本身的资源。

不同于之前那些混球库，puppeteer-core直接通过npm安装即可，很快就会下好。

```bash
npm install puppeteer-core
```

---

## Step2. 找到浏览器启动路径
在windows11中，自带edge，路径为`C:/Program Files (x86)/Microsoft/Edge/Application/msedge.exe`

你要是不知道，就在找到浏览器的快捷方式后->右击属性->找到“目标”这个属性，它的值就是浏览器真实的路径：


<div align=center>
<img src="https://pic4.zhimg.com/v2-36b76fedf1d39e7f6bd3d1ddc06fef1b_r.jpg" style="width: 85%;"/>
</div>

---

## Step3. 代码
先简单写个待转换的HTML，命名为test.html：

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
</head>
<body>
    <h1>Title 1</h1>
    <p>hello world</p>
    <p>Let's talk about something interesting</p>
    <p>你好，世界</p>
    <p>让我们找点有趣的谈资吧</p>
</body>
<style>
</style>
</html>
```

相同目录下，我们的main.js如下：

```js
const fs = require('fs');
const path = require('path');
const puppeteer = require('puppeteer-core');

const browserPath = "C:/Program Files (x86)/Microsoft/Edge/Application/msedge.exe";

/**
 * @param {string} htmlPath 
 * @param {string} pdfPath 
 */
async function html2pdf(htmlPath, pdfPath) {    
    const browser = await puppeteer.launch({
        executablePath: browserPath,
        args: ['--lang=en-US', '--no-sandbox', '--disable-setuid-sandbox']
    });
    
    const page = await browser.newPage();
    const uriFilePath = 'file:///' + path.resolve(htmlPath).replace(/\\/g, "/");
    await page.goto(uriFilePath, { waitUntil: 'networkidle0' });

    const options = {
        path: pdfPath,
        scale: 1,
        displayHeaderFooter: false,
        headerTemplate: "<div style=\"font-size: 9px; margin-left: 1cm;\"> <span class='title'></span></div> <div style=\"font-size: 9px; margin-left: auto; margin-right: 1cm; \"> <span class='date'></span></div>",
        footerTemplate: "<div style=\"font-size: 9px; margin: 0 auto;\"> <span class='pageNumber'></span> / <span class='totalPages'></span></div>",
        printBackground: true,
        landscape: false,
        format: 'A4',
        margin: {
            top: '1.5cm',
            right: '1cm',
            bottom: '1cm',
            left: '1cm'
        }
    };
    await page.pdf(options);
    await browser.close();
}


const html = fs.readFileSync('test.html', 'utf-8');
html2pdf('test.html', 'test.pdf');
```

其中browserPath为你的浏览器路径，请保证有效。

运行上述文件，我们可以得到test.pdf，打开它：

<div align=center>
<img src="https://pic2.zhimg.com/v2-95ed7c57b4ba3a14c16b174e7cdd3c55_r.jpg" style="width: 70%;"/>
</div>


可以看到，有着非常不错的渲染效果。

结语
我一直很好奇，为啥这么多年，一直没有好用的轻量级pdf渲染引擎，我想，大抵是目前已经够用了吧。。。

也可能是笔者又吃了信息不对称的亏，毕竟我的主笔不在nodejs上。