## 前言

在构建稳定好用的前后端服务时，我们总希望前端可以稳定地下载后端提供的资源。有的时候，我们在部署整套服务时，可能存在多个资源站点，如何让前端合适地选择较为 “接近” 地选择站点就成为了一个很重要的技术。

本文就简单记录一下我探索出来的简单代码，永远快速实现一个前端 CDN。

---

## 在指定时间内完成一个异步请求

首先，我们需要在指定时间内完成一个请求函数，这个函数向一个 url 发送 stream 类型的 GET 请求，刚拿到响应的 headers 信息就立刻返回总共的时间开销。并且我们需要设置一个 timeout，如果 stream GET 的消耗时间都超过了这个 timeout，那么直接返回 `Infinity`。

这里主要使用了 `axios` 的 cancelToken 和 web 原生的 `clearTimeout` 函数，代码如下：

```typescript
import axios, { CancelTokenSource } from 'axios';
import { performance } from 'perf_hooks';

function measureRequestTimecost(url: string, timeout: number = 5): Promise<number> {
    let s = performance.now();

    let timer: NodeJS.Timeout;
    let cancelTokenSource: CancelTokenSource = axios.CancelToken.source();

    const responseP = axios.get(url, {
        responseType: 'stream',
        cancelToken: cancelTokenSource.token
    });

    return new Promise<number>((resolve, reject) => {
        // 设置超时计时器
        timer = setTimeout(() => {
            cancelTokenSource.cancel('Request timed out');
            resolve(Infinity);
        }, timeout);

        responseP.then(response => {
            // 清除超时计时器
            clearTimeout(timer);

            const totalLength = response.headers['content-length'];
            if (totalLength > 0) {
                resolve(performance.now() - s);
            } else {
                resolve(Infinity);
            }
        }).catch(error => {
            // 清除超时计时器
            clearTimeout(timer);
            if (axios.isCancel(error)) {
                console.log('请求被取消:', error.message);
            } else {
                reject(error);
            }
        });
    });
}
```

`cancelToken` 机制的设计非常优秀，在 vscode extension API 中，前端 LSP 的各个环节也都设计了 `cancelToken` 的机制，用于把之前还在消息队列中未完成的请求取消，达到防抖的效果。

---

## 选择耗时最短的站点

有了上面那个函数，剩下的小学奥赛生估计都能写出来了，假设我们目前有两个站点 github 和 gitee，更多的站点可以自己添加：

```typescript
export async function chooseBestDownloadSource(timeout: number = 3000) {
    const links = [
        'https://gitee.com',
        'https://github.com'
    ];
    const pools: Promise<number>[] = [];
    for (const link of links) {
        pools.push(measureRequestTimecost(link, timeout));
    }
    let fastTc = Infinity;
    let fastTcIndex = -1;
    for (let i = 0; i < pools.length; ++ i) {
        const tc = await pools[i];
        if (tc < fastTc) {
            fastTc = tc;
            fastTcIndex = i;
        }
    }
    if (fastTcIndex == -1) {
        // 全部超时了就采用列表中的第一个
        return links[0];
    }
    return links[fastTcIndex];
}
```