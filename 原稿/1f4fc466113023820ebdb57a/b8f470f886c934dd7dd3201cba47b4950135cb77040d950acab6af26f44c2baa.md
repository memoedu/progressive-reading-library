## eslint
vue 等前端构建 LSP 默认采用 eslint，eslint 的规则通常比较严格。比如它不允许代码中存在未被使用的变量，像 go 一样，会直接报错。但是前端开发中，给一些实验性变量玩放置 play 是很正常的事情。因此，在开发中关闭 eslint 的部分功能很有必要。

## 关闭 `no-unused-vars`

打开 `package.json`， vue 脚手架创建的默认配置文件中会存在 `eslintConfig` 这个字段：

<div align=center>
<img src="https://pic1.zhimg.com/80/v2-f1654d6f8f101770f7902d0a165c75c9_1440w.png" style="width: 50%;"/>
</div>

找到 `rules` 这个属性，填入如下属性即可关闭未经引用变量的检查。
```json
    "rules": {
        "no-unused-vars": "off"
    }
```

---

## 最粗暴的 eslint-disable

如果希望 eslint 检查跳过某个特定的文件，比如 bundle，那么在这个文件的开头第一行加入如下注释即可：
```js
/* eslint-disable */
```