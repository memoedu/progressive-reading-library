## 前言

最近开始开发一个CSR网络聚合器，用于自动搜集服务器上所有大型数据集的元信息，方便后续建模。

为了对于一些 CSR 网站进行解析并爬取其中的文本内容，我们需要一个无头浏览器。于是我想要花两篇文章的篇幅探讨一下我知道但是没有大规模使用过的两个无头浏览器 —— phantom 和 puppeteer。

作为无头浏览器的开头，我打算先讲讲 phantom。

---

## 安装 & 文档资源

`phantomjs` 的安装比较特殊，在 Ubuntu 上进行安装时出现了 `install.js` 安装脚本提前引用创建路径的问题，看来 `phantomjs` 官方一直没有修复这个 bug。所以虽然可以通过如下的方式进行安装：

```bash
npm install -g phantomjs
```

但是上面的过程大概率会失败，而且 `phantomjs` 也不是一个 js 的 module，它更像是一个独立于 nodejs 的一个单独的 js 运行时。因此，把它作为软件进行安装更为合理。Linux 上安装步骤如下：

```bash
cd ~/download
wget -c https://bitbucket.org/ariya/phantomjs/downloads/phantomjs-2.1.1-linux-x86_64.tar.bz2
tar -xvf phantomjs-2.1.1-linux-x86_64.tar.bz2
```

把解压后的内部的 bin 文件夹加入 `PATH` 即可。

然后我们简单验证一下，编写 `capture.js`：

```bash
var page = require('webpage').create();
var system = require('system');

const args = {};
const argsStack = [];
system.args.forEach(function(piece) {
    if (piece.slice(0, 2) == '--') {
        const argName = piece.slice(2);
        argsStack.push(argName);
    } else if (argsStack.length > 0) {
        const argName = argsStack.pop();
        args[argName] = piece;
    }
});

const url = args['url'];
const domain = url.split('://')[1].split('.')[0];

page.viewportSize = { width: 1920, height: 1080 };
page.open(url, function() {
    page.render(domain + '.jpeg', {format: 'jpeg', quality: '100'});
    phantom.exit();
});
```

命令行输入：

```bash
phantomjs capture.js --url http://baidu.com
```

输入后，会在目录生成一个 baidu 的 screenshot `baidu.jpg`：

<div align=center>
<img src="https://pica.zhimg.com/80/v2-8b96b6f3ce8106bb02f41517f958cbef_1440w.png" style="max-width: 60%;"/>
</div>

说明运行正常。

`phantomjs` 作为无头浏览器，算是一个比较小众的应用，在大部分的领域用得并不多，因此网上可参考的资源并不多，最好的资源就是官方的文档：[https://phantomjs.org/api/](https://phantomjs.org/api/)

---

## 基本用法

就像上面说的，`phantomjs` 是一个独立的 js 运行时，这也意味着不能用常规的方法去编写里面的 js 程序。从我个人实践下来，`phantomjs` 内部的 js 程序的标准必须是 <= ES5 的，所有 ES6 的写法都会导致 `phantomjs` 在运行程序时会直接卡住，这也意味着，`phantomjs` 是用左递归文法编译 js 代码的。

下面的特性都不能在 `phantomjs` 的代码中使用：

- `let` 变量申明
- `() => {}` 箭头函数
- 解构赋值
- 函数的参数设置缺省值（默认值）
- 大部分的 string 的方法

更多有关 ES5 和 ES6 的差异，请移步：[ES5和ES6之间的区别](https://www.cnblogs.com/angel648/p/13256535.html)

在一开始我提供的代码中，开头的两行：

```js
var page = require('webpage').create();
var system = require('system');
```

这两个库都是 `phantomjs` 运行时内部的，你自然无法在运行时内使用自己安装的库。

后面几行是我方便做参数输入写的简单代码，阁下应该能在后续经常用到，为了方便，我下面的例子不再书写 `require` 和参数解析的代码。

### 例子1： 获取网站的标题

通过 `page.evaluate` 可以用于编写在浏览器的控制台环境下你能做的一切操作，比如操作dom，有了dom，用 `document.title` 就能获得网站标题。

```js
page.open(url, function() {
    const title = page.evaluate(function() {
        return document.title;
    })
    console.log(title);
    
    phantom.exit();
});
```

输入：

```bash
phantomjs capture.js --url https://www.zhihu.com/pin/1756890614611095553
```

输入：

```bash
# 一堆警告后

锦恢 的想法: 一次浅尝辄止的 SEO 经验 | 这几天搞 SEO 搞得腰酸背痛（我的网站纯 CSR），明天还有考试，实验室的项目也需要推进，不想折腾了。当我提交了新的 sitemap 后，奇迹发生了——现在的 google 和 bing 搜素引擎居然已经能够解析 SPA 网站的站点信息了（也就是这两家的爬虫能够对于一个爬取的网页上的 Ajax 请求进行多跳爬取，并使用无头浏览器解析最终的画面），所以我这浅尝辄止的 SEO 尝试也就显得没有必要了。 - 知乎
```

为啥会有警告呢？因为知乎用的一些库用了es6标准，导致 `phantomjs` 解析失败。

为了解决这个问题，一共两个办法：

1. 使用 2.5 beta 版本的 `phantomjs`，它几乎支持所有的 es6 标准，但是有点不稳定。
2. 使用最新的无头浏览器 —— google 开发的 Puppeteer。

 

### 例子2：获取一个网页上渲染后的所有文本并保存到 text.json

`page.evaluate` 虽然强大，但是它的回调函数的返回值不能是一个复杂对象，比如 element 就是一个复杂对象，我们只能返回数组或者我们自己构造的简单对象。

```bash

```

输入

```bash
phantomjs scan.js --url https://med.emory.edu/departments/radiation-oncology/research-laboratories/deformable-image-registration/downloads-and-reference-data/4dct.html
```



结果：

<div align=center>
<img src="https://picx.zhimg.com/80/v2-c982c2150448daaf3e05f2df432af522_1440w.png" style="max-width: 60%;"/>
</div>

然后基于得到的数据进行文本挖掘。

除了进行文本提取外，phantom 还支持对表单元素进行操作和一切与 dom 有关的操作。

---

## 总结

phantom 提供了一个轻量级的 无头浏览器解决 方案，通过 phantom 可以轻松获得一些 CSR 网站的文本内容。但是phantom 存在几个致命缺点：

1. 稳定版本无法完整支持 ES6，导致很多现代构建的 CSR 网站都无法解析（比如我的）。
2. 生成的节点存在大量重复节点。

正因为这些缺点，google 也抛弃了 phantom，自己研制了一款全新的无头浏览器 puppeteer。