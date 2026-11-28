## 前言

Webpack 就不多说了，进来看的人都知道它是什么玩意儿。今天为什么再次提起这个工具呢？因为我发现我被 Webpack 的默认优化策略坑了整整三年，今天就浅谈一下如何优化你的 webpack 打包项目。此处假设你已经搭建了一个基于 webpack 打包的 vue 项目。

---

## 策略1：编码压缩

采用编码压缩是 webpack 中一个我个人用下来收益最大的方法，原理就是在打包后把每一个 chunk 的 js 进行压缩，客户端在接收到 js 包再根据编码方式进行解压。最流行的压缩算法就是 gzip，目前压缩效率最高的是 google 工程师发明的 brotli 算法。

使用 brotli 需要同时配置 webpack 和 nginx 才能生效，教程我在之前写过：[Brotli压缩算法的vue打包与Nginx配置](https://kirigaya.cn/blog/article?seq=91).

不过，现在需要小小地更新一下 webpack 中的配置，先安装 zlib：

```bash
npm install zlib -D
```

在 vue.config.js 中配置如下：

```js
// vue.config.js
const isProduction = process.env.NODE_ENV !== 'development';

const zlib = require('zlib');

module.exports = {
    configureWebpack: config => {
        if (isProduction) {
            config.plugins.push(
                new CompressionWebpackPlugin({
                    algorithm: 'brotliCompress',
                    compressionOptions: {
                        [zlib.constants.BROTLI_PARAM_QUALITY]: 11
                    },
                    test: new RegExp(
                        '\\.(' + compressResourceType.join('|') + ')$'
                    ),
                    threshold: 10240,
                    minRatio: 0.8,
                    // deleteOriginalAssets: false
                })
            );
            
            ...
        }
    }
}
```

配置完压缩后，网站首屏加载速度可以提升 80% 左右。

---

## 策略2：取消 prefetch

webpack 在打包 vue 项目时，默认会在最终的 index.html 中加入一堆的 prefetch：

```html
  <link href=css/chunk-02316de6.d6a34d41.css rel=prefetch>
  <link href=css/chunk-17d12024.49e9beee.css rel=prefetch>
  <link href=css/chunk-1a459fbc.bbcdb57b.css rel=prefetch>
  <link href=css/chunk-36294693.ec54126e.css rel=prefetch>
  <link href=css/chunk-41627eb8.db790792.css rel=prefetch>
  <link href=css/chunk-50597cab.6ed8d563.css rel=prefetch>
  <link href=css/chunk-5570b920.dba56c40.css rel=prefetch>
  <link href=css/chunk-68a047cb.98d9913d.css rel=prefetch>
  <link href=css/chunk-6e311e88.a4d37475.css rel=prefetch>
  <link href=css/chunk-994d64b4.1b78acae.css rel=prefetch>
  <link href=css/chunk-9a3846e6.237d6d9f.css rel=prefetch>
  <link href=css/chunk-aadde01a.78263062.css rel=prefetch>
  <link href=css/chunk-cdfd6c6c.ea569cd5.css rel=prefetch>
  <link href=js/chunk-02316de6.1a99e2fb.js rel=prefetch>
  <link href=js/chunk-17d12024.68fb7a74.js rel=prefetch>
  <link href=js/chunk-1a459fbc.dbb6c6c1.js rel=prefetch>
  <link href=js/chunk-36294693.379104b9.js rel=prefetch>
  <link href=js/chunk-41627eb8.ddfc42b0.js rel=prefetch>
  <link href=js/chunk-50597cab.1e207f1b.js rel=prefetch>
  <link href=js/chunk-5570b920.8bca4266.js rel=prefetch>
  <link href=js/chunk-6142f9b6.4c19a6cf.js rel=prefetch>
  <link href=js/chunk-68a047cb.43c9f023.js rel=prefetch>
  <link href=js/chunk-6e311e88.d52693c2.js rel=prefetch>
  <link href=js/chunk-7301aba0.bf9e475d.js rel=prefetch>
  <link href=js/chunk-994d64b4.3d192af0.js rel=prefetch>
  <link href=js/chunk-9a3846e6.954284ba.js rel=prefetch>
  <link href=js/chunk-aadde01a.df6f4439.js rel=prefetch>
  <link href=js/chunk-cdfd6c6c.25e21cb5.js rel=prefetch>
  <link href=css/app.b2839875.css rel=preload as=style>
  <link href=css/chunk-vendors.779f7d1d.css rel=preload as=style>
  <link href=js/app.1a2f81ff.js rel=preload as=script>
  <link href=js/chunk-vendors.3a58506f.js rel=preload as=script>
  <link href=css/chunk-vendors.779f7d1d.css rel=stylesheet>
  <link href=css/app.b2839875.css rel=stylesheet>
```

因为我之前不怎么关注打包结果，所以不是非常关心这一点，直到最近打算折腾 webpack 底层组件时，才发现了打包得到的这一坨。可以看到，里面有一堆的 prefetch。

prefetch 是 HTML 给 tag 的一个特性，允许浏览器在空闲的时候加载对应的资源。但是这么做在如今的大型项目中并不合理，因为：

- 大型项目拥有很多很多的页面，大部分用户可能只会前往某一个页面，大部分页面不是所有人都经常看，把这些冷门页面 prefetch 实在不合理。
- 虽然 prefetch 原理上说是在浏览器空闲时加载，但是它仍然会和 preload 标签内部二次发起的 Ajax 争抢带宽，导致网站初次加载速度变慢。
- 如果你观察其他的 Vue 开发的网站的初次请求，你会发现成熟网站的 prefetch 非常少（比如 B站 使用 vue 开发，但是首页的 prefetch 只有一个）

综上所示，建议直接关闭所有的 prefetch，因为它已经成为了负优化：

```js
// vue.config.js

module.exports = {
    chainWebpack: config => {
        if (isProduction) {
            config.plugins.delete('prefetch');
        }
    }
}
```

删除后，网站速度可以得到进一步提升。

---

## 策略3：切分 chunk-vendors.js

如果你仔细观察过 webpack 的打包结果，你会发现，打包后的 js 分为三类：

- app.<文件标签>.js ：对应 main.js 和 App.vue 及其引用代码。
- chunk-<文件标签>.js ： 对应不同的 vue 文件。
- chunk-vendors-<文件标签>.js ：对应 node_modules 下的库。

其中，chunk-vendors.js 在默认情况下会打包整个 node_modules，一般都比较大，比如我的，就算做了不少优化，但是还有 700KB 。而且它会出现在首次访问网站的请求中，但是我们的网站的首页大部分情况下不会用到所有 node_modules 下的包，所以直接请求整个 node_modules 就很不合理。一个优化策略就是把 chunk-vendors.js 切分成几块，这样可以减少对其他部分的请求，从而加快网站整体的请求速度。

关于 chunk-vendors.js 分割有一个讲的非常详细的文章，强烈推荐：[webpack 4: 代码分割优化最佳实践方案](https://juejin.cn/post/7320288231195426866)，这篇文章给出了不同参数下的 chunk-vendors.js 分割结果对网站流量的影响。我这边就简单点了，直接给出最终的配置方案：

```js
// vue.config.js

module.exports = {
    configureWebpack: config => {
        if (isProduction) {
            config.optimization = {
                splitChunks: {
                    chunks: 'all',
                    maxInitialRequests: Infinity,
                    minSize: 0,
                    cacheGroups: {
                        vendor: {
                            test: /[\\/]node_modules[\\/]/,
                            name(module) {
                                const packageName = module.context.match(/[\\/]node_modules[\\/](.*?)([\\/]|$)/)[1];
                                return `npm.${packageName.replace('@', '')}`;
                            }
                        }
                    }
                }
            }
        }
    }
}
```

通过分包后，我的网站的提升速度不是很显著，因为我的 node_modules 不是很大很大。对于依赖了一大堆库的项目，这个策略应该能起到很大的作用。

---

## 策略4：打包结果可视化

一个好的代码习惯是解决一切的最好手段，如果可以可视化打包后各个项目原文件的大小占比，就可以更好地帮助我们对症下药，找到拖累性能的某个库或者某个不合理的 import 。`*webpack-bundle-analyzer` 这个工具可以帮助我们可视化 webpack 打包结果，首先安装：

```bash
npm install webpack-bundle-analyzer -D
```

然后在 vue.config.js 中进行配置：

```js
const BundleAnalyzerPlugin = require('webpack-bundle-analyzer').BundleAnalyzerPlugin;
module.exports = {
    configureWebpack: config => {
        if (isProduction) {
            config.plugins.push(
            	new BundleAnalyzerPlugin()
            );
        }
    }
}
```

然后运行打包脚本：

```bash
npm run build
```

打包结束后，会自动开启一个 http 服务，可视化你的打包结果中各种成分占比：

<div align=center>
<img src="https://picx.zhimg.com/80/v2-31f1b98c77d6207a6cf3df430ba38bc6_1440w.png" style="width: 70%;"/>
</div>

根据其中最大的几项占比，我们可以更好地去了解哪里可以优化。哪里的 import 可以通过 `import().then` 的方式进行优化。

