## 前言

为了方便开发更加复杂的 webview，我需要快速开发一个与 vscode extension host 心智模型完全相同的测试后端，因此，我把眼光快速投向了 typescript + express，下面是一个快速搭建 express 后端的 **最小例程**。

---

## 最小例程

安装 ts 和开发组件

```bash
npm install @types/cors @types/express @types/morgan @types/node ts-node typescript --save-dev
```

安装 express 和对应中间件

```bash
npm install express body-parser bson morgan cors
```



一个简单的例子如下：

```typescript
import * as path from 'path';
import * as fs from 'fs';

import express, { Request, Response } from 'express';
import morgan from 'morgan';
import cors from 'cors';

import bodyParser from 'body-parser';
import { BSON } from 'bson';

const corsOptions = {
    // 只允许 8080 端口访问过来的前端
    origin: 'http://localhost:8080',
    // 一些旧版浏览器（如 IE11、各种 SmartTV）在 204 状态下会有问题
    optionsSuccessStatus: 200
};

const app = express();

// body 解析中间件，负责将 body 部分的数据解析成 json 并挂载到 req.body 中
app.use(express.json());

// 跨域相关的中间件，匹配跨域协议
app.use(cors(corsOptions));

// 打印信息中间件，这样每一个请求被访问时，都会把基本信息打印到窗口上
app.use(morgan('dev'));

// 测试项目部署请看 README.md
const rootPath = '../digital-vcd-render/public';

// 路由为 localhost:3000
app.get('/', (req: Request, res: Response) => {
    res.send('<h1>Hello, World!</h1><br><img src="https://picx.zhimg.com/v2-b4251de7d2499e942c7ebf447a90d2eb_l.jpg"/>');
});

// 路由为 localhost:3000/save-view
app.post('/save-view', async (req: Request, res: Response) => {    
    try {
        const { file, payload } = req.body;
        const savePath = path.join(rootPath, file);
        const buffer = BSON.serialize(payload);
        fs.writeFileSync(savePath, buffer);
        // send 的内容可以在前端的 response 的 data 中查看
        res.send('message');
    } catch (error) {
        console.log('error happen in /save-view, ' + error);
        res.send('error')
    }
})

// 运行在 3000 端口
const PORT = process.env.PORT || 3000;
app.listen(PORT, () => {
    console.log(`Server is running on port ${PORT}`);
});
```