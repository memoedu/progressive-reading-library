最近在开发一个新的 AI Agent 项目 [SlidevAI](https://github.com/LSTM-Kirigaya/slidev-ai)，这个项目不仅仅是我和太平羊羊在 Modelscope MCPxAgent 比赛的参赛作品，同时也是我用于验证 OpenMCP 生态和方法论的一次重要实践。

那么和大模型服务相关，就一定饶不开 SSE，简单总结一下，我在使用 nestjs 做后端时遇到的一些坑。


## SSE 的基本原理和协议

SSE 本质上是一个 http 长连接，只不过，建立连接后且客户端获取服务端的数据后，并不会立刻切断连接，而是可以持续接受服务端的消息，这个设计使得我们可以利用 SSE 来更加优雅实现一些之前要通过轮询才能实现的耗时功能。

SSE 连接一旦打开，它的数据包遵守如下的协议：

- MIME 类型必须是 `text/event-stream`
- 每行数据为这样的格式：`data: <data>\n\n`

比如下面就是一组 SSE 的响应数据：

```
data: Hello World\n\n
data: {"type":"update","value":123}\n\n
```


## nestjs 注册 SSE 控制器的基本形式

在 nestjs 中，申明一个 sse 连接的方式非常简单：

```typescript
import { Controller, Param, UseGuards, Sse } from '@nestjs/common';
import { Observable } from 'rxjs';

@Controller('slides')
export SlideController {

    @UseGuards(JwtAuthGuard)
    @Sse('process/make-markdown/:id')
    makeMarkdown(
        @Param('id') id: number
    ): Observable<any> {
        return new Observable(subscriber => {
            this.slidesService.makeMarkdownHandler(id, subscriber);
        });
    }
}
```

通过 `subscriber.next("bye");` 就能发送数据。

假设我们的控制器的接入点是 `/api`，那么对于上面的代码，前端就可以如此来请求和获取响应数据：


```js
async function makeMarkdown(id: number) {
    const url = `/api/slides/process/make-outline/${id}`;
    eventSource.value = new EventSource(url, {
        withCredentials: true
    });

    eventSource.value.addEventListener('error', (event: Event) => {
        // 处理错误
    });
    eventSource.value.addEventListener('message', (event: MessageEvent) => {
        // 处理数据 event.data
    })
}
```

看起来非常简单是吧，我就来讲讲我自己遇到的几个坑。

## K1. 鉴权

如果前端你不想造轮子，那么大概率就是基于 `EventSource` 这个内置的方法来发起 SSE，这里就要注意了，`EventSource` 是不支持自定义请求头的，那么假设你的登陆验证的鉴权是基于放在请求头的 Authorization 字段，那么你就不能使用 `EventSource`，必须自己根据我刚才说的 SSE 的协议内容来造轮子解析了，要不然，你就在系统开发初期就基于 Cookie 而非 beaerer token 来维持登陆状态和权限。

所以，尽量前期就将鉴权设计为基于 Cookie 的，我一般喜欢把 jwt 放在 jwt 这个字段，对应的基于 Cookie 的 Strategy 代码：


```typescript
import { ExtractJwt, Strategy } from 'passport-jwt';
import { PassportStrategy } from '@nestjs/passport';
import { Injectable } from '@nestjs/common';
import { UserRepository } from '@/app/users/users.repository';
import { Request } from 'express';

@Injectable()
export class JwtStrategy extends PassportStrategy(Strategy) {
    constructor(private userRepository: UserRepository) {
        super({
            // 自定义从 Cookie 提取 JWT
            jwtFromRequest: ExtractJwt.fromExtractors([
                (request: Request) => {
                    return request.cookies?.jwt;
                },
            ]),
            ignoreExpiration: false,
            secretOrKey: process.env.JWT_SECRET || 'defaultSecretKey',
        });
    }

    async validate(payload: any) {
        return await this.userRepository.findOneById(payload.sub);
    }
}
```

然后前端访问时需携带 Cookie，通过 EventSource 中的 `withCredentials` 参数设置为 `true` 就能实现。

## K2. rxjs 的 next 参数

rxjs 的封装中，我们通过 `subscriber.next()` 方法来发送数据。如果你发送字符串，比如 `subscriber.next('hello')`，那么一切安好，前端可以接收到。但是如果你要发送复杂的对象，直接使用如下的代码是错误的：

```typescript
// 错误代码 ❌
subscriber.next({
    username: 'kirigaya',
    role: 'admin',
    age: 18
});
```

这样发送既不会报错，前端也不会收到任何数据。原因很简单，这么发送，rxjs 最终整合成的数据并不符合 SSE 的协议要求，SSE 协议要求数据格式必须是 `data: {data}\n\n`，而 rxjs 有一个特性，它会把 `{ data: xxx }` 自动序列化为：`data: xxx\n\n`，所以我们可以这么进行发送：

先封装一个基础的函数：

```typescript
export function toSseData(data: any) {
    return {
        data: JSON.stringify(data),
    }
}
```

发送时这么写：

```typescript
subscriber.next(toSseData({
    username: 'kirigaya',
    role: 'admin',
    age: 18
}));
```