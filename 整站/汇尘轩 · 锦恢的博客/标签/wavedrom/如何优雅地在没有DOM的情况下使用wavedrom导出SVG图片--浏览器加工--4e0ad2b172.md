这一个月都在写一个vscode插件，目前进展到自动文档化，需要使用wavedrom这个开源标准来支持一下父老乡亲写的wavedrom风格的注释。浅搜了一下，发现网上的教程（包括官网的）都是和DOM强绑定的，但是我们都知道vscode是没有DOM的，（webview内嵌JS可以访问，但是我不打算这么做）。所以问题来了，如何使用wavedrom编写一个输入为wavedrom风格注释，输出为对应的svg的函数？

因为我打算做一个可以导出markdown, html, pdf的功能，所以使用泛用性最好的svg格式。

---

## 环境 & 准备
nodejs：v18.12.0
打开一个文件夹，安装一下wavedrom：

npm install wavedrom
因为vscode的默认js是common js，所以下面就按照common js的风格编写。

---

## 解读流程
我相信点开这篇文章的诸位应该都没有耐心听我讲解源代码。其实wavedrom的源代码去掉skin，剩余的逻辑其实非常短，所以嘛，不讲问题也不大。

wavedrom的核心函数是一个没有被export出来的函数：renderAny。这玩意儿和内部和DOM没有关系，是完整的输出svg元信息的函数，返回值是一个非常混球的对象（作者全篇不写注释，体验很差）。

继续看你会发现，作者基于onml这个库将renderAny的返回值转换成一个svg标签元素（onml里面有一个函数stringify可以将一个js对象同构地转换成XML/HTML/SVG字符串）。

知道这些之后，我们可以写一个支持单线程的转换函数（文件名 easy-wavedrom）：
```js
const renderAny = require('wavedrom/lib/render-any');
const onmlStringify = require('onml/stringify.js');
const darkSkin = require('wavedrom/skins/dark');
const lightSkin = require('wavedrom/skins/default');

const Style = {
    light : lightSkin,
    dark : darkSkin
};

const Global = {
    makeTimes : 0
}

/**
 * @description make svg according to the input wavedrom-style json
 * @param {string} wavedromComment
 * @param {string} style light, dark
 * @returns {string} 
 */
function makeWaveDromSVG(wavedromComment, style) {
    try {
        const json = JSON.parse(wavedromComment);
        const index = Global.makeTimes;
        const skin = Style[style];
        const renderObj = renderAny(Global.makeTimes, json, skin);
        const svgString = onmlStringify(renderObj);
        Global.makeTimes += 1;
        return svgString;
    } catch (error) {
        return undefined;
    }
}


module.exports = {
    makeWaveDromSVG
};
```
然后我们在main.js中测试它：

```js
const fs = require('fs');

const { makeWaveDromSVG } = require('./easy-wavedrom');

const rawJson = fs.readFileSync('wavedrom.test.json', 'utf-8');
const svgString = makeWaveDromSVG(rawJson, 'dark');
fs.writeFileSync('wavedrom.test.svg', svgString);
```

其中wavedrom.test.json为：

```json
{ 
    "signal" : [
        { "name": "clk",  "wave": "p......" },
        { "name": "bus",  "wave": "x.34.5x",   "data": "head body tail" },
        { "name": "wire", "wave": "0.1..0." }
  ]
}
```
运行main.js，其中makeWaveDromSVG是一个字符串，你可以把它直接插入需要生成的HTML，也可以直接嵌入Markdown中，或者在SPA中更加灵活地操作。生成效果我就用vscode来展示了：