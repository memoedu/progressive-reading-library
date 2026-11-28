## 前言

最近在给 openmcp 做 gemini 的适配，可给我折腾坏了，遇到两个大坑，看得出来 google 对于 gemini 非常不上心呀！

起因是这样的，我的用户反应 openmcp 无法使用 gemini，我就光速注册了一个账号测试，结果发现确实不行，然后我就调试，发现 openai 这个库卡住了。幽默 gemini，官网上的 javascript demo 全部失效。非常好文档，使我亚托莉爆炸。

<div align=center>
<img src="https://pic4.zhimg.com/80/v2-a218b99617be787e214cd9b809c5e61d_1440w.webp" style="width: 100%;"/>
</div>

经过层层调试，我甚至还把 openai-node 的源代码看了一遍，我发现了问题所在：openai 默认采用 fetch 来进行 post 请求获取 SSE 响应，但是 fetch 对于 SSE 响应的处理似乎存在问题，具体问题是什么我不知道，但是换成 axios 就可以正常获得 SSE 响应了。

> 这个期间我尝试了ky, cross-fetch, ofetch, redaxios 等库，不行，全都会卡住。

除此之外，openai 库还会为请求附带如下的请求头：

```bash
'User-Agent': 'OpenAI/JS 5.0.1'
```

通过阅读 openai-node 源代码，我发现了 openai 这个库给了一个接口，可以自定义网络请求的处理逻辑，但是有一个要求：返回值必须是一个 fetch 原生的 Response 对象，于是有了这篇文章，我们需要把 axios 进行封装，使得它的返回值可被识别为一个 Response 对象（需要特别支持 SSE 的响应）。也就是我们需要实现一个 axios 的 adapter。

---

## axios adapter

基本实现比较简单，我不解释了，直接贴代码了：

