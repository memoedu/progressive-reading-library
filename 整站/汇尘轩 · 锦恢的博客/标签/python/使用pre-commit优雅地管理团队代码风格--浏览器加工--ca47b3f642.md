<div align='center'>
<iframe
height=86 width=330
src="//music.163.com/outchain/player?type=2&id=872628&auto=1&height=66"
></iframe>
<p class="img-caption">
灵与肉、虚与实的思考——沙耶之歌</p>
</div>


> 纯氧对生物有害，毫无保留的真相，只会把人的精神击溃。
一比五的氧与氮，才是可供呼吸的空气。
同样，呼吸著以戏言稀释的少量真实，人才能维持健全的心。

最近正好有多个项目都用到了`pre-commit`这个插件，发现在团队开发或者长期项目迭代中，该插件起到了非常有趣的作用。

## pre-commit是什么？

[pre-commit](https://pre-commit.com/)是一个使用Python编写的、用于进行项目代码格式化（码风一致化）的脚本，并且pre-commit可以注册为`git`的一个回调函数。

> 更常见的称呼是“git的hook”。

这个回调函数会在`git commit`前被调用，用于进行项目的码风检查和自动格式化。

## 你为什么需要pre-commit

1. 大家都知道，团队项目最头疼的一点就是码风统一了。并且很多初期加入团队的成员也可能会因为代码风格上的统一导致难以跟进项目。
2. 使用pre-commit也可以减轻CI时reviewer地压力，比如同样一个逻辑，你只是修改了一个语句的代码风格，我们肯定是不希望这个modified出现在PR中的，那么统一的代码格式化脚本可以减少这个现象。

---

## pre-commit使用教程

> 需要注意，`pre-commit`只能在一个git项目中使用，使用前请做好这个心理准备。

### 安装pre-commit脚本

安装方法非常简单，只需要保障你的电脑上存在python和git这两个工具。然后打开命令行，输入如下的命令：

```bash
pip install pre-commit
```


### 在项目中给git注册pre-commit

我们新建一个本地的git项目：
```bash
mkdir PreCommit
cd PreCommit
git init
```
随便新建

然后给这个git项目注册一个pre-commit回调：

```bash
pre-commit install
```

注册完成后，我们需要一个`.pre-commit-config.yaml`文件来定义这个钩子要按照什么风格来格式化代码。

一种可行的代码风格，我放在下面了：
```yaml
repos:
  - repo: https://github.com/pre-commit/mirrors-yapf
    rev: v0.32.0
    hooks:
    - id: yapf
      args: ['--parallel', '--in-place']
  - repo: https://github.com/pre-commit/mirrors-clang-format
    rev: v13.0.1
    hooks:
    - id: clang-format
```


pre-commit的触发条件是git commit使用前，所以我们先commit一次。

```bash
git add .
git commit -am "first commit"
```

commit一回车，pre-commit就会根据配置文件中的信息去下载环境。这个会耗费一定的时间，下载时请关闭所有的网络代理。

下载成功后，pre-commit会自动运行。如果你的项目中存在文件，那么大概率会显示一个Failed信息：

```bash
yapf.....................................................................Failed
- hook id: yapf
- files were modified by this hook
clang-format.........................................(no files to check)Skipped
```

打出failed之后，pre-commit就会帮你把代码整理好。再运行一遍commit就行。


```bash
yapf.....................................................................Passed
clang-format.........................................(no files to check)Skipped
```

> 千万注意，提交PR前一定运行两边git commit，因为pre-commit检查没通过，那么那次的git commit是不会运行的！

除此之外，你也可以运行`pre-commit run --all-files`来手动格式化+检查项目中的所有代码文件。

## 自定义代码风格

通过加入参数就可以完成这个要求，为了方便编写，我们会把参数放在另外一个文件里面。比如我的一个简单的[聊天机器人项目](https://github.com/LSTM-Kirigaya/chatbot-based-on-seq2seq2)中就使用了如下的配置文件：


在文件`.pre-commit-config.yaml`中：

```yaml
repos:
  - repo: https://github.com/pre-commit/mirrors-yapf
    rev: v0.32.0
    hooks:
    - id: yapf
      args: ['--style=.style.yapf', '--parallel', '--in-place']
  - repo: https://github.com/pre-commit/mirrors-clang-format
    rev: v13.0.1
    hooks:
    - id: clang-format
```

在文件`.style.yapf`中：
```yaml
[style]
based_on_style = google
spaces_before_comment = 4
split_before_logical_operator = true
column_limit = 120
```

上面的参数应该非常容易理解，我就不解释了。更多的参数和解析可以参考官网。

这份配置对于python文件是生效，对于其他的语言，不一定生效。如果有需要，需要额外配置一下。反正网上的教程蛮多的，我看cpp、go、js的都有。