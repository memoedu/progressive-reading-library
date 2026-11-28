三年前写的文章了，今天用 crawl4ai 转成 markdown 搬到我的网站上来了- [来自专栏 · 深度学习工程化](https://www.zhihu.com/column/c_1502694618957717504)

---

## 前言
随着AI算法和浏览器OS的逐步成熟，B/S架构逐渐成为了整个AI应用的主要实现方式，并且服务器成为了神经网络推理模型的主战场。但是不是所有人都能买得起带大显存GPU的服务器，而且很多做事优雅的后端工程师也非常拒绝带有大量运行时的Python程序作为他们的服务请求的对象。C/C++才是有梦想的程序员部署AI应用的首选。

那么在PyTorch如此流行的当下，有什么方法可以做到使用C++部署你已经训练好的PyTorch模型吗？方法有很多——可以使用ONNX转tensorRT；可以自己写个脚本把PyTorch模型参数注入tensorflow中，然后用tensorflow的部署工具链来部署。当然，这些都是非常优秀的部署方法，并且甚至逐步被工业界采用。今天锦恢介绍的方法相对来说上手难度更低，也就是使用PyTorch的C++版本LibTorch来重写推理代码，并用JIT跟踪来得到LibTorch可以直接读取的模型参数。

相信这会非常好玩的。

---

## 实验环境
  * Ubuntu18.04 (Win11 WSL2)
  * Python 3.8.10
  * cmake 3.16.3
  * gcc 9.4.0
  * Vscode 最新版本

首先确保上述依赖项都安装完成

---

## 安装 C++ 库

### opencv

对于很多图像相关的任务，C++中需要opencv来做图像IO和基本操作，因此建议装一下opencv。不是很建议直接用包管理器装，版本兼容性会出现问题。建议直接源码安装，具体做法网上教程一大堆，由于我之前装过，此处不再赘述，给一篇曾经参考过的文章：

[](https://link.zhihu.com/?target=https%3A//blog.csdn.net/weixin_44384491/article/details/121142093)

### libtorch

这就是PyTorch的C++接口，安装非常简单，先去PyTorch的官网，根据你的平台下载对应版本的：
![](https://pic1.zhimg.com/v2-4cf0d36cb66f4f28c20de5e6d75354de_1440w.jpg)

:::info
下载的libtorch的版本最好和你的pytorch的版本一致。Linux下各个libtorch的release版本的下载链接可以在下面这篇文章中找到：
[https://blog.csdn.net/liang_baikai/article/details/127849577](https://blog.csdn.net/liang_baikai/article/details/127849577)
:::

下载完成后，随便丢到一个地方去解压，完成。比如我是习惯性放在/usr/local/lib下的。我也建议linux小白将libtorch放在/usr/local/lib下，并保证libtorch文件夹下存在include这个文件夹。

---

## 配置CMakeList.txt

打开一个工作目录，开始配置我们的CMakeLists.txt，文件目录如下：

```
📁.vscode
📁build
📃CMakeLists.txt
📃digit.cpp
📃digit.py
```

.vscode 配置项就不说了，我以前很多篇文章都讲过如何配置vscode的cpp环境
假设我们下.面需要编译的入口文件是digit.cpp：

```cmake
cmake_minimum_required(VERSION 3.0 FATAL_ERROR)project(LibTorchDemo)# compile options
set(CMAKE_CXX_FLAGS_RELEASE "-O3")set(CMAKE_CXX_STANDARD 14)# package
find_package(OpenCV REQUIRED)find_package(Torch REQUIRED PATHS "/usr/local/lib/libtorch")add_executable(digit digit.cpp)# libtorch
target_link_libraries(digit ${TORCH_LIBRARIES})target_link_libraries(digit ${OpenCV_LIBS})
```

/usr/local/lib/libtorch Torch 的 package 路径根据你的安装目录指定
然后写个文件 include 一下库，文件名为 digit.cpp：

```cpp
#include "iostream"#include "opencv2/opencv.hpp"#include "torch/script.h"int main(int argc, char const *argv[])
{
    std::cout << "hello world!" << std::endl;
    return 0;
}

```

编译一把：
```bash
cd build
cmake .. && make -j8
```

出现下图编译成功，说明依赖项准备就绪了：
![](https://pic4.zhimg.com/v2-09ab742c61b0af3046da610b29d2c7a5_1440w.jpg)

:::info
如果出现错误，基本都是找不到头文件或者静态库，如果找不到头文件，在CMakeLists.txt中include_directories()中添加能够搜索到你在cpp中写的相对路径的根目录路径。如果静态库找不到，请检查安装包是否损坏，或者静态库目录是否在gcc的搜索路径中。
:::

---

## 第一步：先用PyTorch训练一个网络
既然我们需要将PyTorch模型使用C++部署，那么首先肯定需要一个Torch的模型。我们先使用PyTorch简单训练一个手写数字识别，相信看这篇文章的靓仔都是torch老手了，我直接上代码：
> 如果你已经有一个模型文件了，请直接跳转到第二步
```python
from sklearn.datasets import load_digits
import torch
from torch import nn
import torch.utils.data as Data
import numpy as np
from sklearn.metrics import accuracy_score
import matplotlib.pyplot as plt
import os

class Digit(nn.Module):
    def __init__(self):
        super().__init__()
        self.conv = nn.Sequential(
            nn.Conv2d(1, 16, 3, 1, 1),
            nn.Tanh(),
            nn.Conv2d(16, 32, 3, 2, 1),
            nn.Tanh(),
            nn.Conv2d(32, 16, 3, 2, 1),
            nn.Tanh(),
            nn.Conv2d(16, 8, 3, 1, 1)
        )

        self.output = nn.Linear(32, 10)

    def forward(self, x):
        out = self.conv(x)
        out = self.output(out.flatten(1))
        return out

RATIO = 0.8
BATCH_SIZE = 128
EPOCH = 10

if __name__ == "__main__":
    X, y = load_digits(return_X_y=True)
    X = X / 16.
    sample_num = len(y)
    X = [x.reshape(1, 8, 8).tolist() for x in X]

    indice = np.arange(sample_num)
    np.random.shuffle(indice)

    X = torch.FloatTensor(X)
    y = torch.LongTensor(y)
    offline = int(sample_num * RATIO)

    train = Data.TensorDataset(X[indice[:offline]], y[indice[:offline]])
    test  = Data.TensorDataset(X[indice[offline:]], y[indice[offline:]])

    train_loader = Data.DataLoader(train, BATCH_SIZE, True)
    test_loader  = Data.DataLoader(test,  BATCH_SIZE, False)

    model = Digit()
    optimizer = torch.optim.RMSprop(model.parameters(), lr=1e-3)
    criterion = nn.CrossEntropyLoss(reduction="mean")

    test_losses = []
    test_accs = []

    for epoch in range(EPOCH):
        model.train()
        for bx, by in train_loader:
            out = model(bx)
            loss = criterion(out, by)
            optimizer.zero_grad()
            loss.backward()
            optimizer.step()

        model.eval()
        correct = 0
        total = 0
        test_loss = []
        test_acc = []
        for bx, by in test_loader:
            with torch.no_grad():
                out = model(bx)
                pre_lab = out.argmax(1)
                loss = criterion(out, by)

            test_loss.append(loss.item())
            test_acc.append(accuracy_score(pre_lab, by))

        test_losses.append(np.mean(test_loss))
        test_accs.append(np.mean(test_acc))

    plt.figure(dpi=120)
    plt.plot(test_losses, 'o-', label="loss")
    plt.plot(test_accs, 'o-', label="accuracy")
    plt.legend()
    plt.grid()
    plt.show()

    if not os.path.exists("model"):
        os.makedirs("model")
    torch.save(model.state_dict(), "model/digit.pth")
```

测试集精度和损失曲线如下：

![](https://pic3.zhimg.com/v2-18dd79329acaee4e2d7f537f38ce7176_1440w.jpg)

运行结束后可以得到模型文件，就在model文件夹中。

---

## 第二步：使用tracing将模型文件转化成TorchScript

PyTorch导出的模型文件是不能直接被libtorch读取的，因为PyTorch默认导出的后端的序列化是joblib。PyTorch通过JIT搭建了Python和C++的桥梁，我们可以将模型转成TorchScript Module，将Python运行时的部分运行时包裹进去。

转换方法非常简单：

```python
import torch
from digit import Digit

model = Digit()
model.load_state_dict(torch.load("model/digit.pth", map_location="cpu"))

sample = torch.randn(1, 1, 8, 8)

trace_model = torch.jit.trace(model, sample)
trace_model.save("model/digit.jit")
```

原理大概就是模拟一个输入sample，让模型走一遍前向推理，将state_dict中的每个参数跟踪一边。因此sample只需要是一个符合输入规范的张量即可。

运行上述代码，可以在model文件夹中得到digit.jit。这个jit文件可以直接被python使用，下面我就来使用JIT跟踪的模型来进行Python端的预测，一来验证我们的JIT跟踪得到的TorchScript模型是否可以正常工作，二来比较JIT会对模型推理的加速起到多大的效果。

我们先画一张图（后面也会用到）

![](https://picx.zhimg.com/v2-28168db8c40b5138e8ed1d4678a2f23d_1440w.jpg)

然后截图，保存到image/sample.png。

运行下述测试代码，由于Python本身的特性和JIT的即时编译的特性，模型在同一进程生命周期内运行时前几次会比较慢，所以在测试前，需要空跑几次：

```python
import time
import torch
import cv2 as cv
from digit import Digit
import matplotlib.pyplot as plt
import seaborn as sns
import pandas as pd

def run_model(model, image):
    s = time.time()
    out = model(image)
    pre_lab = torch.argmax(out, dim=1)
    cost_time = round(time.time() - s, 5)
    return cost_time

image = cv.imread("image/sample.png")
image = cv.cvtColor(image, cv.COLOR_BGR2GRAY)
image = 1 - image / 255.
image = cv.resize(image, (8, 8))


image = torch.FloatTensor(image).unsqueeze(0).unsqueeze(0).contiguous()
origin_model = Digit()
origin_model.load_state_dict(torch.load("model/digit.pth"))
jit_model = torch.jit.load("model/digit.jit")

# init jit
for _ in range(3):
    run_model(origin_model, image)
    run_model(jit_model, image)

test_times = 10

# begin testing
results = pd.DataFrame({
    "type" : ["orgin"] * test_times + ["jit"] * test_times,
    "cost_time" : [run_model(origin_model, image) for _ in range(test_times)] + [run_model(jit_model, image) for _ in range(test_times)]
})

plt.figure(dpi=120)
sns.boxplot(
    x=results["type"],
    y=results["cost_time"]
)
plt.show()
```

性能箱图如下：

![](https://picx.zhimg.com/v2-d1ba1d3d262d59336beb21fbb94656ab_1440w.jpg)

可以看到，JIT的推理速度比原本的有些许的提升。我们的TorchScript模型也可以正常工作。

---

## 第三步：使用libtorch重写推理程序

由于TorchScript可以被C++直接调用，所以我们只需要使用libtorch重写推理代码，并将模型读入就完成了。

libtorch的语法和PyTorch基本一致，学起来很快，于此锦恢就不再赘述了。相应的，在C++中，我们用cv::Mat来取代Python中的numpy.ndarray对象，如何将cv::Mat转成libtorch可以读入的数据结构也会在demo中涉及。

下面的例子会完成一个C++命令行程序，它的第一个参数为模型，第二个参数为需要读入的手写数字图像的路径，预测结果会打印到控制台上。期待已久的C++代码如下：

```cpp
#include "iostream"#include "opencv2/opencv.hpp"#include "torch/script.h"#include "fstream"void checkPath(const char* path) {
    std::ifstream in;
    in.open(path);
    bool flag = (bool)in;
    in.close();
    if (flag) return;
    else {
        std::cout << "file " << path << " doesn't exist!" << std::endl;
        exit(-1);
    }
}

int main(int argc, char const *argv[])
{
    if (argc != 3) {
        std::cout << "usage : digit <model path> <image path>" << std::endl;
        return -1;
    }

    checkPath(argv[1]);
    checkPath(argv[2]);
    cv::Mat img = cv::imread(argv[2]), gimg, fimg, rimg;
    cv::cvtColor(img, gimg, CV_BGR2GRAY);

    gimg.convertTo(fimg, CV_32F, - 1. / 255., 1.);
    cv::resize(fimg, rimg, {8, 8});

    // convert Mat to tensor
at::Tensor img_tensor = torch::from_blob(
        rimg.data,
        {1, 1, 8, 8},
        torch::kFloat32
    );

    // load model
torch::jit::Module model = torch::jit::load(argv[1]);

    // torch.no_grad()
torch::NoGradGuard no_grad;

    // forward
torch::Tensor out = model({img_tensor}).toTensor();
    int pre_lab = torch::argmax(out, 1).item().toInt();

    std::cout << "predict number is " << pre_lab << std::endl;
    return 0;
}

```

:::info
请一定加入 `torch::NoGradGuard no_grad;` 这句话，否则内存会炸。之前和群友 Priesia （深度学习领域部署大师，DragonianLib 作者） 聊过，他的意思是，这个是 libtorch 的引用计数器的 bug
:::

然后我们编译一把：

```bash
cd build && cmake .. && make -j8 && cd ..
```

最终测试一下，先准备两张图片，sample.png和sample2.png。sample.png就是上面那张数字6，我们再准备一张数字2（建议画粗一点，一定要白底黑字），我们打开命令行测试一下效果：

![](https://pic1.zhimg.com/v2-1b8be9cba0fe4bb8411c6dc190f7e9f0_1440w.jpg)

可以看到，成功了。结束，开摆 :D。