[Document | 文档](https://document.kirigaya.cn/blogs/lagrange.onebot/main.html)

## 前言

最近突发奇想，想要给我的 Digital-IDE 交流群做一个问答机器人了，去搜了一圈现在支持 NTQQ 的机器人框架，发现跑不起来，要么就是要去给linux的镜像打补丁，有点麻烦，正巧发现 Lagrange.Core 可以 run 起来，所以想着直接用 Lagrange.Core 做个机器人框架吧！摸鱼了三天，有了现在的这个 typescript 库 `lagrange.onebot`

- [github 项目](https://github.com/LSTM-Kirigaya/Lagrange.onebot)
- [npmjs 主页](https://www.npmjs.com/package/lagrange.onebot)
- [Lagrange.onebot 文档](https://document.kirigaya.cn/blogs/lagrange.onebot/main.html)

下面就是我的文档的“快速开始”这一章的内容了。还是希望阁下可以给我的github一个免费的star。

<div align=center>
<img src="https://pica.zhimg.com/80/v2-5b8c9b9533145fcd76f6869de31da745_1440w.jpeg" style="width: 50%;"/>
</div>

---

基于 Lagrange.Core 实现的 NTQQ 接入框架。

> 本项目由于使用了装饰器特性，所以目前只支持使用 typescript 进行开发。

## 相关项目

<table>
<tr>
  <td><a href="https://github.com/LagrangeDev/Lagrange.Core">Lagrange.onebot</a></td>
  <td> TypeScript 实现轻量级 OneBot 协议的机器人框架（👈你在这里</td>
</tr>
<tr>
  <td><a href="https://github.com/LagrangeDev/Lagrange.Core">Lagrange.Core</a></td>
  <td>NTQQ 的协议实现</td>
</tr>
<tr>
  <td><a href="https://github.com/whitechi73/OpenShamrock">OpenShamrock</a></td>
  <td>基于 Lsposed 实现 OneBot 标准的机器人框架</td>
</tr>
<tr>
  <td><a href="https://github.com/chrononeko/chronocat">Chronocat</a></td>
  <td>基于 Electron 的、模块化的 Satori 框架</td>
</tr>
</table>


## 安装与使用

### 启动拉格朗日服务器

[Lagrange文档 - 快速部署 & 配置](https://lagrangedev.github.io/Lagrange.Doc/Lagrange.OneBot/Config/)

请使用 `screen`, `tmux` 或者 `pm2` 工具将拉格朗日挂在后台。

> 请使用默认配置！目前我们只支持拉格朗日默认的反向ws连接，虽然我知道你们肯定懒得折腾另外两种连接方式。


### 安装与配置本项目

在你的项目文件夹中

```bash
# npm
npm install lagrange.onebot

# yarn
yarn add lagrange.onebot
```

修改 `tsconfig.json`，开启装饰器

```json
{
    "compilerOptions": {
        ...
        "experimentalDecorators": true,
        ...
    }
}
```

### 开始你的第一个 hello world

我们新建两个文件 `main.ts` 和 `impl.ts`. 也就是本项目 `./test` 文件夹下的内容。

下面的代码演示了如何开启服务，并在服务开启和关闭时给某个人发送消息。

```typescript
// main.ts
import { server } from 'lagrange.onebot';
import './impl';

// server 刚启动的时候要做的事情
server.onMounted(c => {
    // 向 QQ 号为 1193466151 的好友发送文本信息 "成功上线"
    c.sendPrivateMsg(1193466151, '成功上线');
});

// server 即将关闭时要做的事情
server.onUnmounted(c => {
    // 向 QQ 号为 1193466151 的好友发送文本信息 "成功下线"
    c.sendPrivateMsg(1193466151, '成功下线');
})

server.run({
    // 拉格朗日服务器中的配置参数
    host: '127.0.0.1',
    port: 8080,
    path: '/onebot/v11/ws',

    // 你启动的机器人的 QQ 号
    qq: 1542544558
});
```

如果想要自定义对于某个人或者某个群的回答行为，可以通过 `mapper 注解/装饰器` 的方式将您写的业务函数装配进事务管线中， `lagrange.onebot` 会自动去处理这些信息。写法综合了 java 的 `springboot` 和 go 的 `gin` ，对于熟悉后端开发的同学而言，应该非常简单。

```typescript
// impl.ts

import { mapper, LagrangeContext, PrivateMessage } from 'lagrange.onebot';

export class Impl {

    // 将对于用于 1193466151 的应答函数装配进管线中
    @mapper.onPrivateUser(1193466151)
    async handleJinhui(c: LagrangeContext<PrivateMessage>) {
        /**
         * c 是 lagrange.onebot 中最核心的上下文，它包含了所有满足 
         * onebot v11 协议的 API
         * c.message 是当前事务的消息
        */
        const msg = c.message.raw_message;
        const reply = '你刚刚的回答是 ' + msg;
        c.sendPrivateMsg(c.message.user_id, reply);
        // 和下面几种写法完全等价
        // c.sendPrivateMsg(1193466151, reply);
        // c.sendMessage(reply);


        // finishSession 会标记当前事务为“已完成”，此时 c.fin 为 true
        // c.fin 为 true 的情况下，所有 onebot v11 API 全部失效
        c.finishSession();
    }
}
```

效果预览

<div align=center>
<img src="https://picx.zhimg.com/80/v2-582932c3b84177184ce83aa8d12ee94b_1440w.png" style="width: 80%;"/>
</div>


更多使用方法，请参考：

- [Lagrange.onebot 官方文档]()
- [Langrange.RagBot 一个将 RAG 加持下的大模型接入 QQ 的项目](https://github.com/LSTM-Kirigaya/Lagrange.RagBot)