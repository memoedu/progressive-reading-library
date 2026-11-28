> 最近迷上了浮世绘的绘画风格，虽然描述本身的精神内核没有变化，但是色彩与构图放在19世纪确实非常惊艳。

简单记录一下vscode语言插件开发中的探索。

## 问题描述

`vscode.DefinitionProvider`类用于提供LSP中的定义跳转的上下文信息。当我们在vscode中按下Ctrl+左击时，当前光标所在的位置处的单个单词会被高亮并划下划线。比如下面的场景中，我们试图查看`ExtensionContext`的定义时，我们把光标移动到ExtensionContext单词的范围内，然后按下Ctrl+左击：

<div align=center>
<img src="https://pic1.zhimg.com/80/v2-8737db8b554b2a52f827a0709b653c8b_1440w.png" style="width: 75%;"/>
<p class="img-caption">ExtensionContext被高亮</p>
</div>

你会发现，ExtensionContext高亮并出现下划线，我们把被高亮并下划线的区域称为`link span`。问题来了，当我们开发其他语言的定义跳转时，假设你已经有了AST解析器，要如何自定义`link span`的范围呢？

这一点在路径类型的变量的定义跳转中非常有用，因为有的路径是以`.xxx`结尾的，`.`在vscode内部不是用于形容一个单词的存在。（详见这个函数`document.getWordRangeAtPosition`，其中，`document`是`vscode.TextDocument`）比如`#include "hello.cpp"`，当试图定义跳转时`link span`范围仅限于`hello`，要如何才能让`link span`范围变成`"hello.cpp"`呢？


## 代码实现

一言以蔽之，`vscode.DefinitionProvider`的`provideDefinition`的返回值不仅可以是`vscode.Location`，还可以是`vscode.LocationLink[]`。笔者推荐LSP开发者使用`vscode.LocationLink[]`作为返回值，它的参数更多，功能更加强大。

`vscode.LocationLink`只是一个`interface`，构造出对应的对象。这个对象中有如下的几个指：

- `targetUri`：跳转到的uri
- `targetRange`：跳转到的区域
- `originSelectionRange`：跳转时的`link span`的范围（也就是我们要的解）
- `targetSelectionRange`：没啥用

节约时间，我给出一段我自己写的插件中的定义跳转有关的代码来描述如何使用：

```javascript
...
...  
const includeResult = util.matchInclude(document, position, all.macro.includes);
if (includeResult) {
    const absPath = hdlPath.rel2abs(filePath, includeResult.name);
    const targetFile = vscode.Uri.file(absPath);
    const targetPosition = new vscode.Position(0, 0);
    const targetRange = new vscode.Range(targetPosition, targetPosition);
    const originSelectionRange = document.getWordRangeAtPosition(position, /[\."_0-9a-zA-Z]+/);
    const link: vscode.LocationLink = { targetUri: targetFile, targetRange, originSelectionRange };
    return [link];
}
...
...
```

这段代码的目的是用于让include路径的`link span`的范围扩展到整个双引号包住的区域。其中关键参数是`originSelectionRange `，善用`document.getWordRangeAtPosition`来优雅而快速地获取自定义的`originSelectionRange`。

效果如下：
<div align=center>
<img src="https://picx.zhimg.com/80/v2-547cd49f143766cab02aa5b991df8894_1440w.png" style="width: 35%;"/>
<p class="img-caption">include路径定义跳转</p>
</div>