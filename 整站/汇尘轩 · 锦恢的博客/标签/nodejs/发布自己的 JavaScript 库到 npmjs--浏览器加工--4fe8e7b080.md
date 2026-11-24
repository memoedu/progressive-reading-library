## 前言

我其实去年就在 npmjs 上发布过自己制作的库 [live2d-render](https://www.npmjs.com/package/live2d-render) 。今年打算更新时，发现居然忘记怎么做了。简单记录一下。

---

## 文件结构

不同于 python 的 PyPi， JavaScript 的库可以打包上传，也可以不打包上传，而且比 PyPi 体验好得多。比如我们现在需要发布一个库，那么上传打包的文件结构就如下：

```
index.js
index.d.ts
package.json
README.md
```

- `package.json`：配置发布信息的文件。
- `README.md`：库首页的说明，和 GitHub 上面的一样。

发布之后，别人如果使用 `npm install live2d-render` 下载后，上面这些文件会出现在这个位置：

```
📁node_modules
   ├─📁live2d-render
       ├─🎗️index.js
       ├─🎗️index.d.ts
       ├─🏷️package.json
       └─📄README.md
   ├─ ...
```

---


## package.json

库的信息全部在 `package.json` 中罗列，我这边就直接给出我的库了：

```json
{
    "name": "live2d-render",
    "version": "0.0.3",
    "description": "a easy-to-go render interface for the newest version of live2d",
    "main": "index.js",
    "scripts": {
        "test": "echo \"Error: no test specified\" && exit 1"
    },
    "repository": {
        "type": "git",
        "url": "git+https://github.com/LSTM-Kirigaya/Live2dRender.git"
    },
    "keywords": [
        "live2d",
        "render",
        "live2d-render"
    ],
    "author": "kirigaya",
    "license": "Apache-2.0",
    "bugs": {
        "url": "https://github.com/LSTM-Kirigaya/Live2dRender/issues"
    },
    "homepage": "https://github.com/LSTM-Kirigaya/Live2dRender#readme"
}
```

除了 `package.json`，最好把自己的项目上传到 GitHub 并给出 GitHub 链接。当然，这个也不是必须的，看你自己了。

---

## 通过 npm 发布

`npm` 本身就带发布包的功能，这点比隔壁的 python 不知道好了多少。在进行发布之前，需要先切换源：

```bash
npm config set registry https://registry.npmjs.org/
```

只有这样，后续的登录和发布的源网站才能正确。

然后，先登录：

```bash
npm login
```

稍等片刻后，会出现一个带有  signin 文字的链接，而且是暂时生成的后缀路由，点进去登录你的 npmjs 账号。就可以了。

然后发布：

```bash
npm publish
```
回车后，还需要根据提示，进入 npmjs 暂时生成的链接进行验证，我的验证机制是本地的 pin 码，建议在注册 npmjs 账号时，更换为更加安全的双因子认证 2FA。一切结束后，发布就完成了。