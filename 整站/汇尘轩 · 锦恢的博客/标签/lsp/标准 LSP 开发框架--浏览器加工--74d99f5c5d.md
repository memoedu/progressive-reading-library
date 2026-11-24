## 前言

LSP 全套服务的开发是技术难度很高的一件事，即便在如今 vscode 已经建立了 lsp 协议框架的前提下，这个过程仍然不简单。我也只是记录对我而言比较有用的常用流程。这是一篇有关 vscode 中开发有关一个编程语言的lsp的最小需求框架。

---

## 基本流程

下面以 vhdl 编程语言 为例子，演示编程语言 lsp 最小实现系统。

### 创建插件

```bash
yo code
```

建议使用 typescript 作为前端开发语言。



### 插件激活事件

```json
    "activationEvents": [
        "onLanguage:vhdl"
    ],
```

### 定义编程语言

```json
    "contributes": {
     	"languages": [
            {
                "id": "vhdl",
                "aliases": [
                    "VHDL",
                    "vhdl"
                ],
                "extensions": [
                    ".vhd",
                    ".vhdl",
                    ".vho",
                    ".vht"
                ],
                "configuration": "./config/vhdl.configuration.json",
                "icon": {
                    "dark": "./images/svg/dark/vhdl.svg",
                    "light": "./images/svg/light/vhdl.svg"
                }
            }
        ],   
    }
```

`configuration`：编程语言配置，包括定义括号闭合、注释等等，这些操作也可以放在后续更加复杂的 LSP 中实现，但是此处实现不仅简单，vscode 内部还有优化。一个例子如下：

```json
{
    "comments": {
       "lineComment": "--",
        "blockComment": [ "/*", "*/" ]
    },
    "brackets": [
        ["{", "}"],
        ["[", "]"],
        ["(", ")"]
    ],
    "autoClosingPairs": [
        ["{" , "}"],
        ["[" , "]"],
        ["(" , ")"],
        ["'" , "'"],
        ["`" , "`"],
        ["\"", "\""]
    ],
    "surroundingPairs": [
        ["{", "}"],
        ["[", "]"],
        ["(", ")"],
        ["\"", "\""],
        ["'", "'"]
    ]
}
```

两个 svg 代表黑色和白色主题下的编程语言文件图标：

```html
<?xml version="1.0" standalone="no"?>
<!DOCTYPE svg PUBLIC "-//W3C//DTD SVG 1.1//EN" "http://www.w3.org/Graphics/SVG/1.1/DTD/svg11.dtd">
<svg t="1671092287106" class="icon" viewBox="0 0 1024 1024" version="1.1"
    xmlns="http://www.w3.org/2000/svg" p-id="1872" data-darkreader-inline-fill=""
    xmlns:xlink="http://www.w3.org/1999/xlink" width="200" height="200">
    <path
        d="M785.51004562 785.51004562H238.43817245V238.43817245h547.07187317m218.82875024 218.82874902V347.85254756h-109.41437511V238.43817245c0-60.7397357-49.19245392-109.41437511-109.41437513-109.41437513h-109.41437391V19.60942341h-109.41437512v109.41437391h-109.41437512V19.60942341H347.85254756v109.41437391H238.43817245c-60.7397357 0-109.41437511 48.67463941-109.41437513 109.41437513V347.85254756H19.60942341v109.41437391h109.41437391v109.41437512H19.60942341v109.41437512h109.41437391v109.41437391a109.41437511 109.41437511 0 0 0 109.41437513 109.41437513H347.85254756v109.41437511h109.41437391v-109.41437511h109.41437512v109.41437511h109.41437512v-109.41437511h109.41437391a109.41437511 109.41437511 0 0 0 109.41437513-109.41437513v-109.41437391h109.41437511v-109.41437512h-109.41437511v-109.41437512m-328.24312416 109.41437512h-109.41437512v-109.41437512h109.41437512m109.41437512-109.41437391H347.85254756v328.24312415h328.24312415z"
        fill="#ff7043" p-id="1873" data-darkreader-inline-fill=""
        style="--darkreader-inline-fill:#000000;"></path>
</svg>
```

