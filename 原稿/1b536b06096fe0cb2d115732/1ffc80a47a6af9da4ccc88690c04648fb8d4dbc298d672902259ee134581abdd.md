## basic

- ddpm前向负责加噪，无参
- ddpm反向去噪，主要耗时，Unet和attention的load在这儿



## inspiration

前向过程可以考虑不做，或者减小加噪程度，从而用于控制最终图像的去噪效果。

反向主要的问题在于如何在满足某种结构信息基本不变的情况下，实现图像的去噪。所以需要寻找一种度量，可导，而且可以描述图像的结构信息。

---

## control groups

### metrics

数据集: set5

| model | PSNR (db) | SSIM |
| --- | --- | --- |
| ffdnet | 35.17 | 0.9328 |
| dncnn_25 | 34.31 | 0.9173 |
| dpsr_x4_gan | 27.28 | 0.7921 |
| swinIR (SOT) | 37.81 | 0.9539 |

> 对于denoise任务，PSNR和SSIM都是越大性能越好



### current diffusion

如果直接使用`Diffusion`进行，效果非常糟糕：

<div align=center>
<img src="https://picx.zhimg.com/80/v2-15c848a68ff5e1ad9120c7ed338af9f7_1440w.png" style="width: 70%;"/>
<p class="img-caption">alt-diffusion, step=50</p>
</div>

<div align=center>
<img src="https://picx.zhimg.com/80/v2-04f324c05af168ead3a37e9e84ee94f9_1440w.png" style="width: 70%;"/>
<p class="img-caption">GLIDE, RePaint</p>
</div>

所以需要进行改进，重新设计网络结构。

---

## Idea

- 给Diffusion加约束。
- 修改损失函数，考虑去噪后的结构信息。

设计的结构图如下：

<div align=center>
<img src="https://picx.zhimg.com/80/v2-cfeadbc6f33fde845b3da518af1db8a7_1440w.png" style="width: 80%;"/>
<p class="img-caption">设计针对于图像去噪的 Diffusion 的改进模型</p>
</div>

> 推翻上述的结构

最终决定改造原本的反向过程，使用如下的式子进行去噪：

$$
 y_{t-1}=\frac{1}{\sqrt{1-\beta_t}}\left( y_t-\frac{\beta_t}{\sqrt{1 - \gamma_t}}f_\theta(x, y_t, \gamma_t) \right)+\sqrt{\beta_t}\epsilon_t, \quad \epsilon_t\sim\mathcal{N}(0, I)
$$

预测噪音而不是去噪图像本身。

## 扩充实验

idea：

在绪论加入几个实验：

- 原图像的人脸检测，含噪图像的人脸检测（不同噪音等级）
- 原图像的SR，含噪图像的语义分割（不同噪音等级）

在实验部分加入：

- 数据集的分布绘制
- 定性对比实验的放大版本

---

参考链接

- [图像去噪方法总结，持续更新中......](https://zhuanlan.zhihu.com/p/390127131)
- [推荐的Diffusion开源项目](https://www.zhihu.com/question/592464144/answer/2958175656)