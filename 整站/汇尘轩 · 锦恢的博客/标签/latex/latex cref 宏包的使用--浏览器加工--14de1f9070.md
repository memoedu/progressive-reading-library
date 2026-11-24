## 前言

今天又忘记 cref 的一些基本使用方法了，简单记录一下。

## cref 介绍

cref 是一款 latex 撰写中很好用的引用器，我们一般引用一个图片，会这么写：

```latex
just as illustrated in fig. \ref{fig:network}
```

但是每次写 `\ref` 前面都要跟着一个 fig.，table.， eq. 之类的前缀，就感觉很麻烦。而 cref 就是为了解决这一件麻烦事准备的。


## 使用

cref 已经根据主题自动设置了选择的 ref 的前缀。我们先引入 cref，一定要在 hyperref 之后引入：

```latex
\usepackage{hyperref}
\hypersetup{  
    colorlinks = true,     % 更改链接颜色  
    linkcolor = blue,      % 链接颜色  
    urlcolor = blue,       % URL 颜色  
    citecolor = blue,      % 引用颜色  
    underline = true,  
    linkbordercolor = red
}

\usepackage{cleveref}
```

使用很简单，下面这样就能达到和之前的 `ref` 一样的用法：

```latex
just as illustrated in \cref{fig:network}
```

如果想要自定义前缀的文字，可以使用 `\crefname` 宏

```latex
\crefname{theorem}{定理}{定理}
\crefname{lemma}{引理}{引理}
\crefname{definition}{定义}{定义}
\crefname{figure}{图}{图}
\crefname{table}{表}{表}
\crefname{algorithm}{算法}{算法}

```