效果如下：

<!DOCTYPE svg PUBLIC "-//W3C//DTD SVG 1.1//EN" "http://www.w3.org/Graphics/SVG/1.1/DTD/svg11.dtd">
<svg t="1671092287106" class="icon" viewBox="0 0 1024 1024" version="1.1"
    xmlns="http://www.w3.org/2000/svg" p-id="1872" data-darkreader-inline-fill=""
    xmlns:xlink="http://www.w3.org/1999/xlink" width="200" height="200">
    <path
        d="M785.51004562 785.51004562H238.43817245V238.43817245h547.07187317m218.82875024 218.82874902V347.85254756h-109.41437511V238.43817245c0-60.7397357-49.19245392-109.41437511-109.41437513-109.41437513h-109.41437391V19.60942341h-109.41437512v109.41437391h-109.41437512V19.60942341H347.85254756v109.41437391H238.43817245c-60.7397357 0-109.41437511 48.67463941-109.41437513 109.41437513V347.85254756H19.60942341v109.41437391h109.41437391v109.41437512H19.60942341v109.41437512h109.41437391v109.41437391a109.41437511 109.41437511 0 0 0 109.41437513 109.41437513H347.85254756v109.41437511h109.41437391v-109.41437511h109.41437512v109.41437511h109.41437512v-109.41437511h109.41437391a109.41437511 109.41437511 0 0 0 109.41437513-109.41437513v-109.41437391h109.41437511v-109.41437512h-109.41437511v-109.41437512m-328.24312416 109.41437512h-109.41437512v-109.41437512h109.41437512m109.41437512-109.41437391H347.85254756v328.24312415h328.24312415z"
        fill="#ff7043" p-id="1873" data-darkreader-inline-fill=""
        style="--darkreader-inline-fill:#000000;"></path>
</svg>



### textmate 语法高亮

```json
    "contributes": {
        "grammars": [
            {
                "language": "vhdl",
                "scopeName": "source.vhdl",
                "path": "./syntaxes/vhdl.tmLanguage.json"
            }
        ]
    }
```

vhdl.tmLanguage.json 遵循 textmate 语法定义词法级高亮。更加复杂的语义级高亮需要在 LSP 中实现。




### LSP 客户端

vscode 显然是 LSP 协议栈中的客户端，它的客户端框架为 `vscode-languageclient/node`

```bash
npm install vscode-languageclient
```

:::

如果出问题了，回退到 7.0.0 版本

:::

创建 LSP 客户端流程如下

```typescript
import { platform } from 'os';
import * as vscode from 'vscode';
import * as path from 'path';

import {
    LanguageClient,
    LanguageClientOptions,
    ServerOptions,
    Executable,
} from "vscode-languageclient/node";

let client: LanguageClient;

function getLspServerExecutionName() {
    // 返回各个平台上编译好的二进制文件文件名，这是 LSP 服务端的进程文件
    return 'server-name';
}

export function activate(context: vscode.ExtensionContext) {
    const lspServerName = getLspServerExecutionName();
    
    // server 相对于插件根目录的路径
    const lspServerPath = context.asAbsolutePath(
        path.join('server', lspServerName)
    );
    const run: Executable = {
        command: lspServerPath,
    };
	
    // 配置两种模式，run 和 debug，F5 调试模式下只会调用 debug 的 配置
    // 如果采用 rust 开发服务端，则 [cargo build]--> debug, [cargo build --release] --> run
    // 此处为了方便，run 和 debug 都选择上面那个
    let serverOptions: ServerOptions = {
        run,
        debug: run,
    };

    let clientOptions: LanguageClientOptions = {
        documentSelector: [
            {
                scheme: 'file',
                language: 'vhdl' 
            }
        ],
    };

    client = new LanguageClient(
        "VHDL LSP",      // 客户端 id
        "VHDL LSP",      // vscode OUTPUT 窗口的名字，服务端的 LOG IO 流会被定向到 OUTPUT 窗口，方便进行调试和记录
        serverOptions,
        clientOptions
    );

    client.start();
}

export function deactivate() {
    if (!client) {
        return undefined;
    }
    client.stop();
}
```



