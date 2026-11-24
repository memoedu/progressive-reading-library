## 前言

在开发任何软件时，我们总希望这个软件的使用群体可以是任何人。为了防止不同国家的人看不懂我们开发的软件呈现的自然语言，一般情况下，很多人会图方便把软件做成全英文的，但是这么做总是会让人觉得不够优雅。而且如今的 chatgpt 等软件已经让优质批量化翻译变得简单易用，这篇文章就简单讲讲如何使用多语言框架 `vue-i18n` 在 vue 项目中添加多语言。

---

## 安装

在 vue 项目中使用如下进行安装：

```bash
npm install vue-i18n
```

---

## 框架部署

在项目中部署 vue-i18n 比较坑，因为内网和外网大部分关于 vue-i18n 的教程都有点过时了，而且并没有完全覆盖 JavaScript 和 TypeScript。下面就直接给出对应的教程。此处以中文和英文双语音为例。

首先在 `src` 目录下创建文件夹 `i18n`，然后创建语言的文件 `zh.json` 和 `en.json`，分别对应 中文 和 英文（详情请见我的往期博客[自然语言的ID——ISO 639语言编码标准](https://kirigaya.cn/blog/article?seq=68)），再创建 `index.js`，文件目录如下：

```txt
📁src
   ├─📁components
   ├─📁hook
   ├─📁i18n
   │  ├─🎗️index.js
   │  ├─🏷️zh.json
   │  └─🏷️en.json
   ├─📁views
   ├─🌿App.vue
   └─🎗️main.js
```

在 `🏷️en.json` 中如下编写：

```json
{
    "module": "Modules",
    "signal": "Signals",
    "search-signal": "Search Signal"
}
```

在 `🏷️zh.json` 中如下编写：

```json
{
    "module": "模块",
    "signal": "信号",
    "search-signal": "搜索信号"
}
```

在 `🎗️index.js` 中如下编写：

```javascript
import { createI18n } from 'vue-i18n';

import en from './en.json';
import zh from './zh.json';

const i18n = createI18n({
    legacy: false,
    locale: 'en',		// 默认语言
    messages: { en, zh }
});

export default i18n;
```

如果你使用 TypeScript，则在 `index.ts` 中如下编写：

```typescript
import { createI18n } from 'vue-i18n';

import en from './en.json';
import zh from './zh.json';

type MessageSchema = typeof en

const i18n = createI18n<[MessageSchema], 'en' | 'zh' >({
    legacy: false,
    locale: 'en',		// 默认语言
    messages: { en, zh }
});

export default i18n;
```

在 `🎗️main.js` 中如下编写：

```javascript
import { createApp } from 'vue';
import App from './App.vue';
import i18n from './i18n';

createApp(App)
    .use(i18n)
    .mount('#app')
```

TypeScript 的 `main.ts` 编写完全一致。

需要注意，所有的 `🏷️*.json` 文件的 key 必须完全一致（数量一致，名称一致），否则会报错。

---

## 基本使用

如果需要在前端渲染某个json的key对应的文本，使用如下：

```javascript
import { useI18n } from 'vue-i18n';

export default {
    name: 'wires',
    props: {},
    setup() {
        const { t } = useI18n();

        return {
            t
        }
    }
}
```

template 中使用 t 进行文本渲染：

```html
<template>
    <div>
        <div>{{ t('signal') }}</div>
    </div>
</template>
```

由于默认语言是 en，上述文本会被渲染为 `Signals`。如果我们编写了一个功能，用来切换语言，那么通过 vue-i18n 的 API 切换语言呢？和上面类似，先通过解构赋值得到 `t` 和 `locale`，然后对 `locale` 的 value 直接赋值即可：

```javascript
const { t, locale } = useI18n();

// 切换为中文
locale.value = 'zh';
```

如此，我们便轻易完成了对于语言的管理和切换。因为 `locale` 本身也是一个 ref 对象，所以如果各位读者足够了解组件，可以将 locale 绑定到 input 的 v-model 上从而完成语言的选择。

<div align=center>
<img src="https://pic1.zhimg.com/80/v2-ee9ddf8823b467aeab2cc5a4a3782426_1440w.png" style="width: 80%;"/>
<p class="img-caption">en & zh</p>
</div>

---

## Bug 解决：i18n 在 setup 之外的地方使用报错

 如果想要在非 vue 的 setup 环境下使用 i18n （比如开发工具函数），不能直接 import vue-i18n，需要 import 刚刚创建的 `🎗️index.js` 中的文件：

```js
// utils.js
import i18n from '/path/to/i18n/index.js';
const { t } = i18n.global;

// 函数代码
// ...

```