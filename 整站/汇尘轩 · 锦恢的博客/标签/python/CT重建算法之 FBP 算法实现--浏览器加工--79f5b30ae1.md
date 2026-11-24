医学影像计算课堂作业，我无法忍耐 Python 的运行速度慢 Matlab 那么多，遂进行改写与优化，运行速度至少提升了1000倍。

## CPU 版本

优化后的 FBP 算法的整套执行程序如下，优化后，运行时间从2000秒下降到4.5秒。

```python
def anti_prj_per_angle(detector_num: int, beta, phi: np.ndarray, D, T, nT0, r, pj_t: np.ndarray):
    th = np.pi / 2 + beta + phi
    s = D * r * np.sin(th) / (D + r * np.cos(th))
    curdet = np.int64((s - nT0) / T + 0.5)
    mask = (curdet > 0) & (curdet < detector_num - 1)
    lam = (s - nT0) / T + 0.5 - curdet
    U = 1 + r * np.sin(beta - phi) / D

    curdet[(curdet + 1) >= pj_t.size] = 0
    pj_t_1 = np.take(pj_t, curdet)
    pj_t_2 = np.take(pj_t, curdet + 1)
    
    I = np.where(mask, ((1 - lam) * pj_t_1 + lam * pj_t_2) / U ** 2, 0)

    return I

def fanfbp(pj: np.ndarray, sp0: np.ndarray, dp0: np.ndarray, gridN: int, gridL: int, theta: np.ndarray) -> np.ndarray:  
  
    # 1.修正投影函数，注意将探测器平移成虚拟探测器
    D = abs(sp0[0])  
    detector_num = len(dp0[0])  # 探测器个数
    angle_num = len(theta)  # 角度数  
    nT = dp0[1] * D / (D + dp0[0])  # 虚拟探测器的纵坐标  
    T = nT[1] - nT[0]  # 虚拟探测器间隔  

    pj = pj * D / np.sqrt(D ** 2 + nT ** 2)
    pj = pj * (abs(theta[1] - theta[0]))  
  
    # 2.卷积核（RL kernel h_RL和 SL Kernel h_SL）  
    # h_RL = ... (需要提供具体的卷积核函数)  
  
    # 预先计算所有点的极坐标  
    r, phi = getrphi(np.arange(gridN ** 2), gridN, gridL)  
    
    # 3.卷积加权反投影
    parallel = Parallel(n_jobs=-1)
    results = parallel(
        delayed(anti_prj_per_angle)(detector_num, theta[t] - np.pi / 2, phi, D[0], T, nT[0], r, pj[t])
        for t in range(angle_num)
    )

    return sum(results)

```

最终得到的 `test.png` 如下图所示：
<div align=center>
<img src="https://pic1.zhimg.com/80/v2-d7982cf1ec3af42cb6253d96a189fbc4_1440w.png" style="width: 500px;"/>
</div>

---

## GPU 版本

增设 GPU 版本，运行耗时进行下降到0.5秒。

