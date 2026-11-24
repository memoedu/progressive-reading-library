## 前言

天下苦 element-plus 久矣，为了取代之前使用的 element-plus 分页器，我决定自己写一个，什么是分页器？给一张图你就知道了：

<div align=center>
<img src="https://pica.zhimg.com/80/v2-594f41418cf3563ed01fc98d2101923f_1440w.jpeg" style="width: 60%;"/>
</div>

GitHub 上面用于展示页数和翻页的组件就称为分页器 Pagination，这是一个用于展示大量条目数据时的一个很好的分块组件。

## 需求分析

一个基本的分页器需要满足下面的三个需求：

- 显示总的页数（total）和当前处于的页的编号（current-page）
- 当总页数超出某个值时（page-size），分页器能够使用省略号省略多余的页，以免渲染不好看。
- 可以点击分页器的前进后退，或者上面的页编号进行跳转。

## 代码

### template 部分

```html
<template>
    <div>
        <ul class="pagination">
            <li>
                <span 
                    :class="pagination.preButtonClass"
                    class="pageButton"
                    @click="pagination.toPrev">Prev</span>
            </li>
            <li v-for="(page, index) of pagination.renderPages" :key="index">
                <a class="pageNumber page"
                   :class="(props.currentPage === page) ? 'current-page-active': ''"
                   @click="pagination.toPage(page)"
                >{{ page }}</a>
            </li>
            <li>
                <span 
                    :class="pagination.nextButtonClass"
                    class="pageButton"
                    @click="pagination.toNext">Next</span>
            </li>
        </ul>
    </div>
</template>
```


### script 部分

```html
<script>
import { computed, reactive } from "vue";

export default {
    name: "KPagination",
    props: {
        total: {
            type: Number,
            required: true
        },
        pageSize: {
            type: Number,
            default: 7
        },
        currentPage: {
            type: Number,
            required: true
        }
    },
    emits: ["update:current-page"],
    setup(props, ctx) {
        const pagination = reactive({
            preButtonClass: computed(() => (props.currentPage === 1) ? 'button disabled' : 'button'),
            nextButtonClass: computed(() => (props.currentPage === props.total) ? 'button disabled' : 'button'),
            toPrev() {
                ctx.emit("update:current-page", props.currentPage - 1);
            },
            toNext() {
                ctx.emit("update:current-page", props.currentPage + 1);
            },
            toPage(page) {
                if (typeof page === 'number') {
                    ctx.emit("update:current-page", page);
                }
            },
            renderPages: computed(() => {
                const pages = [];
                if (props.total <= props.pageSize) {
                    for (let i = 1; i <= props.total; ++ i) {
                        pages.push(i);
                    }
                } else if (props.currentPage * 2 < props.pageSize) {
                    // 处于开头几个页
                    for (let i = 1; i < props.pageSize; ++ i) {
                        pages.push(i);
                    }
                    pages.push('...');
                    pages.push(props.total);
                } else if ((props.total - props.currentPage + 1) * 2 < props.pageSize) {
                    // 处于结尾几个页
                    pages.push(1);
                    pages.push('...');
                    for (let i = 1; i < props.pageSize; ++ i) {
                        pages.push(i + props.total - props.pageSize + 1);
                    }
                } else {
                    // 处于中间状态
                    pages.push(1);
                    pages.push('...');
                    for (let i = 0; i < props.pageSize - 2; ++ i) {
                        pages.push(i + props.currentPage - ((props.pageSize - 2) >> 1));
                    }
                    pages.push('...');
                    pages.push(props.total);
                }

                return pages;
            })
        });
        return {
            pagination,
            props
        };
    }
};
</script>
```


### style 部分


```html
<style>
.pageNumber {
    font-size: 18px !important;
    font-family: var(--base-font);
    cursor: pointer;
    user-select: none;
}

.current-page-active {
    color: white !important;
    background-color: var(--main-color);
}

.pageNumber:hover {
    color: white !important;
    background-color: var(--shadow-color);
    transition: .3s linear ease-in-out;
}

.pageButton {
    font-size: 14px;
    user-select: none;
}

ul.pagination {
    cursor: default;
    list-style: none;
    padding-left: 0;
}

ul.pagination li {
    display: inline-block;
    padding-left: 0;
    vertical-align: middle;
}

ul.pagination li>.page {
    -moz-transition: background-color 0.2s ease-in-out, color 0.2s ease-in-out;
    -webkit-transition: background-color 0.2s ease-in-out, color 0.2s ease-in-out;
    -ms-transition: background-color 0.2s ease-in-out, color 0.2s ease-in-out;
    transition: background-color 0.2s ease-in-out, color 0.2s ease-in-out;
    border-bottom: 0;
    border-radius: 0.375em;
    display: inline-block;
    font-size: 0.8em;
    font-weight: 600;
    height: 2em;
    line-height: 2em;
    margin: 0 0.125em;
    min-width: 2em;
    padding: 0 0.5em;
    text-align: center;
}

ul.pagination li:first-child {
    padding-right: 0.75em;
}

ul.pagination li:last-child {
    padding-left: 0.75em;
}

@media screen and (max-width: 480px) {
    ul.pagination li:nth-child(n+2):nth-last-child(n+2) {
        display: none;
    }

    ul.pagination li:first-child {
        padding-right: 0;
    }
}
</style>
```

---

## 使用

使用前，我们先将上面的组件文件 `KPagination.vue` 注册为 kpagination，然后找到需要插入的地方，写下如下代码：

```html
<kpagination
    v-model:current-page="myBlogs.curPage"
    :page-size="7"
    :total="myBlogs.pageNum"
    @click="myBlogs.move"
></kpagination>
```

其中 `v-model:current-page` 是当前的页码，是双向绑定的；`page-size` 设为 7，代表最多展示 7 个页码，多余使用省略号替代；`total` 代表总的页码数； `@click` 绑定的函数用于在用户点击了页码时做出反应，比如我这里就是做出请求，根据页码向后端请求这个页码上的文章。

最终的效果如下：

<div align=center>
<img src="https://picx.zhimg.com/80/v2-3aa1f75378eeda2cf88ffff42906d9bc_1440w.jpeg" style="width: 60%;"/>
</div>