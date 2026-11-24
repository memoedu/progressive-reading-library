## 什么是 webview ？

vscode 的 webview 为 vscode 插件生态提供了非常强大的自由度。锦恢之所以坚持开发 vscode 插件而没有倒戈去 neovim 生态最大的原因莫过于 vscode 的 webview，你可以当场查看 markdown 渲染结果：

<div align=center>
<img src="https://picx.zhimg.com/80/v2-9424a46f5b7efe3b28e823a19b889ff2_1440w.jpeg" style="max-width: 60%;"/>
</div>

你可以在 vscode 里面开心地画流程图也是因为其中的 webview（drawio）在工作

<div align=center>
<img src="https://pic1.zhimg.com/80/v2-183eb2730f9cd1037e303eb25f3b455e_1440w.jpeg" style="max-width: 60%;"/>
</div>

你甚至可以在 vscode 里面看视频：

<div align=center>
<img src="https://pic1.zhimg.com/80/v2-4a9b5c2de848e72a1c64b48984504efe_1440w.jpeg" style="max-width: 60%;"/>
</div>

下面这个是我用 webgl2 开发的 vcd 渲染器，也是基于 vscode webview：

<div align=center>
<img src="https://pica.zhimg.com/80/v2-bc9844383bff10498ba78c5b2277d8d3_1440w.jpeg" style="max-width: 60%;"/>
</div>

---

## 前言

基础教程在 vscode 官方的 API 文档中讲到了，写得挺好的：https://code.visualstudio.com/api/extension-guides/webview 看完这个教程，对于基本 API 的使用就能了解了。

当然，做 Webview 开发需要一定的 Web 基础。

不过 vscode 插件开发的教程一直非常非常少，关于 webview 的少之又少，关于将大规模前端应用嵌入 webview 的教程全网更是查无此文。主要是因为细节确实太多，少部分会的人（比如我）也没有时间全部写下来，太琐碎了。

本文主要讲一下你在开发 vscode webview 插件时， 一定会遇到但是基本上搜不到的东西。

---

##  webview 的工作权限

webview 装载的 html 默认是不允许执行 JavaScript ，也不允许发起任意形式的网络请求（包括请求 css，JavaScript，fetch 等等）。这对于一些简单的应用，比如渲染一张图片，是没啥问题的，但是你的应用只要稍微复杂一点，就一定需要用到 fetch 这样的请求，比如 webpack 打包的结果。

### 获取执行脚本的权限

执行 js 的权限，只需要在创建 webview 时，设置运行执行的参数即可：

```javascript
const panel = vscode.window.createWebviewPanel(
    'Wave Viewer',
    'Wave Viewer',
    vscode.ViewColumn.One,	// 渲染成单独的页面，且不分栏
    {
        enableScripts: true, // 运行执行 js
    }
);
```



### webview 发起资源请求的方法

虽然 webview 不能直接发起任意形式的网络请求，但是 vscode 提供了一种特殊的协议 —— `vscode-resource` 协议，简单说就是，如果你请求的资源符合如下形式，那么网络请求就会被 vscode 许可：

```
https://file+.vscode-resource.vscode-cdn.net/<请求资源的绝对路径的uri>
```

为了让一个已经打包好的网页应用可以装载进入 vscode，我们一般会写一个叫做 `getWebviewContent` 的函数，用来给 html 的每一个资源请求项加上上面这一堆的前缀。

比如我的网页项目打包完成后在当前插件项目的 `./resouces/viewer` 下，那么 `getWebviewContent` 这么写：

```javascript
import * as fs from 'fs';
import * as fspath from 'path';

public getWebviewContent(): string | undefined {
    const extensionPath = ... // 此处通过 context.extensionPath 获取插件的根目录绝对路径
    // 找到你的 index.html 所在文件夹的绝对路径
    const htmlRoot = fspath.join(extensionPath, 'resources', 'viewer');
    const htmlIndexPath = fspath.join(htmlRoot, 'index.html');
    const html = fs.readFileSync(htmlIndexPath)?.replace(/(<link.+?href="|<script.+?src="|<img.+?src=")(.+?)"/g, (m, $1, $2) => {
        const absLocalPath = fspath.resolve(htmlRoot, $2);
        // this.panel 就是你创建的 webview 对象
        const webviewUri = this.panel?.webview.asWebviewUri(vscode.Uri.file(absLocalPath));
        const replaceHref = $1 + webviewUri?.toString() + '"';
        return replaceHref;
    });
    return html;
}
```

这样，`index.html` 中所有的js，css，图片请求会被改造为符合 `vscode-resource` 的形式。

但是，这么做还会有问题，如果你是使用 react，vue 这类 SPA 框架写的网页，然后用的 vite 或者 webpack 打包的，那么你在项目中引用的资源是不会被改造的。比如你在 `sidebar.vue` 中创建了一个图片：

```html
<template>
    ...
    <img src="../assets/image.png"/>
    ...
</template>
```

上面这段代码会被编译成 js 代码， img 标签的 src 会变成一个 js 的字符串，它们并不在 `getWebviewContent` 改造的范围内。这种情况建议不要使用 img 载入图片，而是使用 div 标签 + css，比如上面的例子改写成这样：

