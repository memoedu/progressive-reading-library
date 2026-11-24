## 前言

在 [Web Worker 使用与 Wasm 的引入](https://kirigaya.cn/blog/article?seq=221) 这篇文章中，我详细讲述了如何引入 web worker，其中也提到了 `new Worker` 的一个限制——构造 worker 时使用的地址必须和 localhost server 同源，也就是 domain 必须相同。

但是在一些特殊的情况下，我们不得不让通过跨域请求构造 worker，比如 vscode webview，vscode webview 的 domain 是 `vscode-webview`，但是 vscode 内所有请求都必须满足 `vscode-resource` 协议，也就是它们的 domain 都是 `file+.vscode-resource.vscode-cdn.net`。

那么应该如何解决呢？

省流版可以看我在 StackOverflow 上的回答：[Do VSCode webviews support Web Workers?](https://stackoverflow.com/questions/65999175/do-vscode-webviews-support-web-workers/78473163#78473163)

---

## 跨域 worker 构造

废话不多说，直接上代码：

```javascript
const type = 'application/javascript';

/**
 * 
 * @param {string} originalWorkerUrl 
 * @param {{
 *  useBlob?: boolean
 *  skipSameOrigin?: boolean
 * }} options 
 * @returns {Promise<string>}
 */
async function getCrossOriginWorkerURL (originalWorkerUrl, options = {}) {
    const skipSameOrigin = options.skipSameOrigin || true;
    const useBlob = options.useBlob || true;

    // 本来就是同源的，不需要额外进行处理，直接返回
    if (!originalWorkerUrl.includes('://') || originalWorkerUrl.includes(window.location.origin)) {
        return originalWorkerUrl;
    }

    // 获取 worker 的 js 代码
    const res = await fetch(originalWorkerUrl);
    const workerJsCode = await res.text();

    // worker 所在的文件夹 url
    const workerPath = new URL(originalWorkerUrl).href.split('/');
    workerPath.pop();

    // 给 importScripts 的每一个引入的js文件增加 workerPath 组成绝对路径
    const importScriptsFix = `const _importScripts = importScripts;
const _fixImports = (url) => new URL(url, '${workerPath.join('/') + '/'}').href;

importScripts = (...urls) => _importScripts(...urls.map(_fixImports));
`;

    // 以 MIME 协议组成 blob，从而进行装载
    let resultURL = `data:${type},` + encodeURIComponent(importScriptsFix + workerJsCode);
    if (useBlob) {
        // 在外面再套一层 importScripts，这样第二次请求得到的 worker 就不是 blob worker 了
        // 直接使用 createObjectURL 请求一个 worker 的源码是不行的，因为这样得到的 blob worker
        // 会有原本 worker 的 feature 无法使用
        resultURL = URL.createObjectURL(
            new Blob([`importScripts("${resultURL}")`], { type })
        );
    }
    return resultURL;
}

export {
    getCrossOriginWorkerURL
}
```

当我们想要跨域构造 worker 时，如此操作：

```javascript
// 获取 workerPath，可以是地址，可以是网络链接，不需要同源
const workerURL = await getCrossOriginWorkerURL(workerPath);
const worker = new Worker(workerURL);
```

如此，就可以完成跨域 worker 的构造了。

实现原理其实是钻了一个 worker 设计上的小空子——虽然 worker 不允许我们请求跨域地址，但是 worker 内部的 `importScripts` 可以呀！那么我们只需要进行一次简单的套娃构造：

1. 先获取 worker.js 的代码，通过允许跨域请求的函数（比如 `fetch`）即可。
2. 根据特殊需求，修改 worker.js 代码的 `importScripts` 内的路径什么的。比如加一个资源前缀。
3. 把整个代码包装成 MIMIE type 为 `application/javascript` 的base64字符串，然后全部放进一个 `importScripts` 中。
4. 把这个只有一行 importScripts 的作为新的 worker 代码转换成 blob url 装载进入 worker。



事实上，已经有人把这个过程做了一个库 [crossoriginworker](https://www.npmjs.com/package/crossoriginworker)：

```bash
npm install crossoriginworker
```

使用起来和上面一样：

```javascript
import getCrossOriginWorkerURL from 'crossoriginworker';

// acquire "workerPath"

const workerURL = await getCrossOriginWorkerURL(workerPath);
const worker = new Worker(workerURL);
```

---

## vscode webview 中的 worker

在 vscode 扩展开发中，对于一些功能复杂的 webview，有时也需要在里面构造 worker 来分担渲染线程的压力，不让 webview 看着一卡一卡的。

做法和上面类似，在 webview 代码中，先基于上面的技巧进行跨域 worker 构造：

```javascript
// 从 vscode 扩展中获取 workerPath
const workerURL = await getCrossOriginWorkerURL(workerPath);
const worker = new Worker(workerURL);
```



其中的 `workerPath` 是在 vscode host 中经过 vscode-resource 协议编码过的字符串：

```javascript
const workerPath = this.panel.webview.asWebviewUri(vscode.Uri.file("/path/to/worker.js"));
// 将 workerPath 传递给 webview
```

如此一来，就完成了。