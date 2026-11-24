## 前言

相信大家在不少网站都见过一个基础的按钮动画，就是你把鼠标移动到这个按钮上，按钮下方会自动展开一个功能列表，让你进行选择，比如文档网站的语言选择按钮、网购网站的商品类型选择功能，都有这些功能。比如：

<center>
<video width="80%" controls>  
  <source src="https://kirigaya.cn/files/video/blog/vue-float-window.mp4" type="video/mp4">  
  您的浏览器不支持视频标签。  
</video>
</center>

我其实之前实现过类似的功能，但是由于当时年纪还比较小，并没有封装模块的意思，因此，在迁移到 vue + typescript 的第二个年头，我打算趁着这次给实验室搭建数据平台的机会，来实现一下浮动式下拉窗口的 vue 模块。

---

## 使用

使用很直观，在这个模块中，我用了两个具名插槽：

| slot | 解释 |
| --- | --- |
| header | 主要的按钮 |
| content | 悬浮到按钮上显示的悬浮窗口内容 |

模块参数为：

```ts
interface FloatWindowProps {
    buttonClass?: string  // 最外层按钮的 css class，默认为 float-window-button
    headerClass?: string // header 插槽元素的 css class，默认为 float-window-header
    contentClass?: string // content 插槽元素的 css class，默认为 float-window-content
    hiddeDelay?: number // 鼠标离开 header 或者 content 后 content 多少 ms 后消失，默认为 300
    animation?: string // content 展示时的动画，默认为 main-fade，需要自己实现
}

```

一个示例如下，文件为 `App.vue`，假设我们刚才把 `float-window.vue` 存储到了文件夹 `components` 下。省略部分不重要的细节：

```html
<template>
...
...
            <floatWindow 
                :content-class="'i18n-list'"
                :animation="'main-fade'"
            >
                <template #header>

                </template>
                <template #content>
                    <div>我是悬浮窗口的内容</div>
                </template>
            </floatWindow>
...
...
</template>

<script setup lang="ts">
import floatWindow from "./components/float-window.vue";
// 其他逻辑代码

</script>

<style>
.main-fade-enter-from,
.main-fade-leave-to {
    opacity: 0;
}
.main-fade-enter-to,
.main-fade-leave-from {
    opacity: 1;
}
.main-fade-enter-active,
.main-fade-leave-active {
    transition: opacity var(--main-during);
    -moz-transition: opacity var(--main-during);
    -webkit-transition: opacity var(--main-during);
}

.i18n-list {
    top: calc(var(--nav-height) - 25px);
    right: 25px;
    padding: 10px;
    background-color: white;
    box-shadow: 0 0 15px rgba(0, 0, 0, 0.25);
    border-radius: .9em;
}
</style>
```

---

## 代码

以下文件为 `float-window.vue`。阁下可以直接复制粘贴

```html
<!-- Component Definition -->
<template>
    <button :class="props.buttonClass">
        <div :class="props.headerClass" 
            @mouseover="contentController.show" 
            @mouseleave="contentController.hide"
        >
            <slot name="header"></slot>
        </div>
        <Transition :name="props.animation">
            <div v-show="!contentController.hidden" 
                @mouseover="contentController.show"
                @mouseleave="contentController.hide"
            >
                <div 
                    :class="props.contentClass" 
                    style="position: absolute;" 
                >
                    <slot name="content"></slot>
                </div>
            </div>
        </Transition>
    </button>
</template>

<script setup lang="ts">
import { defineProps, withDefaults, reactive } from 'vue';

interface FloatWindowProps {
    buttonClass?: string
    headerClass?: string
    contentClass?: string
    hiddeDelay?: number
    animation?: string
}

const props = withDefaults(defineProps<FloatWindowProps>(), {
    buttonClass: 'float-window-button',
    headerClass: 'float-window-header',
    contentClass: 'float-window-content',
    hiddeDelay: 300,
    animation: 'main-fade'
});

// acquire handle of elements

const contentController = reactive({
    hidden: true,
    hideTimerID: -1,
    show() {
        this.hidden = false;
        if (this.hideTimerID != -1) {
            clearInterval(this.hideTimerID);
            this.hideTimerID = -1;
            console.log(props.contentClass);
            
        }
    },
    hide() {
        this.hideTimerID = setTimeout(() => {
            this.hidden = true;
        }, props.hiddeDelay);
    }
});

</script>

<!-- style -->
<style>
:root {
    --animation-during: .5s;
}

/* 虽然名字很奇怪，但是这个类代表最外层的那家伙 */
.float-window-button {
    height: fit-content;
    width: fit-content;
    font-size: 16px;
}

/* 一般情况下，我们认为 content 依附于 header */
.float-window-header {
    position: relative;
}

.float-window-content {
    position: absolute;
    bottom: -20px;
    padding: 10px;
    border-radius: .9em;
}
</style>
```