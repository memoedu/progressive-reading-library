在上一节中，我们简单配置了一个功能齐全的 nvim ，这一节，将简单记录剩下的一个很重要的技术，那就是 LSP 。考虑到笔者的开发习惯，本文中只会讲解我平时常用的几种编程语言的 LSP 安装方法，其实大同小异，如果没有 cover 到阁下需要使用的编程语言，读者可以参考 nvim-lspconfig 官方给出的[配置大全](https://github.com/neovim/nvim-lspconfig/blob/master/doc/server_configurations.md)，这个文件中列出了所有目前官方支持的 LSP 和对应的安装方法。


## LSP 的定义

什么是 LSP ？ LSP 是 language server protocol 的缩写，倘若阁下有过在 vscode ， Pycharm 这样的编辑器上打代码的经验，那么你大可认为 LSP = 语法高亮 + 自动补全 + 悬停提示 + 定义跳转 + 语法纠错等有关编程语言语法服务的功能组件。

在上一节中，我们已经通过 nvim tree-sitter 完成了对于 特定语言 的语法高亮设置。

在这一节中，我将简单罗列常用编程语言的 nvim-lsp 的安装方法，读者可以随时来查阅。

---

## 准备

- 按照上一节配置完成的 nvim
- 带有 npm 的 nodejs（没有的请移步 [Download Nodejs](https://nodejs.org/en/download)）

---

## Python

Python 作为最常用的编程语言，无论是深度学习，数据分析，还是网络爬虫，都有 Python 的影子。想要配置 python 的 nvim lsp，首先你需要先安装 python。打开命令行，安装 `pyright`：

```bash
$ npm i -g pyright
```

安装结束后，可以先初始化 `pyright`：

```bash
$ pyright
```

然后进入 nvim 配置目录（Windows是 `$env:LOCALAPPDATA/nvim`，Linux 是 `~/.config/nvim`），打开 `./lua/custom/configs/lspconfig.lua`，修改 `servers ` 变量：

```lua
local servers = { 
    "pyright"
}
```

退出后，进入一个 含有 python 文件的项目，打开 python 文件，敲入文字后，稍等片刻，即可完成激活。

<div align=center>
<img src="https://picx.zhimg.com/80/v2-7b1122f3bbb6c18c604d9eae2bbf9335_1440w.png" style="width: 80%;"/>
</div>

别忘了更新语法高亮：

```vim
:TSInstall python
```

---

## C++

最近在用 C++ 作为 source 写 wasm，简单配置一下吧！

我们此处使用 clangd 作为 C++ 的 LSP ，先进入 `https://github.com/clangd/clangd/releases` ，找到符合你操作系统的版本，并完成下载，比如：

```bash
# windows
$ wget https://kirigaya.cn/files/windows_tools/clangd-windows-16.0.2.zip
# Linux
$ wget -c https://kirigaya.cn/files/linux_tools/clangd-linux-16.0.2.zip
```

下载，解压，并将其中的 bin 文件夹路径加入环境变量，加入后测试：

```bash
$ clangd
```

没有报错说明成功。

然后和上面一样，打开 `./lua/custom/configs/lspconfig.lua`，修改 `servers ` 变量：

```lua
local servers = { 
    "pyright",
    "clangd"
}
```
保存退出后，打开一个 C++ 项目，测试：

<div align=center>
<img src="https://picx.zhimg.com/80/v2-db93355c1783b70bc79c8efb4dbc3dfb_1440w.png" style="width: 80%;"/>
</div>

别忘了更新语法高亮：

```vim
:TSInstall cpp
```

---

## JavaScript / TypeScript

我现在写网站主打就是 typescript， TS 是必须配置的。先安装 LSP ：

> 这套方案在 Javascript 上面也是生效的

```
npm install -g typescript-language-server typescript
```


修改 `servers ` 变量：

```lua
local servers = { "pyright", "clangd", "tsserver" }
```

更新一下 TS 和 JS 的语法高亮：

```vim
:TSInstall typescript
:TSInstall javascript
```

打开一个 typescript 项目，测试：

<div align=center>
<img src="https://pic1.zhimg.com/80/v2-08896de7243c60fbb268a7659ff1037d_1440w.png" style="width: 80%;"/>
</div>

---

## Go

Go 算是现在非常常用的非图形界面可执行文件源语言，因为真的很香。而且我的网站后端就是 Go 写的，对于一个喜欢直接上服务器改代码的人，我没有理由不加一个 Go 的 LSP。

首先，你当然需要一个 Go 环境，这个按照 Go 官网傻瓜式安装即可。然后安装强大的 Go LSP，也就是你在 vscode 上使用的 gopls：

```bash
$ go install golang.org/x/tools/gopls@latest
```

修改 `servers ` 变量：

```lua
local servers = { "pyright", "clangd", "tsserver", "gopls" }
```

在 nvim 下安装 Go 的语法高亮：

```vim
:TSInstall go
```

进入一个 Go 的项目进行测试：


<div align=center>
<img src="https://picx.zhimg.com/80/v2-9a895548a3093e04433b25f29e2cec2f_1440w.png" style="width: 80%;"/>
</div>

---

## Vue

作为一个业余网站开发者，怎么能少了 vue 呢？立马安装一下 vue 的 LSP：

```bash
# html / css 的 自动补全
$ npm install -g vscode-html-languageservice

# js / ts 的自动补全
$ npm install -g typescript-language-server

# vue 的 自动补全
$ npm install -g @vue/language-server
```

先引入 html 和 css 的语言服务定义：
```lua
local servers = {
    "html",
    "cssls",
    "tsserver",
    "clangd",
    "pyright",
    "gopls"
}
```

vue 的 LSP 配置需要一些额外的参数，我们就不放在上面的列表里面了，我们单独拿出来：

```lua
-- local lspconfig = require "lspconfig"
lspconfig.volar.setup {
    filetypes = { 'javascript', 'typescript', 'javascriptreact', 'typescriptreact', 'vue', 'json'}
}
```

进入一个 vue 项目，然后再局部安装一个 typescript (这个混蛋项目还依赖于 typescript 里面的几个js文件)：

```bash
$ npm install --save typescript
```

进入后就能正常工作了：

<div align=center>
<img src="https://picx.zhimg.com/80/v2-b36338a3945875514045639a0c317078_1440w.png" style="width: 80%;"/>
</div>


---

## 总结

最后再说一句，我个人使用 neovim 仅仅是作为 vim 的上位替代，而不是替代 vscode， neovim 只是一个终端应用， LSP 的优化做得不如 vscode 的，所以开发体验上手感有点稍稍的阻尼感（当然，很多人非常喜欢）。 vscode 仍然会是我的主力开发工具。

那么为什么要入坑 neovim 呢？因为有时候我希望可以直接在服务器、板子、机器人上进行代码修改或者小型模块开发。此时 vscode 显得有点太慢了。总之，找到自己喜欢的开发模式就好。