```typescript
import axios from "axios";
import { HttpsProxyAgent } from 'https-proxy-agent';

interface FetchOptions {
    method?: string;
    headers?: Record<string, string>;
    body?: string | Buffer | FormData | URLSearchParams | object;
    [key: string]: any;
}

interface FetchResponse {
    ok: boolean;
    status: number;
    statusText: string;
    headers: Headers;
    url: string;
    redirected: boolean;
    type: string;
    body: any;

    json(): Promise<any>;
    text(): Promise<string>;
    arrayBuffer(): Promise<ArrayBuffer>;
    getReader(): ReadableStreamDefaultReader;
}

interface ReadableStreamDefaultReader {
    read(): Promise<{ done: boolean, value?: any }>;
    cancel(): Promise<void>;
    releaseLock(): void;
    get closed(): boolean;
}

/**
 * 将 axios 配置转换为 fetch 风格的配置
 */
function adaptRequestOptions(url: string, options: FetchOptions = {}): any {
    const axiosConfig: any = {
        url,
        method: options.method || 'GET',
        headers: options.headers,
        responseType: 'stream'
    };

    // 处理 body/data 转换
    if (options.body) {
        if (typeof options.body === 'string' || Buffer.isBuffer(options.body)) {
            axiosConfig.data = options.body;
        } else if (typeof options.body === 'object') {
            // 如果是 FormData、URLSearchParams 等特殊类型需要特殊处理
            if (options.body instanceof FormData) {
                axiosConfig.data = options.body;
                axiosConfig.headers = {
                    ...axiosConfig.headers,
                    'Content-Type': 'multipart/form-data'
                };
            } else if (options.body instanceof URLSearchParams) {
                axiosConfig.data = options.body.toString();
                axiosConfig.headers = {
                    ...axiosConfig.headers,
                    'Content-Type': 'application/x-www-form-urlencoded'
                };
            } else {
                // 普通 JSON 对象
                axiosConfig.data = JSON.stringify(options.body);
                axiosConfig.headers = {
                    ...axiosConfig.headers,
                    'Content-Type': 'application/json'
                };
            }
        }
    }

    return axiosConfig;
}

/**
 * 将 axios 响应转换为 fetch 风格的 Response 对象
 */
function adaptResponse(axiosResponse: FetchOptions): FetchResponse {
    // 创建 Headers 对象
    const headers = new Headers();
    Object.entries(axiosResponse.headers || {}).forEach(([key, value]) => {
        headers.append(key, value);
    });

    // 创建符合 Fetch API 的 Response 对象
    const fetchResponse = {
        ok: axiosResponse.status >= 200 && axiosResponse.status < 300,
        status: axiosResponse.status,
        statusText: axiosResponse.statusText,
        headers: headers,
        url: axiosResponse.config.url,
        redirected: false, // axios 不直接提供此信息
        type: 'basic', // 简单类型
        body: null,

        // 标准方法
        json: async () => {
            if (typeof axiosResponse.data === 'object') {
                return axiosResponse.data;
            }
            throw new Error('Response is not JSON');
        },
        text: async () => {
            if (typeof axiosResponse.data === 'string') {
                return axiosResponse.data;
            }
            return JSON.stringify(axiosResponse.data);
        },
        arrayBuffer: async () => {
            throw new Error('arrayBuffer not implemented for streaming');
        },

        // 流式支持
        getReader: () => {
            if (!axiosResponse.data.on || typeof axiosResponse.data.on !== 'function') {
                throw new Error('Not a stream response');
            }

            // 将 Node.js 流转换为 Web Streams 的 ReadableStream
            const nodeStream = axiosResponse.data;
            let isCancelled = false;

            return {
                read: () => {
                    if (isCancelled) {
                        return Promise.resolve({ done: true });
                    }

                    return new Promise((resolve, reject) => {
                        const onData = (chunk: any) => {
                            cleanup();
                            resolve({ done: false, value: chunk });
                        };

                        const onEnd = () => {
                            cleanup();
                            resolve({ done: true });
                        };

                        const onError = (err: Error) => {
                            cleanup();
                            reject(err);
                        };

                        const cleanup = () => {
                            nodeStream.off('data', onData);
                            nodeStream.off('end', onEnd);
                            nodeStream.off('error', onError);
                        };

                        nodeStream.once('data', onData);
                        nodeStream.once('end', onEnd);
                        nodeStream.once('error', onError);
                    });
                },

                cancel: () => {
                    isCancelled = true;
                    nodeStream.destroy();
                    return Promise.resolve();
                },

                releaseLock: () => {
                    // TODO: 实现 releaseLock 方法
                },

                get closed() {
                    return isCancelled;
                }
            };
        }
    } as FetchResponse;

    // 设置 body 为可读流
    if (axiosResponse.data.on && typeof axiosResponse.data.on === 'function') {
        fetchResponse.body = {
            getReader: fetchResponse.getReader
        };
    }

    return fetchResponse;
}

/**
 * @description 主函数 - 用 axios 实现 fetch
 */
export async function axiosFetch(input: any, init: any, requestOption: { proxyServer?: string } = {}): Promise<any> {
    const axiosConfig = adaptRequestOptions(input, init);

    const {
        proxyServer = ''
    } = requestOption;

    if (proxyServer) {
        const proxyAgent = new HttpsProxyAgent(proxyServer);
        axiosConfig.httpsAgent = proxyAgent;
        axiosConfig.httpAgent = proxyAgent;
    }

    try {
        const response = await axios(axiosConfig) as FetchOptions;
        return adaptResponse(response);
    } catch (error: any) {
        if (error.response) {
            return adaptResponse(error.response);
        }
        throw error;
    }
}
```

---

## 在 openai 中替换原本的网络请求

实现完成 axios adapter 后，我们只需要在 openai 这个库中通过 fetch 参数来自定义网络请求行为即可：

```typescript
import { OpenAI } from "openai";

async function askLlm() {
// ... existing code ...

    const client = new OpenAI({
        baseURL,
        apiKey,
        fetch: async (input: string | URL | Request, init?: RequestInit) => {

            console.log('openai fetch begin, proxyServer:', proxyServer);
            
            if (model.startsWith('gemini') && init) {
                // 该死的 google
                init.headers = {
                    'Content-Type': 'application/json',
                    'Authorization': `Bearer ${apiKey}`
                }
            }
            
            return await axiosFetch(input, init, { proxyServer });
        }
    });

// ... existing code ...
}
```
最终，我也是成功支持了 gemini 的使用，难怪大部分厂商都选择不去支持 gemini，gemini 的 openai 适配做得确实乏善可陈呀 lol

最后贴一张 openmcp 的成功运行截图

<div align=center>
<img src="https://pic1.zhimg.com/80/v2-38be51e4055636abdece3be4a1cb2df2_1440w.png" style="width: 100%;"/>
</div>

欢迎大家为 openmcp 点 star：[LSTM-Kirigaya/openmcp-client](https://github.com/LSTM-Kirigaya/openmcp-client)

这是 [openmcp 官方文档](https://kirigaya.cn/openmcp/)