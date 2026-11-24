## 前言

vue3 想要进行组件通信，如果通信的两个组件满足在 dom 上，一个是另一个的祖先节点，那么可以使用 vue3 提供的 provide，inject 进行通信，这是 vue3 的新特性，高效且方便。

## 使用

假设现在有两个组件 parent.vue 和 child.vue，其中，在最终的 DOM 上，child.vue 被渲染在 parent.vue 的内部。parent.vue 可以通过 `provide` 将一个响应式数据传递给 child.vue :

```html
<script setup>
import { provide, ref } from 'vue';
const data = ref("hello");
provide("unique-data", data);
</script>
```

> provide 和 inject 的使用都必须在 setup 的周期内，后面的实现原理中会讲到。

在 child.vue 中，可以使用 `inject` 接受这个响应式的数据：

```html
<script setup>
import { inject } from 'vue';
const data = inject("unique-data");
</script>
```

这样，`child.vue` 就可以通过这个 `inject` 返回的响应式的 `data` 和 parent.vue 进行通信了。parent.vue 和 child.vue 中的 `data` 现在就是完全相同的数据，两边会同步发生变化。

事实上，除了数据外，`provide` 和 `inject` 还能传递函数，在看完后面的原理后，这是非常明显的。

---

## 原理

vue3 的每一个 vnode 都有一个属性叫 `provides`，实例化时每个 vnode 会把自己的 `provides` 对象传递给子节点。`provide` 源码如下：

```js
function provide(key, value) {
    if (!currentInstance) {
        {
            warn(`provide() can only be used inside setup().`);
        }
    }
    else {
        // currentInstance 就是目前进行 setup 的实例
        // 如果 currentInstance 是 provide 这个函数的发起者，那么它的 provides 属性一开始是空的
        let provides = currentInstance.provides;
        // 下面的赋值和判断在干一件事：如果目前父节点的 provides 和自己的 provides 都是空的，那么就创建一个；如果父节点的 provides 不是空的，那么就需要保证自己的 provides 和 父节点的一致
        const parentProvides = currentInstance.parent && currentInstance.parent.provides;
        if (parentProvides === provides) {
            provides = currentInstance.provides = Object.create(parentProvides);
        }
        // 把注入的数据放进 provides 对象里
        provides[key] = value;
    }
}
```

`inject` 源码如下：

```js
function inject(key, defaultValue, treatDefaultAsFactory = false) {
    const instance = currentInstance || currentRenderingInstance;
    if (instance) {
        // 如果父节点存在，就拿父节点的 provides，没有就拿自己的，这意味着， inject 和 provide 也可以在同一个组件内使用，但是事实上，没有这个必要。
        const provides = instance.parent == null
            ? instance.vnode.appContext && instance.vnode.appContext.provides
            : instance.parent.provides;
        if (provides && key in provides) {
            // TS doesn't allow symbol as index type
            return provides[key];
        }
        else if (arguments.length > 1) {
            return treatDefaultAsFactory && shared.isFunction(defaultValue)
                ? defaultValue.call(instance.proxy)
                : defaultValue;
        }
        else {
            warn(`injection "${String(key)}" not found.`);
        }
    }
    else {
        warn(`inject() can only be used inside setup() or functional components.`);
    }
}
```