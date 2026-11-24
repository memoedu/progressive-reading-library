<center>
<iframe frameborder="no" border="0" marginwidth="0" marginheight="0" width=330 height=86 src="//music.163.com/outchain/player?type=2&id=2013726546&auto=1&height=66"></iframe>
</center>

## 前言

最近想写一个增量解析器用于拓展 vcd 的一些标准，考虑到 vcd 这样的波形记录语言和 http 报文一样，都是满足上下文无关文法（CFG）的格式，于是想着简单用用 LL 文法进行解析算了。

> 所谓上下文无关文法，说人话就是，每一行或者每一个我们称之为生成式的表达式的左值可以随意替换，也就是这一行的变量申明不需要关心上下文的其他表达式，你可以把这一行的位置随意替换，但是形式语言的含义仍然不变。比如 json 在大部分情况下就是上下文无关语法；但是C语言，JavaScript，python 这些图灵完备的编程语言基本都不是上下文无关文法的。
>
> 如果这么说你还是听不懂，可以出门左拐重修编译原理了。

LL 文法是用于处理 CFG 的一种方式，开发起来相对简单，关于 LL 文法构造的 LL 分析器的定义，可以看这里：[LL 分析器](https://zh.wikipedia.org/wiki/LL%E5%89%96%E6%9E%90%E5%99%A8)。

作为 nodejs 官方维护的一个项目，`llparse` 可以说是非常简单的用于生成解析器的一个框架。准确说，当你使用 `llparse` 描述了一个解析器的基本语法后， `llparse` 会生成逻辑同构的 C语言的源文件和头文件，然后用户可以自己再用C或者C++编写胶水文件，然后编译成exe、wasm或者dll以供不同软件、运行时使用。

而且 `llparse` 支持增量解析，它生成的 C语言代码编译后的结果可以每次只输入需要解析的形式语言的一小部分（比如114514个字节），然后再解析，这样编译出来的解析器解析超大文件也不会造成内存不足的情况。

我发现这么好用的文法框架全网居然没有任何教程，于是简单写点，以供后人参考。

---

## 原料

为了进行本文章中的实验，请事先安装好如下软件：

- gcc
- nodejs

---

我不太擅长写编译原理方向的博文，所以我直接从例子出发讲解了。

## 创建项目

首先先确保你的电脑上已经安装了 nodejs ，此处使用 `yarn` 作为包管理器，没有安装的同学：

```bash
npm install yarn -g
```

然后我们新建一个项目并下载 `llparse` 这个库：

```bash
mkdir -p ~/code/nodejs/llparse
cd ~/code/nodejs/llparse
yarn; yarn add llparse
```

## 运行你的 hello world

`llparse` 官方提供了一个简单的例子用于解析 http1.1 的请求报文的请求行部分，我稍作修改，变成了更加符合本期教程的模样，复制下方代码到 `main.js`：

```javascript
const { LLParse } = require('llparse');
const fs = require('fs');

const prjName = 'http_parser';
const p = new LLParse(prjName);

const method = p.node('method');
const beforeUrl = p.node('before_url');
const urlSpan = p.span(p.code.span('on_url'));
const url = p.node('url');
const http = p.node('http');

// Add custom uint8_t property to the state
p.property('i8', 'method');

// Store method inside a custom property
const onMethod = p.invoke(p.code.store('method'), beforeUrl);

// Invoke custom C function
const complete = p.invoke(p.code.match('on_complete'), {
  // Restart
  0: method
}, p.error(4, '`on_complete` error'));

method
  .select({
    'HEAD': 0, 'GET': 1, 'POST': 2, 'PUT': 3,
    'DELETE': 4, 'OPTIONS': 5, 'CONNECT': 6,
    'TRACE': 7, 'PATCH': 8
  }, onMethod)
  .otherwise(p.error(5, 'Expected method'));

beforeUrl
  .match(' ', beforeUrl)
  .otherwise(urlSpan.start(url));

url
  .peek(' ', urlSpan.end(http))
  .skipTo(url);

http
  .match(' HTTP/1.1\n', complete)
  .otherwise(p.error(6, 'Expected HTTP/1.1 and two newlines'));

const artifacts = p.build(method);
fs.writeFileSync(prjName + '.c', artifacts.c);
fs.writeFileSync(prjName + '.h', artifacts.header);
```

你暂时不需要看懂代码的含义，我们先运行，输入：

```bash
node main.js
```

运行成功后，你应该会看到目录下生成了两个新的文件：

```bash
http_parser.c
http_parser.h
```

后续，我们可以根据这两个文件来解析任意的http报文的开头，既然是C语言写的，自然可以非常方便地编译成如下几种运行时或用途：

- 可执行文件（exe）
- 给 web 加速（wasm）
- 给 python 编写 API（pyd）

我们现在演示一个简单的，也就是编译成可执行文件。我们先创建需要验证的 http 报文 `http.txt`：

```
GET /index.html HTTP/1.1
Host: www.example.com
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/97.0.4692.99 Safari/537.36
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,image/apng,*/*;q=0.8,application/signed-exchange;v=b3;q=0.9
Accept-Encoding: gzip, deflate, br
Accept-Language: en-US,en;q=0.9
Connection: keep-alive
```

> http的请求和响应报文格式在 [MDN 的文档](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Messages) 说得很详细了。

然后我们自己写一个代码来调用 llparse 生成的解析器库：

```c
// main.c
#include "stdio.h"
#include "stdlib.h"
#include "string.h"
#include "http_parser.h"

#define CACHE_SIZE 200
char buffer[CACHE_SIZE * 2];

http_parser_t http_result;

char HTTP_METHOD[10][10] = {
    "HEAD", "GET", "POST", "PUT",
    "DELETE", "OPTIONS", "CONNECT",
    "TRACE", "PATCH"
};

int on_url(http_parser_t* s, const unsigned char* p, const unsigned char* endp) {
    printf("url: ");
    for (unsigned int i = 0; (p + i) != endp; ++ i) {
        printf("%c", *(p + i));
    }
    printf(", ");
    return 0;
}

int on_complete(http_parser_t* s, const unsigned char* p, const unsigned char* endp) {
    if (s->error > 0) {
        printf("error code: %d, reason: %s, position: %s\n",
                s->error, s->reason, s->error_pos);
    } else {
        printf("method: %s\n", HTTP_METHOD[s->method]);
    }
    return 0;
}

int main(int argc, char const *argv[]) {
    if (argc >= 2) {
        // 初始化全部置为 0
        http_parser_init(&http_result);
        memset(buffer, 0, CACHE_SIZE * 2);

        FILE *file = fopen(argv[1], "r");
        if (file != NULL) {
            while (fread(buffer, 1, CACHE_SIZE - 5, file) > 0) {
                buffer[CACHE_SIZE - 4] = '\0';
                http_parser_execute(&http_result, buffer, buffer + CACHE_SIZE - 1);
            }
        }

        fclose(file);
    }
    return 0;
}
```

然后编译运行：

```bash
# windows
gcc -o parser http_parser.c main.c; ./parser.exe http.txt

# linux
gcc -o parser http_parser.c main.c && ./parser http.txt
```

输出：

```
url: /index.html, method: GET
```

可以看到，解析器达到了我们的预期。

## llparse 基础数据结构

下面就要开始介绍 llparse 了，请抛开你本科学的那一套用于描述文法关系的形式语言，我们这里为了更加简单地进行开发，往往会将 LL 文法的最左推导描述成一个状态机，也就是一个流程图。

作为流程图最基本的描述单元，我需要先给如下四种节点进行定义，它们也对应着 llparse 在 nodejs 端的类或者函数：

<div align=center>
<img src="https://picx.zhimg.com/80/v2-c3172751f41e6411c8ea8cb910b6d84d_1440w.png" style="width: 80%;"/>
</div>

如果 `node.Match` 作为参数被输入到一个操作内，往往代表该操作结束后跳转到这个节点。

比如 `main.js` 用于描述 http 版本号字段的代码：

```javascript
http
  .match(' HTTP/1.1\n', complete)
  .otherwise(p.error(6, 'Expected HTTP/1.1 and two newlines'));
```

这段代表 http 节点先进行 `match` 匹配，如果成功了，进入 `complete` 节点（是一个 `Invoke`）；否则，进入 `NodeError` 节点并停止解析。

显然，`otherwise` 是否则的意思，类似于 `else`，可以理解为对于异常的捕获。

除了最常用的 `match`，llparse 还提供了 `select` 和 `peek` 匹配操作，它们的说明如下：

1. **select 方法：** `select` 方法用于在当前节点根据输入的不同值选择不同的转换路径。这些值与转换目标节点或动作相关联。例如，在 HTTP 解析器中，可以根据输入的 HTTP 方法名选择不同的路径。
2. **match 方法：** `match` 方法用于在当前节点尝试匹配输入的固定字符串。如果匹配成功，解析器将转移到指定的目标节点。这在解析固定格式的字符串时非常有用，比如在 HTTP 解析器中匹配 HTTP 协议版本字符串。
3. **peek 方法：** `peek` 方法类似于 `match` 方法，但它只是查看输入的字符串而不消耗它。这允许解析器检查输入的一部分而不将解析器的位置移动到下一个字符。通常，`peek` 方法结合 `span` 方法使用，用于确定某些标记的位置而不影响解析器的状态。

## main.js 状态机

我们再结合 `main.js` 的片段讲解一下 `select`，它其实就是 `switch`：

```javascript
p.property('i8', 'method');

const onMethod = p.invoke(p.code.store('method'), beforeUrl);

method
  .select({
    'HEAD': 0, 'GET': 1, 'POST': 2, 'PUT': 3,
    'DELETE': 4, 'OPTIONS': 5, 'CONNECT': 6,
    'TRACE': 7, 'PATCH': 8
  }, onMethod)
  .otherwise(p.error(5, 'Expected method'));
```

编译成的 C 会先进入 `method` 节点，然后进行 `select` 操作，C语言会尝试使用当前位置往后字符串的前缀去匹配 HEAD, GET, POST 这些字符串，并将它们对应的数字作为返回值返回。如果这个过程成功匹配到了，则进入 `onMethod` 节点，否则，进入 `NodeError` 节点报错退出解析。

进入 `onMethod` 节点后，发现这是一个 `Invoke`，便执行内部的代码片段，此处的代码片段为：

```javascript
p.code.store('method')
```

它代表将上一个节点的返回值赋值给C语言中申明好的一个名为 `method` 的变量，改变了的申明在第一行，是一个 uint8 （i8 是 LLVM IR 中对于 uint8 的描述，其他的数据类型自行查阅 LLVM IR）：

```c
// JavaScript 中
p.property('i8', 'method');

// 在 C语言中会被翻译为
struct http_parser_s {
  	// ...
	uint8_t method;
  	// ...
};
```

uint8 赋值这个过程是不可能出错的，所以不做 `otherwise` 判断了，直接进入 `beforeUrl` 节点。

怎么说，是不是很简单？我把完整的LL 文法流程图画出来了，如下所示：

<div align=center>
<img src="https://picx.zhimg.com/80/v2-340fbceda31de4f1047618d796d9424e_1440w.png" style="width: 80%;"/>
</div>

有了这张图，想要如法炮制制作其他的形式语言的 nodejs 描述也不是难事了。

---

## llparse-dot 可视化

llparse 有一个配套的可视化工具 `llparse-dot`，你可以借助它来进行 debug，首先安装一下：

```
npm install llparse-dot
```

该工具借助著名的可视化工具 Graphviz 进行渲染，你还需要安装 dot 下载链接 [Graphviz](https://graphviz.org/download/)

在刚刚的 `main.js` 的结尾加上这么几行：

```javascript
// main.js
const { Dot } = require('llparse-dot');
const llDot = new Dot();
// 输入根节点 method
const code = llDot.build(method);
fs.writeFileSync('http_parser.dot', code, { encoding: 'utf-8' });
```

重新运行 `main.js` 会生成 `http_parser.dot` 绘图脚本，然后通过 dot 进行渲染：

```bash
# 输出 png 文件
dot -Tpng .\http_parser.dot -o http_parser.png

# 输出 svg 文件
dot -Tsvg .\http_parser.dot -o http_parser.svg
```

渲染效果：

<div align=center>
<img src="https://pica.zhimg.com/80/v2-ae70fcc05975abc1e9f43b7ecd4e1ed6_1440w.png" style="width: 80%;"/>
</div>