```html
<template>
    ...
    <div class="my-image"/>
    ...
</template>

<style>
    .my-image {
        height: xxx;
        width: xxx;
        background-image: url(../assets/image.png);
    }
</style>
```

这是因为 vscode 并不限制 webview 的 css 发起的请求。所以尽量用 css 来获取本地图片资源吧！

---

## webview 如何设置图标

默认情况下， 你创建的 webview 的图标是空的，如下图：

<div align=center>
<img src="https://picx.zhimg.com/80/v2-c64f3e0e394ee2acc2b79d372e2cf823_1440w.png" style="max-width: 60%;"/>
</div>

设置图标通过创建好的 panel ，设置它的 iconPath 即可：

```javascript
// 创建 panel
const panel = vscode.window.createWebviewPanel({ ... })

// 获取图标的绝对路径
const iconPath = fspath.join(extensionPath, 'images', 'icon.svg');

// 设置图标 uri
panel.iconPath = vscode.Uri.file(iconPath);
```

效果如下：

<div align=center>
<img src="https://picx.zhimg.com/80/v2-e79d127f0eb820def23ef4d67a8fe550_1440w.png" style="max-width: 60%;"/>
</div>

---

## webview 如何和 vscode 进行通信

### vscode → webview

vscode 向 webview 发送数据可以通过 `panel.webview.postMessage` 实现，参数可以是任意对象：

```javascript
this.panel?.webview.postMessage({
	command: 'python -m http.server',
    args: '--port 9090'
});
```

webview 端的 html 可以通过监听 message 事件来获取 vscode 发送的数据：

```js
window.addEventListener('message', event => {
    const message = event.data;
    const { command, args } = message;
    console.log(command);		// python -m http.server
    console.log(args);			// --port 9090
});
```

### webview → vscode

而 webview 如果想向 vscode 发送数据（这么做不常见，尽可能不要把你的软件设计成这样），webview 端可以使用 vscode webview 环境下的一个特殊的库——“vscode”，通过 `acquireVsCodeApi` 获取，然后发送数据的写法和 vscode 端发送数据的写法差不多：

```js
const vscode = acquireVsCodeApi();
vscode.postMessage({
    result: "一切安好",
    timeCost: 2000
});
```

vscode 端通过 `panel.webview.onDidReceiveMessage` 监听并获取数据：

```js
this.panel?.webview.onDidReceiveMessage(message => {
   const { result, timeCost } = message;
    console.log(result);		// 一切安好
    console.log(timeCost);		// 2000
});
```

### 一种粗暴但是很美妙的方法——用于使用 vue/react 开发 webview 插件

虽然 vscode 提供了这样一套机制，但是这么做在进行开发时非常恶心，因为：

- 我们使用 vue 和 react 开发 app 时并不在 vscode 插件环境内，既没有 `message` 消息可以监听，也没有 `acquireVsCodeApi`
- 只有 webpack 打包扔到插件项目并启动调试后才能使用上述的方法，在开发中这是不可能的。

而且最重要的一点是，大部分 webview 插件并不需要全双工通信，只需要 vscode 向 webview 传递数据完成初始化即可，比如图片查看器，它只需要 vscode 把 图片的 uri 传给 webview 就行。

为了更好的 vue 插件开发体验，我介绍一种我使用的开发方法。

首先，我们假设我们开发的是一个图片查看器，它需要一个图片的链接作为输入，那么我们先把开发测试用的 `image.png` 放在 `./public` 文件夹下，然后在 `./public/index.html` 中这么写：

```html
<!DOCTYPE html>
<html lang="">

<head>
    <meta charset="utf-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width,initial-scale=1.0">
    <link rel="icon" href="<%= BASE_URL %>icon.png">
    <title>锦恢的图片查看器</title>

    <script>
        window.readImageData = async () => {
            const response = await fetch('image.png');
            const arraybuffer = await response.arrayBuffer();
            return arraybuffer;
        }
    </script>
</head>

<body>
    <div id="app"></div>
</body>

</html>
```

也就是说，我们现在 index.html 中定义了一个挂载在全局的获取图像的函数。然后在开发阶段，我们只需要在 vue 内部使用 `window.readImageData` 就能获取这个图像，从而顺利完成图片查看器的开发了。

假设你顺利完成了开发，然后使用 webpack 打包完成了。

此时，你会把打包完成的结果放进插件项目的某个文件夹内，并使用前文介绍的 `getWebviewContent` 获取了使用了 `vscode-resource` 改造的 `index.html`。此时，你要创建这个图片查看器 app 到 webview 上，而且已经获得了需要渲染的图片的 uri，那么你要做的事情就超级简答啦——直接把 `image.png` 替换成图片的 uri 就行了：

```js
const previewHtml = this.getWebviewContent();
const webviewUri = this.panel.webview.asWebviewUri(uri);
this.panel.webview.html = previewHtml.replace('image.png', webviewUri.toString());
```

是不是暴力又简单？

<div align=center>
<img src="https://picx.zhimg.com/80/v2-9ba7d9f525d5b4ff888cf15d5c065ae2_1440w.jpeg" style="max-width: 60%;"/>
</div>

这样做不仅在开发时完全不需要顾及 vscode 本身那该死的通信机制，而且还能直接一行代码完成vscode 数据的传入。

## 打包

这个很坑，有空再来写。