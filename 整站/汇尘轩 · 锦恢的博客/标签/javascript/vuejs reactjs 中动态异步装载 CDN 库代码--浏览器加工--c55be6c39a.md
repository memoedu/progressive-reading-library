## 问题背景

相信做过前端项目，特别是有追求的前端项目的同学，在一次使用 `vue.js/react.js` 这类 SPA 时都遇到过这样的问题：我希望调用一个库完成我的需求，比如我的需求就是在网站上播放乐谱，然后你 google 到了对应的库，但是发现这个库只提供了通过 `script` 标签引入的调用方式，比如：

```html
<script src="https://cdn.jsdelivr.net/npm/@coderline/alphatab@latest/dist/alphaTab.min.js"></script>
<div id="alphaTab" data-tex="true">

<script type="text/javascript">
const element = document.getElementById('alphaTab');
const api = new alphaTab.AlphaTabApi(element);
</script>
```

> alphaTab.js 本身提供了 ES6 标准下的 npm 包，我这里只是举个例子。

但是你的 vue.js 只允许通过 `npm install` 的方式安装包和 `import` 的方式调用包。那么怎么做才能引入这个包呢？

## 普通的方法

很多人能想到的办法无非如下几条：


1. 直接在 SPA 的入口文件 index.html 中加入这个 script 标签，在其中引入库代码，然后用挂载在 `window` 对象上的方法来调用这个库。这么做最省事，但是会让你的网站在刚刚载入时就开始加载这个库，如果你的网站上开发了许多的功能模块，那么其他用户载入你网站的首屏加载时间会大幅度上升，严重影响用户体验。
2. 自己基于 script 的库和官方文档自己想办法写一个库，然后上传到 npmjs 造福其他用户。但是这么做需要深入了解库代码，费时费力。
3. 在一个独立的 html 文件中通过 script 引入库，然后在 SPA 中，通过 iframe 引入这个独立的 html。这么做可以很好规避上面两个方法的问题，也是我过去三年常用的方法。但是这么做有如下几个问题：
    - 通过 iframe 引入的网页需要使用 iframe 的通讯 API 来与 iframe 内部的库函数进行交互，无疑增加了网站的后期维护成本。
    - iframe 在父元素上大小不会根据内部元素的排版而自动发生变化，如果你的 iframe 内部的东西比较多， 使用 iframe 得到的效果会很难看，它的高度/宽度不会被撑开。不足的部分会以 iframe 滚动条的形式出现。严重影响网站的美观程度。


## 动态引入库代码

### script 标签引入库时的浏览器行为

在介绍本文的方法之前，我们需要先了解一下基于 script 标签的库代码引入对应的浏览器行为是什么样子的。当我们在 html 中加入如下语句：

```html
<script src="https://cdn.jsdelivr.net/npm/@coderline/alphatab@latest/dist/alphaTab.min.js"></script>
```

浏览器的会先去请求其中的链接，请求到链接的代码后（是一大堆文本），将代码解析并装载进 V8 运行时中，此时，这堆小混蛋就是一个真正 JS 对象，并且 V8 运行时会将这个小混蛋挂载在全局对象 window 上面。你可以打开控制台，然后输入 `window.<你引入的库的对象>`（比如我的就是 `window.alphaTab`），就可以看到引入库对象的所有可用方法了：

<div align=center>
<img src="https://pica.zhimg.com/80/v2-0f778e8ac37b555362c1ba05434bb1a1_1440w.png" style="width: 80%;"/>
</div>

既然知道了这个过程，那么就简单了呀！我们直接在 JavaScript 代码层面模拟这个过程不就能够实现想在哪里加载库就在哪里加载库了吗？

### JavaScript 动态异步装载库代码函数

显然，我们的动态装载代码不能阻塞主进程，所以我们的装载代码肯定要返回一个 `Promise` 来实现异步装载库代码。

一般情况下，虽然你可以通过 window 挂载完后的库对象，但是我仍然建议读者在自己的代码中设置一个全局对象来指向这个库代码，比如下面代码中的 `_alpha_tab_instance`。这样可以更好地远离该死的 window 对象。

```javascript
let _alpha_tab_instance = null;

function load(src) {
    if (_alpha_tab_instance) {
        return;
    }
    const script = document.createElement('script');
    script.src = src;

    return new Promise((resolve, reject) => {
        script.onload = () => {
            console.log('finish loading lib from ' + src);
            _alpha_tab_instance = window.alphaTab;
            resolve();
        };
        script.onerror = (error) => {
            console.error('Error loading lib from ' + src,  error);
            reject(error);
        };
        document.head.appendChild(script);
    });
}
```


你在你的什么奇奇怪怪的 `xxx.vue` 或者 `xxx.jsx` 中就可以这么写：

```javascript
async function 初始化函数() {
    await load('https://kirigaya.cn/files/web/alphaTab.min.js')
}
```

为了更好的用户体验，你也可以在 await 前后包一个 Loading 的遮罩层：

<div align=center>
<img src="https://picx.zhimg.com/80/v2-8abeb23321375dadafa2108f3de4b5d2_1440w.png" style="width: 80%;"/>
</div>

等到遮罩层消失，你就可以肆无忌惮地调用内部存储对象或者 window 上的玩意儿了。

比如我现在可以欣赏丝滑的吉他谱载入动画和播放功能了，都是基于这样的装载函数实现的。

Enjoy ~

---

不说了，我要去写会儿数学题放松一会下了。

<div align=center>
<img src="https://picx.zhimg.com/80/v2-244739e67dd9d70fbea1e4a7e48300da_1440w.png" style="width: 50%;"/>
</div>