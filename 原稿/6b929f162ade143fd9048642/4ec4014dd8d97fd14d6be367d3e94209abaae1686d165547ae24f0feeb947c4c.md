在 latex 文章中写代码是一个非常常见的需求。其中，不少人都很喜欢的且非常优雅的就是 minted 宏包了。

## minted 的基本用法

在你的latex项目中通过如下引入 minted：

```latex
\usepackage{minted}
```

正文中使用如下：

```latex
\begin{minted}[frame=single, linenos=true]{python}
>>> import numpy as np
>>> from scipy import stats
>>> x = np.array([61, 70, 69, 63, 75, 62, 70, 66])
>>> y = np.array([55, 71, 67, 61, 70, 61, 71, 61])
>>> _, p = stats.ttest_1samp(x - y, 0)
>>> p
0.042982513619168904
\end{minted}
```

其中参数 `frame` 代表代码块的边框样式，默认是无，`linenos` 代表是否显示行号。花括号后面的 `{python}` 代表我们下面的代码是 python，如果你需要使用 C++ 或者Java，就改成 `{c++}, {java}`。渲染效果如下：

<div align=center>
<img src="https://pic1.zhimg.com/80/v2-5ac17dcee1af88e830d25f7e3b404d81_1440w.png" style="width: 60%;"/>
<p class="img-caption">minted 渲染效果</p>
</div>

更多关于 `minted` 宏包的使用说明和参数，可以在命令行中输入如下指令来打开 `minted` 的文档：

```bash
$ texdoc minted
```

## minted 进阶

光是上面的使用体验，个人感觉不是很好。社区中，有人提出了可以结合 `tcolorbx` 宏包做出更加好看的效果。

我们先引入必要的宏包：

```latex
\usepackage{minted}
\usepackage{xcolor}
\usepackage{tcolorbox}
\tcbuselibrary{skins}
\tcbuselibrary{minted}
\usemintedstyle{lovelace}
```

> 其中，最后一行的 lovelace 是代码的高亮样式，如果你喜欢 minted 默认的高亮样式，可以去除最后一样的引入

正文中如下使用：
```latex
\begin{tcblisting}{listing engine=minted,boxrule=0.1mm,
colback=blue!5!white,colframe=blue!75!black,
listing only,left=5mm,
minted language=python,
minted options={fontsize=\small,breaklines, autogobble,linenos,numbersep=3mm}}
def mat2col(mat: np.ndarray, ksize: int, stride:int=1) -> np.ndarray:
    assert len(mat.shape) == 2
    h, w = mat.shape    
    out_h = (h - ksize) // stride + 1
    out_w = (w - ksize) // stride + 1
    beta_0 = np.arange(ksize)
    beta_i = np.arange(out_h * out_w)
    alpha_0_0 = (beta_i // out_w) * w + (beta_i % out_w) * stride
    alpha_0 = beta_0 + alpha_0_0[..., None].repeat(ksize, axis=1)
    alpha_i = np.arange(ksize) * w
    alpha_0 = np.tile(alpha_0, ksize)
    alpha_i = np.repeat(alpha_i, ksize)
    alpha = alpha_0 + alpha_i
    col = np.take(mat.flatten(), alpha)
    return col
\end{tcblisting}
```

渲染效果：

<div align=center>
<img src="https://picx.zhimg.com/80/v2-f0f12b7155a4d6da99f1a71c4afbe40a_1440w.png" style="width: 60%;"/>
<p class="img-caption">更加花哨的 minted 渲染效果</p>
</div>



## 参考资料

- [https://blog.csdn.net/xenonhu/article/details/88978672](https://blog.csdn.net/xenonhu/article/details/88978672)