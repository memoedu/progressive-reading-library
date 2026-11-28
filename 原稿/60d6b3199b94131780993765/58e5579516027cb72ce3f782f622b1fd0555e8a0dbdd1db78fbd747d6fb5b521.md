## 前言

最近心血来潮，打算给 Live2dRender 上线一下官方文档，熟悉我的朋友都知道，除了我的主站 `kirigaya.cn` 外，我还有一个专门用来呈现“系统性，非碎片化”内容的副站，`document.kirigaya.cn`，使用 vuepress 开发的。专门用来放置一些被我精心整理成 markdown 书籍和文档。我开发的软件的文档一般都会放在那里面。

但是支持单纯的 markdown 语法很多时候无法满足我开发文档的一些小心思。比如我非常喜欢知乎的这个块状链接：

<div align=center>
<img src="https://picx.zhimg.com/80/v2-6554a90dc27cd5a757017acd456095a6_1440w.png" style="width: 60%;"/>
</div>

这比单纯的链接好看多了，但是我们一般打 markdown 写 `[xxx](https://xxx)` 的样式就很简单，无法像知乎一样，还能往链接上加图片和标题，简介。

幸好，vuepress 提供了功能，让熟悉 vue 开发的朋友在撰写博客时能直接开发自己的组件，然后在 markdown 中使用，个人体验非常好！

<div align=center>
<img src="https://picx.zhimg.com/80/v2-eac4ea307c22de913763bf8c2b95c21a_1440w.webp?source=c8b7c179"/>
</div>

---

## vuepress 组件开发

想要在 vuepress 中开发组件，只需要在 `.vuepress/components` 文件夹下创建对应的文件即可。

比如你创建了 `my-image.vue`，那么在 markdown 中，直接使用如下的方法就能使用：

```markdown
## 健身技巧

今天我们介绍一种新的健身方法，如下图

<my-image
	src="https://xxx.xxx"
	title="新的健身技巧"
/>

我们 ...
```

vuepress 会自动把 components 下的组件全局注册，非常方便。

---

## vuepress 开发案例——带标题，简介和 cover 的超链接

那么我们来实现刚刚说的那个块状超链接，它需要能够呈现一个超链接的 标题，简介和 cover，再加上超链接本身，因此需要三个参数，我们在 `.vuepress/components` 下创建 `detail-url.ts` 和 `detail-url.vue`。

detail-url.ts

```typescript
export const webUrl = {
    github: 'https://pic1.zhimg.com/80/v2-9cb9694baf4c131e3e4d5954f3e552a1_1440w.png',
    npm: 'https://picx.zhimg.com/80/v2-65e3195cf7b30a95244e1b3337b0cad6_1440w.png'
};
```

detail-url.vue

```html
<template>
    <div class="detail-url-wrapper">
        <div class="detail-url-container"
            @click="click()"
        >
            <div class="cover">
                <img :src="blockLogo" class="display-image">
            </div>
            <div class="main">
                <div class="title">
                    {{ blockTitle }}
                </div>
                <div class="desc">
                    <span>{{ props.desc }}</span>
                </div>
            </div>
        </div>
    </div>
</template>

<script setup lang="ts">
import { defineComponent, defineProps, computed } from 'vue';

import { webUrl } from './detail-url';

defineComponent({
    name: 'detail-url'
});

const props = defineProps({
    href: {
        type: String,
        required: true
    },
    logo: {
        type: String,
        default: 'https://avatars.githubusercontent.com/u/59416203?v=4'
    },
    desc: {
        type: String,
        required: true
    },
    title: {
        type: String,
        required: false
    }
});

const blockTitle = computed(() => {
    if (props.title) {
        return props.title;
    } else {
        try {
            const url = new URL(props.href);
            return url.hostname;
        } catch (error) {
            return 'No Title';
        }
    }
});

const blockLogo = computed(() => {
    if (props.logo.startsWith('http')) {
        return props.logo;
    } else {
        const url = webUrl[props.logo];
        if (url) {
            return url;
        } else {
            return props.logo;
        }
    }
})

function click() {
    window.open(props.href, '_blank');
}

</script>

<style>
.detail-url-wrapper {
    width: 100%;
    display: flex;
    margin-top: 10px;
    margin-bottom: 10px;
}

.detail-url-container {
    padding: 0px 10px;
    width: 80%;
    border-radius: .8rem;
    background-color: rgba(93, 103, 232, 0.1);
    backdrop-filter: blur(5px);
    display: flex;
    align-items: center;
    cursor: pointer;
    transition: var(--animation-5s);
}


.detail-url-container:hover {
    background: linear-gradient(
                    90deg, 
                    rgba(254, 125, 55, 0.1) 25%,
                    rgba(93, 103, 232, 0.1) 37%,
                    rgba(255, 255, 255, 0) 63%
                );
    background-size: 400% 100%;
    animation: loading-mask 1.4s cubic-bezier(0.23,1,0.32,1);
    transition: var(--animation-5s);
}

.detail-url-container .cover {
    position: relative;
    overflow: hidden;
    height: 60px;
    width: 60px;
    overflow: hidden;
    border-radius: 0.6em;
}

.detail-url-container .main {
    margin: 15px;
}

.detail-url-container .title {
    font-size: 1.0rem;
    font-weight: 800;
    margin-bottom: 5px;
}

.detail-url-container .desc {
    font-size: 0.9rem;
}


.display-image {
    position: absolute;
    top: 50%;
    height: 100%;
    width: 100%;
    object-fit: cover;
    transform: translateY(-50%);
}

</style>
```

然后，就可以在 markdown 中愉快使用了：

```markdown
## 安装与使用

:::info
因为这个知乎问题下的评论：

<detail-url
    href="https://www.zhihu.com/question/659058271/answer/3538726616"
    title="博客园页面的这个姑娘动画是怎么生成的？"
    desc="她好可爱哦。"
></detail-url>

在 2024.06.26 我考完试后，我把 Live2dRender 移植到了 ES 模块，这样在静态页面博客，比如 hexo 和 hugo 中也能使用了，特此纪念。0.0.3 版本开始，我还给 live2d 加了一个工具箱。看下面。
:::

项目链接，求个免费的 star :D

<detail-url
    href="https://github.com/LSTM-Kirigaya/Live2dRender"
    logo="github"
    title="LSTM-Kirigaya/Live2dRender"
    desc="https://github.com/LSTM-Kirigaya/Live2dRender"
></detail-url>

<detail-url
    href="https://www.zhihu.com/question/659058271/answer/3538726616"
    logo="npm"
    title="live2d-render - npm"
    desc="https://www.npmjs.com/package/live2d-render"
></detail-url>
```

渲染效果：https://document.kirigaya.cn/blogs/live2d-render/main.html#live2drender
<div align=center>
<img src="https://pic1.zhimg.com/80/v2-6265e38745ff836509d37f87a665a28d_1440w.png" style="width: 60%;"/>
</div>

