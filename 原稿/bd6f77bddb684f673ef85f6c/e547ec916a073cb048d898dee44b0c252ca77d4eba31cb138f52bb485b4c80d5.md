## 前言

最近在准备Digital-IDE 0.3.0的发布，只差文档需要更新了，因此接触了夜云之前做的docsify文档。我一开始还很惊讶，因为我曾经用过一些静态文档，类似于vuepress这样的，我原本以为会是一个可编译的web项目，但是docsify的工程里面只有一个index.html和一个README.md。对于单页面的文档而言，实在是太优雅了。所以这篇博客简单记录一下docsify的极简使用。

构建一个基于docsify的无路由文档，只需要四个文件：


- index.html：生成文档html的文件
-  _coverpage.md：配置文档封面
- README.md：文档主体内容
- config.md：配置，默认情况下可以什么都不写

---

## index.html

首先直接准备如下的html文件。

```html
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <title>Document</title>
    <meta http-equiv="X-UA-Compatible" content="IE=edge,chrome=1" />
    <meta name="description" content="Description">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, minimum-scale=1.0">
    <link rel="stylesheet" href="//cdn.jsdelivr.net/npm/docsify@4/lib/themes/vue.css">
</head>

<body>
    <div id="app"></div>
    <script>
        window.$docsify = {
            name: '',
            repo: ''
        }
        // 添加全文搜索的配置
        window.$docsify = {
            search: {
                placeholder: '搜索',
                noData: '找不到结果!',
                depth: 3
            },
        }
        // 添加封面
        window.$docsify = {
            onlyCover: false,
            coverpage: true
        }
    </script>
    <script src="//unpkg.com/docsify/lib/docsify.min.js"></script>
    <script src="//unpkg.com/docsify/lib/plugins/search.js"></script>
    <!-- 添加代码高亮的配置  -->
    <script src="//cdn.jsdelivr.net/npm/prismjs@1/components/prism-json.min.js"></script>
    <script src="//cdn.jsdelivr.net/npm/prismjs@1/components/prism-vhdl.min.js"></script>
    <script src="//cdn.jsdelivr.net/npm/prismjs@1/components/prism-verilog.min.js"></script>
    <!-- 添加一键拷贝代码的配置 -->
    <script src="//unpkg.com/docsify-copy-code"></script>
</body>

</html>
```

## _coverpage.md

`_coverpage.md`用于配置封面：

```plaintext
<!-- _coverpage.md -->

![logo](https://s2.loli.net/2022/01/04/t1DSl2Io85GEQV3.png)

# 产品名字 <small>x.x.x</small>

> 文档的 xxx 介绍

- 本文档用于 xxx

[GitHub](https://github.com/Bestduan/Digital-IDE)
[Get Started](#快速开始)
```

## README.md

`README.md`里面就写上你的文档即可，语法和标准的markdown是一样的。


```plaintext
# 这是一个文档标题

## 安装
...

## 使用
....

## 开发人员
...
```

然后，你可以使用vscode的live server插件，或者直接是应用http server这样的命令行工具开启前端服务，就可以看到最终的效果了。