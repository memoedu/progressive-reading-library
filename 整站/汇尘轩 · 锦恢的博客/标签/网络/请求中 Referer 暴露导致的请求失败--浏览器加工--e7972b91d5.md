## 前言

今天在开发一款应用的时候发现一个神奇的问题，我这个应用需要通过 markdown 在网页上渲染若干个图片：

```markdown
![Image 1](https://c-ssl.duitang.com/uploads/item/201910/22/20191022190241_dixsc.thumb.1000_0.jpg)
```

讲道理，markdown 中 image 的请求和渲染，我应该相当熟练了才对，没想到居然渲染失败了，打开控制台一看，发现图片的网站请求红了，我以为是 mcp 返回的图片链接是失效的，然后我就单独访问了上面的链接，是可以如下解析并得到图片的：

<div align=center>
<img src="https://c-ssl.duitang.com/uploads/item/201910/22/20191022190241_dixsc.thumb.1000_0.jpg" style="max-width: 50%;"/>
</div>

诶？见鬼了，为啥我单独访问是没问题的，但是在 app 网页内访问就不行呢？

---

## 问题发现：Referer

没办法，只能细看一下响应头了，下面是响应头中的关键信息：

```nginx
access-control-allow-origin: *
content-type: text/html
location: https://www.duitang.com/
x-error-info: RefererWhite
```

`x-error-info` 一般会一些自定义的错误信息，它反映的是 `RefererWhite`，意思是服务器检查了请求 Referer，因 Referer 不在白名单中而拒绝请求。

:::info 请求头中的 Referer
Referer 是 http 协议中的请求头中的参数，用于告诉服务器，请求你的是谁，对于无状态但是又希望受限鉴权的服务器请求中，可以通过这个参数来限制访问。
:::

那么我们自然就去看请求头的 Referer 参数了，一看，果然：

```
:authority: c-ssl.dtstatic.com
:path: /uploads/item/202007/15/20200715183732_iomsj.thumb.1000_0.jpg
referer: http://localhost:8081/
sec-fetch-site: cross-site
```

我们把请求头的 referer 信息设置成了 http://localhost:8081/，因为这是本地开发的嘛，看起来，目标的图片服务器是加入了 referer 检测的白名单。导致我们请求失败。

---

## 解决方法

最简单的解决方法就是请求的时候直接不发送 Referer 参数了，这个可以通过在 html 中加入 meta 标签实现：

```html
<meta name="referrer" content="no-referrer">
```

当然，因为我这个应用是跑在本地的，所以可以还可以考虑做一些代理转发，预先下载什么的，但是这些都稍微麻烦了一点，还是上面这个简单。