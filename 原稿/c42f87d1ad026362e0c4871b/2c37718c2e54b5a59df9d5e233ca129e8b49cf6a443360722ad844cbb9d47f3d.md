<div align='center'>
<iframe
height=86 width=330
src="//music.163.com/outchain/player?type=2&id=412247172&auto=1&height=66"
></iframe>
<p class="img-caption">Cavatina (From "The Deer Hunter")</p>
</div>

## 为什么我还需要MPA？

虽然我已经成功实现了自己的博客系统，但是我发现因为网上有很多的资料和文档都是直接写的具有依赖关系的markdown文件，虽然我的博客系统能够非常丝滑地完成markdown的渲染和上传，但是遇到存在多级依赖关系的文档或者书本，就显得不大好用了。因此，一个可以把任何嵌套结构的markdown文档项目编译成MPA的静态资源站点显得非常有必要搭建了。

> 我曾经还觉得没必要来着。。。

网上也不少的静态页面构建项目：`docsify`, `gitbook`, `vuepress`。我最终采用了`vuepress`主题，`vuepress-reco`作为构建方案，原因如下：

- `docsify`对于复杂依赖关系无法解析，而且有的页面解析出错，总之就是bug太多，几乎不能用。
- `gitbook`作为老牌文档构建工具，已经停止维护，使用新版`nodejs`的`npm`安装非常困难，网上大部分教程根本用不了。
- `vuepress`虽然也是依托答辩，但是生态还行，而且基于`vuepress`二次开发的主题很多，这些主题都有对应的文档，比如我使用的这个`vuepress-reco`
  - GitHub链接：[vuepress-reco/vuepress-theme-reco: 🔥 The 2.x of vuepress-theme-reco. (github.com)](https://github.com/vuepress-reco/vuepress-theme-reco)
  - Doc链接：[vuepress-reco (recoluan.com)](http://v2.vuepress-reco.recoluan.com/)

> 当然，实际开发和部署的`vuepress`本质上还是一个SPA
>
> 需要注意的是，MPA范式下，可拓展性很差，别费力气去定制化一个可玩度极高的MPA，它的作用就是文档。

---

## 安装与项目初始化

通过下面的语句全局安装`reco`的脚手架。

```bash
npm install @vuepress-reco/theme-cli@1.0.7 -g
```

初始化项目，如果是要在当前文件夹下开辟一个新的项目，第一个选项就选择yes，最后选择`2.X`，这样功能全一些。：

```bash
theme-cli init
```

我的选择如下：

```bash
? Whether to create a new directory? Yes
? What's the name of new directory? test
? What's the title of your project?
? What's the description of your project?
? What's the author's name?
? What style do you want your home page to be?(The 2.x version is the alpha version) 2.x
\ [1/1] Load file from git
```



进入创建的项目后：

```bash
npm i
```



### 开发模式

```bash
npm run dev
```

运行成功如下：

```bash
> vuepress-theme-reco-demo@2.0.0 dev
> vuepress dev .

⠙ Initializing and preparing dataNo character metrics for 'ϝ' in style 'Main-Regular'
No character metrics for 'ϝ' in style 'Main-Regular'
No character metrics for 'ϝ' in style 'Main-Regular'
✔ Initializing and preparing data - done in 451ms

  vite v4.0.4 dev server running at:

  ➜  Local:   http://localhost:8080/
  ➜  Network: http://180.109.137.210:8080/
  ➜  Network: http://122.96.110.53:8080/
```

默认界面如下：

<div align=center>
<img src="https://pica.zhimg.com/80/v2-c36f8ada28770b2ba545fb275c7fa633_1440w.png" style="width: 70%;"/>
<p class="img-caption">vuepress-reco默认启动界面</p>
</div>



### 部署

```bash
npm run build
```

生成的可部署SPA在`.vuepress/dist`中，将这个`dist`上传到服务器，然后改写一下`nginx`配置就好了。

---

## 比较重要的几个文件

在了解如何使用`vuepress`部署文档前，一定要知道几个比较重要的文件和文件夹：

- `.vuepress/config.ts`：博客配置文件，可以参考生成的样例。
- `.vuepress/public`：全局静态资源文件夹，如果你在markdown配置中看到了`/xxx.png`或者`/xxx.jpg`，那么说明这个地方请求的`xxx`文件在`.vuepress/public`下，把对应的静态资源放进来即可。
- `blogs`：所有的博客需要放在这里。
- `docs`：所有的文档需要放在这里。
- `README.md`：博客的入口，你的站点的首页内容在这里定义。



### vuepress markdown文件渲染和访问的基本规则

1. 首页Home是根据`README.md`渲染的。
2. 任何在当前项目中的markdown文件都可以通过相对路径+`文件名`+`.html`的方式访问，比如，你在项目根目录下创建了`./test/cate/hello.md`，开发模式下就可以通过`localhost:8080/test/cate/hello.html`的方式访问到。但是我仍然强烈建议你只将markdown写在`blogs`或者`docs`文件夹下。否则很多插件会失效。
3. 假设你访问了`/xxx/xxx/`，没有指定具体的html文件，那么默认会去访问`/xxx/xxx/README.md`，这也通常被用于制作子文档或者子篇章的首页。



### 如何部署自己的博客？

默认情况下，由于`README.md`开了`Blog`插件：

```yaml
---
home: true
modules:
  - BannerBrand		# 在本页面可以开启头图（这个模式下，无法显示左侧目录）
  - Blog			# 在本页面显示blogs中的内容
  - MdContent
  - Footer			# 显示页脚链接，我觉得不好看，一般选择关闭
---
```

所以会在首页下自动渲染`blogs`这个文件夹下的所有markdown文件，渲染的标签页的名字是由markdown的YAML Front的title属性指明的。

<div align=center>
<img src="https://picx.zhimg.com/80/v2-499c3daa90fd2e985ee384c30d39ed99_1440w.png" style="width: 70%;"/>
<p class="img-caption">vuepress-reco Blog插件显示列表</p>
</div>


当然，这是有要求的：

1. 这个markdown文件是在`blogs`文件夹的文件夹下，比如`blogs/test.md`就不会被渲染到blog列表中，但是`blogs/test/test.md`就会被渲染到上方的列表中。
1. 路径不能含有空格或者中文。
1. 指定了YAML Front的`title`属性的值。

比如我们现在创建一个`blogs/main/system.md`：

```yaml
---
title: 系统教程
tags:
 - 系统
categories:
 - 教程
---

这是系统教程
```

> 目前`reco`还不支持文件热更新，每次增加或删除新的文件都需要重启整个项目才能看到变更

重启项目后可以看到：

<div align=center>
<img src="https://picx.zhimg.com/80/v2-2df9efec7cb5ecc08992271aafa020f5_1440w.png" style="width: 70%;"/>
<p class="img-caption">新增的博客</p>
</div>



### 如何部署自己的文档？

在`docs`文件夹下可以部署自己的文档，与`blogs`不同的是，`docs`不会被Blog插件渲染到首页面下。

为了更加优雅的多文档组织，我的建议是：

1. 新建一个博客作为你某个文档的入口文件。
2. 在博客中写上你的文档的各种基本信息。
3. 在`config.ts`中定义这个页面的左侧目录栏。

比如我们刚刚的`system.md`是“系统教程”这个文档的入口，假设，这个文档有“章节一”和“章节二”，我们在`./docs/system`文件夹下新建两个文件：`chapter1.md`和`chapter2.md`。

> 文件名请务必使用英文，且不含空格。每个文档建议放在`./docs/<文档英文名>/`下

```markdown
---
title: "系统 章节一"
date: 2023/03/29
---

这是章节一
```

```markdown
---
title: "系统 章节二"
date: 2023/03/29
---

这是章节二
```

然后进入`config.ts`，找到`series`属性，加入：

```js
"/blogs/main/system.html": [
    {
        text: "目录",
        children: [
            "/docs/system/chapter1",
            "/docs/system/chapter2"
        ]
    }
]
```

> 如果父子页面在项目中是父子关系，那么`children`下的路径可以不写前缀，默认以父组件路径为前缀

运行后效果：

<div align=center>
<img src="https://pica.zhimg.com/80/v2-ebd4b268d085007b5d3321cc134158c0_1440w.png" style="width: 70%;"/>
<p class="img-caption">给文档增加目录和入口</p>
</div>

这基本就满足构建一个文档的基本需求了。

最后，放一下我自己的文档：[锦恢的书籍&文档 (kirigaya.cn)](https://document.kirigaya.cn/)

---

## 部署时可能遇到的错误

`vuepress-reco`使用`vite`作为构建工具，大家都知道`vite`bug很多，所以简单记录一下可能的问题。

1. 如果markdown中使用了内嵌html，且html存在语法错误，或者你自己都没意识到你写了一个html标签，那么构建时会报错。
2. 静态资源路径，比如markdown中的图片的链接，不能含有空格，最好别是中文，否则构建时可能找不到对应的资源。
3. build完后的是SPA，要么用二级域名，要么开一个新的端口，不要想着在同一个域名下部署两个SPA，这样对用户太不友好了。