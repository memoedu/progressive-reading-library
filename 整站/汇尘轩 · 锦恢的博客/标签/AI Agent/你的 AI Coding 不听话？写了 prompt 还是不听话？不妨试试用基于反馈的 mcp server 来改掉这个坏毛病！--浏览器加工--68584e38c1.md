## 前言

相信大家在使用 AI Coding 进行项目维护时一定遇到过这样一个烦人的问题：明明我都在提示词里面说了，新的后端 API 在 v1 文件夹中，AI Agent 偏偏要去新建一个文件；又或者，明明我都说了，返回的不是 restful api 风格，而是我自定义范型的接口，但是 Agent 偏偏不去加，等等诸如此类明明很简单的规则，但是 Agent 偏偏不去遵守。

![](https://picx.zhimg.com/v2-c8e388449c2e528d99db05008dfb214b_1440w.jpg)

如果阁下亲手制作过 Agent 或者使用自己写好的规范（后文简称 spec）喂给 AI 后，不难发现如下的一个规律：当 spec 越来越长后，Agent 似乎只会对开头和结尾的规则更加遵守，中间的 spec 大模型会直接忽略，就好像你的头发一样从未存在过一样。

比如，当你的 spec 如下时：

```
spec1: 本项目是一个 App Route 风格的 nextjs 全栈项目。
spec2: 请使用 axios 发起和管理网络请求，而不要使用 fetch。
```

这个时候，AI Agent 会完美遵守 spec1 和 spec2，但是当你的项目逐渐复杂，确定了大量规范后：

```
spec1: 本项目是一个 App Route 风格的 nextjs 全栈项目。
spec2: 请使用 axios 发起和管理网络请求，而不要使用 fetch。
spec3: 使用 NextResponse 返回 JSON。（内容严格为 success,message,data）
...
spec200: 接口里面应该尽量不要设置 userId，因为可以通过 getServerSession() 方法获取 session，通过 session.user 或者 session.profile 来获取用户信息。
```

此时，只有 spec1\~3 和 spec198\~200 会被遵守，中间的 spec 会被直接忽略。就好像它们从未存在过一样。这可不仅仅是 AI Coding 存在的问题，当你写了一个自己的 Agent 后，prompt 写得特别长时也会出现上面的问题。

而这篇文章，则希望给还在烦躁的诸位提供一个可选项，解决自己的问题，从而心情舒畅地和好朋友在组会上打星际争霸2。

---

## 为什么会这样？自回归模型的**中间遗忘效应**

在大模型可解释领域，我们一般把这个现象称为大模型的**序位效应（serial-position effect）/ 中间遗忘效应（lost-in-the-middle）。下图摘自上个月在 arxiv 发表的一篇论文 [Lost in the Middle: An Emergent Property from Information Retrieval Demands in LLMs](https://www.alphaxiv.org/abs/2510.10276v1)，通过量化实验，证明了中间遗忘效应确实是存在的。**

![图1：LLM和人类记忆中“中间遗失”效应的比较](https://paper-assets.alphaxiv.org/figures/2510.10276v1/img-0.jpeg)

当然，[Serial Position Effects of Large Language Models](https://www.alphaxiv.org/abs/2406.15981v1) 这篇文章我也推荐的，感兴趣的读者欢迎阅读来加强对大模型底层原理的认知。

至于为什么会出现这个问题，则与注意力分布与位置偏好、后训练与预训练的数据偏差、ChatML 结构等等原因都有关系。你们只需要知道一件，那就是中间遗忘效应广泛存在于自回归架构的大模型上。所以不要还像三岁小孩一样，认为存在这个效应就是模型训练地太烂，抱歉，目前看来，这个问题是广泛存在的，最强的 gpt5 也存在这个问题。

---

## 解决思路

那么如何解决这个问题呢？我的做法是这样的：与其向 AI 提供一堆的 spec 作为 prompt 插入对话开头。比如将一部分没有模糊性的，非常明确的 spec 直接硬编码成规则库 checker，然后封装为 mcp server 或者 eslint 插件。这样 agent 在生成代码后，会使用 mcp server 里面的 checker 进行检查并获取反馈，从而继续 agent loop，最终符合你的规则地完美完成代码生成。

> 当然，封装为 eslint 也是 ok 的，因为大部分 Coding Agent 都会在生成代码结束后检查 Linter 缓冲区（也就是一个集中显示所有下面划红线报错的错误汇总，这是 LSP 协议的一部分），然后根据报错反馈继续修改代码。编写 eslint 相当于将你的规则转换成了错误信息的反馈提供给了 Coding Agent。两种方法都行，限于篇幅，本文只演示前者。

那还说啥，打开 vscode，开始演示。本次代码也开源了，可以直接体验：[LSTM-Kirigaya/ai-coding-feedback-mcp](https://github.com/LSTM-Kirigaya/ai-coding-feedback-mcp)

这次我们要完成的规范如下：NextResponse.json 响应值必须加上严格的范型定义，例子如下

错误案例 ❌

```typescript

export async function POST(req: Request) {
	...
    return NextResponse.json({
        success: true,
        message: '创建训练提醒成功',
        data: {
            reminder: data
        }
    });
}
```

正确案例 ✅

```typescript
import type { CreateTrainingReminderDto, CreateTrainingReminderResponseDto } from './entity';

export async function POST(req: Request) {
	...
    return NextResponse.json<ApiData<CreateTrainingReminderResponseDto>>({
        success: true,
        message: '创建训练提醒成功',
        data: {
            reminder: data
        }
    });
}
```

---

## 1. 创建项目

由于我演示的是 nextjs 项目的代码规范，而 nextjs 大部分都是通过 typescript 编写的，typescript 自己就有非常成熟的解析 typescript 并获取 AST 的库，所以本次教程采用 typescript 来编写 MCP 服务器。

首先创建项目：

```bash
mkdir ai-coding-feedback-mcp
cd ai-coding-feedback-mcp
git init
npm init -y
npm install @modelcontextprotocol/sdk ts-morph
npm install typescript @types/node --save
npx tsc --init
```

本次项目的基本结构如下：

```
ai-coding-feedback-mcp/
├── test/                    # 测试数据
│
├── src/                     # 源码目录
│   ├── checker.ts           # 核心逻辑：检查 TypeScript 文件并分析 API/代码问题
│   ├── server.ts            # MCP 服务的入口，注册工具、路由、handler
│   ├── utils.ts             # 通用工具函数（文件查找、AST 处理等）
│   ├── constants.ts         # 常量声明（规则名、配置键等）
│   └── index.ts             # 包入口，统一导出或连接各模块
│
├── tsconfig.json            # TypeScript 配置
```

## 2. 通过大模型将 spec 转换为 checker

我们主要的判断逻辑都写在 checker.ts 中，也是核心部分的逻辑，你可以直接通过下面的 prompt 来让大模型根据你的 spec 生成对应的 checkder 代码：

```
请根据你的 prompt.md 生成一个符合如下规定的 checker:

编写一个 AST scanning 工具 + CLI，可以输出错误报告，例如：
[error] /api/user/update/route.ts  第 8 行：未检测到 getServerSession()
[error] /api/user/update/route.ts  第 23 行：未使用 ApiData<ReturnType>
[error] /api/user/update/entity.ts 缺少 XxxDto 或 XxxResponseDto
```

> 记得把你的 spec 合集放入 [prompt.md](http://prompt.md) 中，因为隐私问题，锦恢不方便开源我的 [prompt.md](http://prompt.md)，还请见谅。

这个部分上下文很短，且 spec 本事就很凝练，大模型生成的 checker 质量往往很高。本次博客的 checker 代码在 <https://github.com/LSTM-Kirigaya/ai-coding-feedback-mcp/blob/main/src/checker.ts>，请自行查看。

然后我们可以简单进行测试：

```bash
npx tsx src/index.ts check
```

然后，就能看到如下结果：

```
❌ 检查发现以下问题：
 - [test/create-training-reminder/route.ts] 缺少 import type 引入 DTO 类型（必须来自 ./entity）
 - [test/create-training-reminder/route.ts] 返回必须是 ApiData<CreateTrainingReminderResponseDto>
 - [/Users/kirigaya/projects/ai-coding-feedback-mcp/test/create-training-session/entity.ts] entity.ts 中缺少接口 CreateTrainingSessionResponseDto
```

## 3. 封装为 mcp server

测试无误后，我们就可以把我们的 checker 封装为一个 mcp server，server.ts 代码如下：

```typescript
import { McpServer } from '@modelcontextprotocol/sdk/server/mcp.js';
import { StdioServerTransport } from '@modelcontextprotocol/sdk/server/stdio';
import { z } from 'zod';
import { checkBackendApi } from './checker';

const server = new McpServer({
    name: 'ai-coding-feedback-mcp',
    version: '1.0.0'
});

server.registerTool(
    'check_nextjs_backend_api',
    {
        title: 'Check Nextjs Backend Api',
        description: '检查 nextjs 后端 api 是否符合规范',
        inputSchema: {
            folderPath: z.string().describe('需要检查的功能模块的目录，它一般包含 route.ts 文件'),
            tsConfigFilePath: z.string().describe('tsconfig.json 文件的路径'),
        }
    },
    async ({ folderPath, tsConfigFilePath }) => {
        const issues = checkBackendApi(folderPath, tsConfigFilePath);

        return {
            content: [{ type: 'text', text: JSON.stringify(issues) }],
        };
    }
);

server.connect(new StdioServerTransport());
```

然后，我们可以在 openmcp 中对 mcp server 简单测试一下：

![图片](https://cdn3.ldstatic.com/original/4X/f/9/c/f9c0ba45a45c1fe206e0c7b4f823410760e65648.webp)

看起来没什么问题了，那么接下来，我们就可以将 mcp server 放入 Coding Agent 中了。

## 4. 在 AI Coding 工具中安装 MCP 服务器

几乎所有主流的 AI Coding 都支持 MCP 服务器（Cursor，Claude Code，Qwen Coder，Trae），我此处时使用的是 qwen coder 的 vscode 插件版，lingma，在 vscode 插件商城搜索「**Lingma - Alibaba Cloud AI Coding Assistant**」就能搜到，安装后，使用支付宝登录即可免费使用。

在 lingma 的窗口，点击「MCP 工具」进入 MCP 工具合集中。

![](https://pic3.zhimg.com/v2-b4b75e9497993370703611c6e510f32c_r.jpg)

然后点击右上角的 +，选择手动添加，填入如下参数，记得适配你自己的路径：

![](https://pic3.zhimg.com/v2-150bcee270e7c6cb2847611576ceb004_r.jpg)

> 还有一种非常简单的方法，那就是利用 openmcp 的导出配置功能，直接复制后选择 lingma 右上角的 + 的配置文件添加，把复制的配置文件粘贴进去就行。openmcp 导出配置的功能，请看我们准备的文档：[openmcp 文档 | 导出 mcp 服务器](https://openmcp.kirigaya.cn/zh/plugin-tutorial/usage/export-mcp.html#%E7%AC%AC%E4%B8%80%E6%AD%A5-%E5%AF%BC%E5%87%BA-mcpconfig-json)

## 5. 开始享受！

准备就绪后，我们询问 lingma 一个问题：「请帮我规范和修改 test 文件夹下的内容」

你会发现，lingma 会调用我们的 mcp 进行检查，并得到错误的反馈：

![](https://picx.zhimg.com/v2-7c4b725a6273261b45a697d3d3fe3b01_r.jpg)

然后 lingma 就会基于我们的 checker 给出的错误反馈最终成功修改代码成我们想要的样子：

![](https://pic2.zhimg.com/v2-847f9683edce036802c4aa04df9bf91d_r.jpg)

而且由于是基于 checker 的，spec 再长，中间遗忘效应也会被缓解很多。

---

## 结语

怎么说？看完后，小登们学到新东西了，老登们的 OKR 又有着落了，简直就是完美结局呀！

![](https://pic4.zhimg.com/v2-f95406f7aac6571edd83062fac02160f_r.jpg)

其实归根到底，本次介绍的方法本质就是「基于反馈的 AI Agent 调试方法」，我也会在我的「锦恢的 AI Agent 小白教程」系列里详细讨论的，感兴趣的话，不妨点个关注吧！