```python
def fanfbp(pj: torch.Tensor, sp0: torch.Tensor, dp0: torch.Tensor, gridN: int, gridL: int, theta: torch.Tensor) -> torch.Tensor:  
  
    # 1.修正投影函数，注意将探测器平移成虚拟探测器  
    D = abs(sp0[0])  
    detector_num = len(dp0[0])  # 探测器个数
    angle_num = len(theta)  # 角度数  
    nT = dp0[1] * D / (D + dp0[0])  # 虚拟探测器的纵坐标  
    T = nT[1] - nT[0]  # 虚拟探测器间隔

    print(detector_num)

    pj = pj * D / torch.sqrt(D ** 2 + nT ** 2).cuda()
    pj = pj * (torch.abs(theta[1] - theta[0]))  

    # 2.卷积核（RL kernel h_RL和 SL Kernel h_SL）  
    # h_RL = ... (需要提供具体的卷积核函数)  
  
    r, phi = getrphi(np.arange(gridN ** 2), gridN, gridL)

    r = convert_tensor(r)
    phi = convert_tensor(phi)

    # 化为 三阶张量，一网打尽
    phi = phi.expand(angle_num, *phi.shape)
    r = r.expand(angle_num, *r.shape)
    beta = theta - torch.pi / 2
    beta = beta[:, None, None].expand(len(beta), gridN, gridN)

    th = torch.pi / 2 + beta + phi
    s = D * r * torch.sin(th) / (D + r * torch.cos(th))
    curdet = torch.floor((s - nT[0]) / T + 0.5).type(torch.int64)
    lam = (s - nT[0]) / T + 0.5 - curdet
    U = 1 + r * torch.sin(beta - phi) / D

    mask = (curdet > 0) & (curdet < detector_num - 1)
    curdet = curdet.clip(0, detector_num - 2)

    origin_shape = curdet.shape
    pj_curdet = pj.gather(1, curdet.reshape(angle_num, -1)).reshape(origin_shape)
    pj_curdet_1 = pj.gather(1, (curdet + 1).reshape(angle_num, -1)).reshape(origin_shape)

    I_s = torch.where(mask, ((1 - lam) * pj_curdet + lam * pj_curdet_1) / U ** 2, 0)

    return I_s.sum(axis=0)

def convert_tensor(a: np.ndarray) -> torch.Tensor:
    return torch.tensor(a).cuda()
```
---

## 滤波器
可以看到，之前重建出来的图像有点糊，CT 重建常用的滤波器是RL和SL。

> 其中 n 是探测器的个数，T是探测器之间的距离

RL 滤波器的公式如下：

$$
h_{R-L}(n, T)=\begin{cases}
\frac{1}{4T^2} & n=0 \\
0 & \text{n is even} \\
-\frac{1}{(n\pi T)^2} & \text{n is odd} 
\end{cases}
$$

SL 滤波器的公式如下：

$$
h_{S-L}(n, T)=-\frac{2}{\pi^2T^2(4n^2-1)}
$$

代码上实现时，得到的滤波器系数数组实际上会和投影信号之间进行卷积，这个可以看成是一个滤波操作。因此，需要保证滤波器组是对称的，且中心点的系数最大，这样才能比较好地在滤波的同时，留住原本的响应值。生成滤波器系数的代码如下：

```python
@numba.njit()
def make_RL_kernel(detector_num, T):
    kernel = np.zeros(detector_num)
    for i in range(detector_num):
        if i == detector_num // 2 - 1:
            kernel[i] = 1 / (4 * T ** 2)
        elif i % 2 == 1:
            kernel[i] = -1 / ((i - detector_num // 2 + 1) * np.pi * T) ** 2
    return kernel

@numba.njit()
def make_SL_kernel(detector_num, T):
    indices = np.arange(detector_num) - detector_num // 2 + 1
    kernel = - 2 / (np.pi * np.pi * T * T * (4 * indices * indices - 1))
    return kernel
```

使用时，只需要在输入`anti_prj_per_angle`前对投影进行卷积即可，这里以普通的按照循环写的代码为例，循环向量化请自己改：

```python
    # 选择你的卷积核
    kernel = make_RL_kernel(detector_num, T)
    I = np.zeros((gridN, gridN))

    for t in range(angle_num):
        convres = np.convolve(pj[t], kernel, mode='full')
        # 要从 detector_num // 2 - 1 处开始取值，因为构造的滤波器的 detector_num // 2 - 1 刚好是中心，两边对称
        pj_t = convres[detector_num // 2 - 1: detector_num // 2 - 1 + len(pj[t])]
        I += anti_prj_per_angle(gridN, theta[t] - np.pi / 2, phi, D[0], T, nT[0], r, pj_t)

    return I
```

最终的效果如下：
<div align=center>
<img src=" https://pic1.zhimg.com/80/v2-0b405493f170f1d6f4365fc88cd0bdb8_1440w.png" style="width: 500px;"/>
</div>

是不是清楚了很多？