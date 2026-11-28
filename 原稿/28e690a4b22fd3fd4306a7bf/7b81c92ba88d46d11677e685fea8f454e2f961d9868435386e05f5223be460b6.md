## 前言

在 [NestJS 快速入门](https://kirigaya.cn/blog/article?seq=303) 中，我们讨论了如何快速开始一个 NestJS 项目。在文章的 CI/CD 这个节中，我说了关于 husky 的更多用法后面讨论，所以，here it come :D

---

## 快速开始

安装并初始化 husky 的步骤比较简单，在上一章我已经写过了：

```bash
# 安装 husky，一个很好用的 commit 插件
npm install husky lint-staged --save-dev

# 初始化 husky
npx husky init
```

上面的做法会在根目录下产生一个 `.husky` 文件夹：

```
📦.husky
 ┣ 📂_
 ┗ 📜pre-commit
```

我们每次使用 git 提交代码时，commit 之前，husky 都会自动让 git 执行 `.husky/pre-commit` 中的脚本内容。

pre-commit 默认为 `npm test`，也就是会执行一次 UT。

---

## 代码格式化

除了进行了 UT 之外，precommit 里面放入代码格式化插件也是非常不错的，比如 typescript 的格式化工具 `prettier`。在上一篇文章中，我们使用 `nest` 脚手架生成的代码默认都是两个空格缩进，而非一个 tab 缩进，在进行前后端都是 typescript 的全栈项目开发时，缩进不统一会带来巨大的心智负担。

而且，每次提交前对代码进行格式化也是增加项目可维护性的一个很好的方法，NestJS 的项目默认是提供了 prettier 工具的，如果没有，通过下面的命令安装：

```bash
npm install prettier --save-dev
```

然后在 `package.json` 中添加命令：

```json
	"scripts": {
		"format": "prettier --write \"src/**/*.ts\" \"test/**/*.ts\""
	}
```

在根目录创建 `.prettierrc`：

```json
{
	"singleQuote": true,
	"trailingComma": "all",
	"useTabs": true,
	"tabWidth": 4
}
```

我们先测试一下:

```bash
npm run test
```

假设我们有一个如下的文件

```typescript
import { Module } from '@nestjs/common';
import { TeaService } from './tea.service';
import { TeaController } from './tea.controller';

@Module({
  controllers: [TeaController],
  providers: [TeaService],
})
export class TeaModule {



	
}


```

格式完成后，它的样子：

```typescript
import { Module } from '@nestjs/common';
import { TeaService } from './tea.service';
import { TeaController } from './tea.controller';

@Module({
	controllers: [TeaController],
	providers: [TeaService],
})
export class TeaModule {}

```

这样可以有效降低 CI/CD 代码审查时的负担，也可以让团队代码风格尽可能保持一致。

我们来把格式化加入 precommit 中，修改 `./husky/pre-commit` 如下：

```bash
npm run format
npm test
```

这样每次提交代码就能先进行格式化，再进行测试了。

## 其他操作

除了格式化和UT，还有很多其他操作可以添加进入 pre-commit 来降低项目未来的维护成本，比如 eslint 检查变量使用情况，命名规则等等更加严格的规范，代码复杂度检查。直接执行下面的命令就能进行静态检查：

```bash
npm run lint
```

但是一定要记住一点，这些操作并不是越多越好，一定要根据团队的情况量体裁衣地设置 pre-commit。数量太多的 pre-commit 会显著增加项目的上手门槛。比如我的项目运行上面的 lint 后报错如下：

```log

> learn-nestjs@0.0.1 lint
> eslint "{src,apps,libs,test}/**/*.ts" --fix


/Users/kirigaya/projects/learn-nestjs/src/animation/animation.service.ts
   7:9   error  'createAnimationDto' is defined but never used  @typescript-eslint/no-unused-vars
  19:21  error  'updateAnimationDto' is defined but never used  @typescript-eslint/no-unused-vars

/Users/kirigaya/projects/learn-nestjs/src/main.ts
  8:1  warning  Promises must be awaited, end with a call to .catch, end with a call to .then with a rejection handler or be explicitly marked as ignored with the `void` operator  @typescript-eslint/no-floating-promises

/Users/kirigaya/projects/learn-nestjs/src/tea/tea.service.ts
   7:9   error  'createTeaDto' is defined but never used  @typescript-eslint/no-unused-vars
  19:21  error  'updateTeaDto' is defined but never used  @typescript-eslint/no-unused-vars

✖ 5 problems (4 errors, 1 warning)

```

也就是存在申明了但是没有使用的变量，但是这个在日常开发迭代中是非常正常的，所以有的时候我们也会考虑去除这个变量。当然，如果你想要在 nestjs 中体验 gin 的开发体验，加上也无妨 lol

周期较短的项目可以考虑只使用最基本的 pre-commit，即 UT + prettier，甚至什么都不用。

下面就给出一些常用的 pre-commit 检查环节参考。

### 代码复杂度检查

`.eslintrc.json` 或者 `eslint.config.mjs` 中添加规则：

```typescript
{
  "rules": {
    "complexity": ["error", 10] // 限制函数复杂度不超过 10
  }
}
```

:::info 关于CI中的代码复杂度检查
正规的 CI 代码复杂度检查其实更适合使用 Plato 或 CodeMetrics 在提交后的 CI 服务器中执行。此处给出的只是初步检查。
:::

### 依赖检查

确保项目依赖是最新的，并且没有已知的安全漏洞。

```bash
npm audit
```

### 文件命名规范检查

确保文件命名符合项目规范（如 kebab-case、PascalCase 等）。

安装 eslint-plugin-filenames：

```bash
npm install eslint-plugin-filenames --save-dev
```

在 .eslintrc.json 中配置：


```json
{
  "plugins": ["filenames"],
  "rules": {
    "filenames/match-regex": ["error", "^[a-z-]+$", true] // 示例：kebab-case
  }
}
```

### 敏感信息检查

防止提交敏感信息（如 API 密钥、密码等）。

安装 truffleHog：

```bash
pip install truffleHog
```

在 lint-staged 中配置：

```json
{
  "lint-staged": {
    "**/*": [
      "truffleHog --regex --entropy=False ."
    ]
  }
}
```