## console.log 被干掉了

最近在给一个网页做响应拦截，当我植入脚本观察拦截端口的输出文本时，我发现控制台什么输出都没有。我一开始以为程序的覆写没有成功，就把端口处的 `console.log` 换成了 `alert`，发现疯狂弹窗，说明拦截函数确实被调用了，所以我打开控制台，输入了 `console.log`，看到了如下的输出：

```js
> console.log
< ƒ log() {}
```
这是空的！看来开发者为了防止有人用标准输出暴露网站的工作变量，还挺敬业的。那么要如何解决呢？

## 解决方法

上过 Web内核原理 的小朋友应该都知道，标准输出 console 是被挂载在 window 对象上的，window 对象又是 DOM 全局唯一的，那么我们再整个 DOM 出来，然后把新 DOM 的 console 给这个来一次移花接木不就行了吗？

我把标准输出恢复函数写成 `recoverConsole`，如下：

```js
function recoverConsole() {
    const iframe = document.createElement('iframe');
    iframe.style.display = 'none';
    document.body.appendChild(iframe);
    window.console = iframe.contentWindow.console;
}


(function() {
    'use strict';

    // 注册拦截器
    window.XMLHttpRequest = CustomHttpRequest;
    
    // 恢复标准输入
    recoverConsole();
})();
```

刷新网站，发现没问题了：

<div align=center>
<img src="https://pic1.zhimg.com/80/v2-38af378f785754830ffce34e6e586436_1440w.png" style="width: 60%;"/>
</div>
