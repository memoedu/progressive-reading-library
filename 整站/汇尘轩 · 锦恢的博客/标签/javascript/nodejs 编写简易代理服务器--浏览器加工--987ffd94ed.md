## 前言

编写代理服务器 proxy 是每一个守法公民都可能会的东西，特别是在我们进行内网穿透等场景下，对某个套接字段进行代理是非常常见的事情。本文就简单讲一下如何使用编写网络应用层的常用语言 JavaScript 编写一个简单的代理局域网 https 服务的代理器。

原理非常简单，我就不做解释了。代码如下所示。

---

## 代码

```js
// proxy.js
const https = require('https');
const http = require('http');
const httpProxy = require('http-proxy');

const config = {
    target: 'https://172.16.120.7:32206',
    port: 8001
}

// 如果目标服务器的安全证书没有问题的话，这步可以不写
const agent = new https.Agent({
    rejectUnauthorized: false
});


const proxyServer = httpProxy.createProxyServer({
    secure: false,
    agent: agent
});

const server = http.createServer(( req, res ) => {
    console.log(`[nodejs proxy] transfer to ${config.target}${req.url}`);
    proxyServer.web(req, res, { target: config.target });
});


server.listen(config.port, () => {
    console.log(`[nodejs proxy] start successfully, ${config.target} -> https://localhost:${config.port}`);
});
```

编写完成后，我们在后台运行：

```bash
node proxy.js	
```

运行后，我们访问代理页面，效果如下：

<div align=center>
<img src="https://picx.zhimg.com/80/v2-935b11de5bb1401f4ca2811ff3c8df51_1440w.jpeg" style="width: 60%;"/>
</div>

proxy.js 控制台的打印信息如下：

<div align=center>
<img src="https://picx.zhimg.com/80/v2-76238f1a29dbc68cc73e6bd7e0465205_1440w.jpeg" style="width: 60%;"/>
</div>