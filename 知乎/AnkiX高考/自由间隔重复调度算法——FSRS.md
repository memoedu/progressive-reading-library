# 自由间隔重复调度算法——FSRS

> [原文链接](https://zhuanlan.zhihu.com/p/454932406)

上周和太记作者 [@林一二](https://www.zhihu.com/people/16c82891a98d1e3e49d07a9c75aae4dc) 、钓鱼插件作者 [@凹夫几](https://www.zhihu.com/people/7c856f3f6b55bcbd409bfa51ee891d59) 讨论了一下开源间隔重复的未来，于是抽空写了一个间隔重复算法：[open-spaced-repetition/free-spaced-repetition-scheduler](https://link.zhihu.com/?target=https%3A//github.com/open-spaced-repetition/free-spaced-repetition-scheduler)

这里简单介绍一下。

## 名称中的「自由」是什么意思？

本算法支持自由复习，可以提前，也可以推迟，算法会根据记忆模型进行适应。

同时，间隔重复是实现自由学习[1]的基础技术之一。而且算法可以在本地运行，无需担心泄露自己的学习数据。

## 算法的原理是什么？

本算法基于 SuperMemo 作者 [Piotr Wozniak](https://link.zhihu.com/?target=https%3A//supermemo.guru/wiki/Piotr_Wozniak)[2] 提出的 [DSR 模型](https://link.zhihu.com/?target=https%3A//supermemo.guru/wiki/Two_components_of_memory)开发。

该模型考虑了影响记忆的三个变量：难度（difficulty），稳定性（stability）和可提取性（retrievability）。

稳定性指的是记忆的存储强度，越高，记忆遗忘得越慢。可提取性指的是记忆的检索强度，越低，记忆遗忘的概率越高。

在本模型中，考虑了以下记忆规律：

记忆材料越难，记忆稳定性增长越慢
记忆稳定性越高，记忆稳定性增长越慢（又称为[记忆稳定化衰减](https://link.zhihu.com/?target=https%3A//supermemo.guru/wiki/Stabilization_decay)）
记忆可提取性越低，记忆稳定性增长越快（又称为[记忆稳定化曲线](https://link.zhihu.com/?target=https%3A//supermemo.guru/wiki/Stabilization_curve)）

## 能简单描述一下算法吗？

难度范围 
![](https://www.zhihu.com/equation?tex=D%5Cin+%5B1%2C10%5D)
 
初始难度 
![](https://www.zhihu.com/equation?tex=D_0+%3D+5)
 
初始稳定性 
![](https://www.zhihu.com/equation?tex=S_0+%3D+2)
 
指数遗忘曲线模型 
![](https://www.zhihu.com/equation?tex=R+%3D+%5Cexp%28%5Cln+0.9+%5Ccdot+%5Ccfrac%7BI%7D%7BS%7D%29)
 
复习成功后稳定性更新公式 
![](https://www.zhihu.com/equation?tex=S%5E%5Cprime%3DS%5Ccdot+%281+%2B+a+%5Ctimes+D+%5E+%7B-b%7D+%5Ctimes+S%5E%7B-c%7D+%5Ctimes+%28%5Cexp%281+-+R%29-1%29%29)
 

稳定性增长系数 a = 60
难度衰减系数 b = 0.7
稳定性衰减系数 c = 0.2

复习失败后的稳定性 
![](https://www.zhihu.com/equation?tex=S_%7BL%7D+%3D+S_0%5E%7B-f%5Ccdot+L%7D)
 

遗忘次数 
![](https://www.zhihu.com/equation?tex=L+%3E%3D+0)
 
遗忘衰减系数 f = 0.3

复习后难度更新公式 
![](https://www.zhihu.com/equation?tex=D%5E%5Cprime+%3D+D+-+d%5Ccdot%28Grade+-+1%29+-+e%5Ccdot%281-R%29)
 

评分范围 
![](https://www.zhihu.com/equation?tex=Grade%5Cin+%5C%7B0%2C1%2C2%5C%7D)
 

0 表示遗忘（复习失败）
1 表示记住（复习成功）
2 表示容易（复习成功）

评分影响系数 d = 1
可提取性影响系数 e = 1

初始稳定性自适应 
![](https://www.zhihu.com/equation?tex=S_0+%3D+%5Ccfrac%7B%5Cln+0.9%7D%7B%5Ccfrac%7B%5Csum%5Climits_i%5En+%5Cln+R_i+%5Ctimes+I_i%5Ctimes+cnt_i%7D%7B%5Csum%5Climits_i%5En+I_i%5E2%5Ctimes+++cnt_i%7D%7D)
 

即对第一次复习到遗忘曲线进行线性回归，计算实际的稳定性

初始难度自适应 
![](https://www.zhihu.com/equation?tex=D_0%5E%7B%5Cprime%7D+%3D+%5Ccfrac%7B%5Cln+R_t%7D%7B%5Cln+R_c%7D%5E%7B%5Cfrac%7B1%7D%7B-b%7D%7D+%5Ctimes+D_0)
 

目标保留率 
![](https://www.zhihu.com/equation?tex=R_t)
 
目前保留率 
![](https://www.zhihu.com/equation?tex=R_c)
 
如果 
![](https://www.zhihu.com/equation?tex=R_c+%3C+R_t)
 ，就提高初始难度，这样新卡片的复习间隔增长速度就会下降，反之亦然

## 有现成的算法库吗？

钓鱼插件实现了该算法的 JavaScript 版本：[fishing/action-fishing.js](https://link.zhihu.com/?target=https%3A//github.com/oflg/fishing/blob/master/action-fishing.js)，本项目中的 simulator.py 中实现了 Python 版本。

该算法尚未稳定，还有待收集数据验证。很多参数是人工设置的，尚未实现自适应，所以目前没有提供算法库。

## 我可以使用该算法吗？

可以，但请在项目中指向本仓库。

字虽不多，但求有意。最近还得忙下一篇论文的算法，就不一一展开介绍了，感兴趣的朋友直接 fork 一份代码自己玩吧～
