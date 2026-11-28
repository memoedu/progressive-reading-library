设置超链接和引用的样式是 LaTeX 中非常常见的一个需求，基本用法如下：

```latex
\usepackage{hyperref}
\hypersetup{  
    colorlinks = true,    % 更改链接颜色  
    linkcolor = blue,      % 链接颜色  
    urlcolor = blue,        % URL 颜色  
    citecolor = green,     % 引用颜色  
    underline = true,  
    linkbordercolor = red,  
}
```
这样， `\href{}`, `\cite{}` 和 `\cref{}` 处的引用序号或者链接都会被染色：

```latex
test ref figure \cref{fig:bilateral-filter}

We look up the paper: \cite{buades2011non}

our open source repo: \href{https://github.com/LSTM-Kirigaya/localaplace}{LSTM-Kirigaya/localaplace: implement of Local Laplace Filter algorithm (github.com)} 
```

渲染效果：

<div align=center>
<img src="https://pica.zhimg.com/80/v2-cb84ba696ffe8a1922e4117203aa14e9_1440w.png" style="width: 70%;"/>
</div>
