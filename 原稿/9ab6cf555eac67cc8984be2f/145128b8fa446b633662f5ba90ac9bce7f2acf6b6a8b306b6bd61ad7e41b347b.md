## 前言

最近一个好哥们的组采购了一批新卡，其中出现了AGX Orin系列的嵌入式开发板，这是一种 Nvidia 专门给端侧深度学习模型部署的开发板。

<div align=center>
<img src="https://pic1.zhimg.com/80/v2-d9c1d174a46ce9777a11bb46f0fb949d_1440w.png" style="width: 30%;"/>
</div>

然后哥们在底层驱动安装上遇到了一些麻烦，于是找我帮忙，顺带让我也来玩玩这块未来会安装在他们的机械臂上的上位机开发板。

> 没错，他们是做高速通信芯片，业余顺带大家一起做做电机驱动板。

---

## 初始化 Ubuntu

先完成 Ubuntu aarch64 的基本初始化，也就是给 apt 换源，然后运行下面的语句，加入 nvidia 的驱动源：

```bash
sudo bash -c 'echo "deb https://repo.download.nvidia.com/jetson/common r34.1 main" >> /etc/apt/sources.list.d/nvidia-l4t-apt-source.list'
sudo bash -c 'echo "deb https://repo.download.nvidia.com/jetson/t234 r34.1 main" >> /etc/apt/sources.list.d/nvidia-l4t-apt-source.list'
```

然后更新一下系统软件：

```bash
sudo apt update
sudo apt dist-upgrade
sudo reboot
sudo apt install nvidia-jetpack
```

然后可以安装 miniconda 或者 anaconda，当然，你使用 Ubuntu 自带的 python 也是没问题的。此处建议使用 python 3.10（否则你就要自己编译 pytorch 了）

---

## 安装 pytorch 环境

需要注意，orin 平台上的 pytorch 并不是一般的架构下的 torch，anaconda 和 PyPi 的存储库中都没有合适的编译版本。获取 orin 平台上的 torch 一共有两种方法。

### 方法一：从官网下载

最简单的自然从官网下载对应版本的，官方已经编译好了一组 ABI，就放在 https://forums.developer.nvidia.com/t/pytorch-for-jetson/72048 里面。

下载之前，我们先查看一下 Jetpack 的版本号：

```bash
sudo apt-cache show nvidia-jetpack | grep Version
```

输出：

```
Version: 6.0+b106
Version: 6.0+b87
Version: 5.0.1-b118
```

可以看到，当前驱动支持到 6.0 版本，那么我们就下载官网上最新版本的 whl 文件：

```bash
mkdir -p ~/download && cd ~/download
wget -o torch-2.3.0-cp310-cp310-linux_aarch64.whl -c https://nvidia.box.com/shared/static/mp164asf3sceb570wvjsrezk1p4ftj8t.whl
wget -o torchaudio-2.3.0+952ea74-cp310-cp310-linux_aarch64.whl -c https://nvidia.box.com/shared/static/9agsjfee0my4sxckdpuk9x9gt8agvjje.whl
wget -o torchvision-0.18.0a0+6043bc2-cp310-cp310-linux_aarch64.whl -c https://nvidia.box.com/shared/static/xpr06qe6ql3l6rj22cu3c45tz1wzi36p.whl
```

安装前记得先卸载本地的 torch，torchvision 和 torchaudio：

```bash
pip uninstall torch torchaudio torchvision
pip install torch-2.3.0-cp310-cp310-linux_aarch64.whl
pip install torchaudio-2.3.0+952ea74-cp310-cp310-linux_aarch64.whl
pip install torchvision-0.18.0a0+6043bc2-cp310-cp310-linux_aarch64.whl
```

安装结束后，记得验证一下 cuda 是否可用：

```python
python -c "import torch;print(torch.cuda.is_available())"
```

输出：

```
True
```

说明当前 cuda 环境已经没有问题了。

:::info

orin 属于 Jetson 的一员，因为 Jetson 的特殊性，orin 上常规的 nvidia-smi 和 nvitop 工具是没有用的，上面无法显示正确的GPU状态。

:::

### 方法二：手动编译

我们还可以自己去手动编译 torch，先递归下载 pytorch 源代码和对应的第三方依赖库代码（onnx，pybind11，gemm等），下面的过程占用时间较长：

```bash
git clone --recursive --branch v2.0.1 http://github.com/pytorch/pytorch
```

然后准备一下基础编译工具、blas 驱动开发库和 openmpi 驱动开发库：

```bash
sudo apt-get install cmake libopenblas-dev libopenmpi-dev
```

检查一下 cmake 版本：

```bash
cmake -version
```

如果展示的版本小于 3.18 那么再安装如下的 python 库：

```bash
pip install scikit-build ninja cmake
```

然后我们进入项目开始准备编译吧，首先先安装基本的 python 库：

```bash
cd pytorch
pip install -r requirements.txt
```

创建 `build_orin.sh` 脚本，配置一下编译 orin 上的· pytorch 的基本参数：

```bash
# build_orin.sh
#!/bin/bash

export USE_NCCL=0
export USE_DISTRIBUTED=1
export USE_QNNPACK=0
export USE_PYTORCH_QNNPACK=0

# Orin 是 安培架构的，所以第一个版本用 8
export TORCH_CUDA_ARCH_LIST="8.7"

export PYTORCH_BUILD_VERSION=2.0.1
export PYTORCH_BUILD_NUMBER=1

python3 setup.py bdist_wheel
```

然后开始编译，编译时间较长，建议挂载 screen 或者 tmux 后台进行控制。

```bash
sh build_orin.sh
```

<div align=center>
<img src="https://picx.zhimg.com/80/v2-7d3f39b5fcbf0def5966eeff16143578_1440w.png" style="width: 70%;"/>
</div>

编译结束后，ABI 生成在 dist 文件夹下，我们直接安装即可，安装之前，先卸载现有的 torch torchvision torchaudio

```bash
pip uninstall torch torchaudio torchvision
pip install dist/torch-2.0.1-cp310-cp310-linux_aarch64.whl
```

安装结束后，记得验证一下 cuda 是否可用：

```python
python -c "import torch;print(torch.cuda.is_available())"
```

输出：

```
True
```

说明当前 cuda 环境已经没有问题了。

---

## 部署 QWEN2.0 大模型

先看看显存：

```python
>>> import torch
>>> torch.cuda.get_device_properties(0).total_memory / 1024 / 1024 / 1024
61.368560791015625
```

可以看到，一共有 61 G 的显存，对于 Jetson 这个嵌入式系统而言，这个配置可以说非常爽了。

为了好玩，我们不妨在 板子 上部署一个大模型吧！

这里选择了口碑比较不错的 qwen2.0，只需要几行代码，我们就可以直接完成大模型的部署和推理，先安装基本的库：

```bash
pip install transformers bs4
```

然后通过下面的代码获取下载 qwen2.0 的权重文件的下载命令：

```python
# fetch-huggingface.py
import requests as r
from bs4 import BeautifulSoup
import argparse

parser = argparse.ArgumentParser()
parser.add_argument('--url')
args = parser.parse_args()

url = args.url
headers = {
    'User-Agent': 'Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/127.0.0.0 Safari/537.36'
}

hf = 'https://huggingface.co'
def get_repo_resource(url):
    res = r.get(url, headers=headers)
    resource = []
    if res.status_code == 200:
        html = res.text
        soup = BeautifulSoup(res.text, 'html.parser')

        for tag in soup.find_all('a', attrs={'class': 'group'}):
            title: str = tag.attrs.get('title', '').lower()
            if title.startswith('download'):
                href: str = tag.attrs['href']
                filename = href.split('?')[0].split('/')[-1]
                resource.append((filename, hf + href))
    return resource

commands = []
for filename, href in get_repo_resource(url):
    commands.append('wget -O {} -c {}'.format(filename, href))
print(' && '.join(commands))
```

qwen2.0 的 hf 的 LFS 仓库地址为：https://huggingface.co/Qwen/Qwen2-7B-Instruct/tree/main，所以输入：

```bash
python fetch-huggingface.py --url https://huggingface.co/Qwen/Qwen2-7B-Instruct/tree/main
```

输出：

```bash
wget -O .gitattributes -c https://huggingface.co/Qwen/Qwen2-7B-Instruct/resolve/main/.gitattributes?download=true && wget -O LICENSE -c https://huggingface.co/Qwen/Qwen2-7B-Instruct/resolve/main/LICENSE?download=true && wget -O README.md -c https://huggingface.co/Qwen/Qwen2-7B-Instruct/resolve/main/README.md?download=true && wget -O config.json -c https://huggingface.co/Qwen/Qwen2-7B-Instruct/resolve/main/config.json?download=true && wget -O generation_config.json -c https://huggingface.co/Qwen/Qwen2-7B-Instruct/resolve/main/generation_config.json?download=true && wget -O merges.txt -c https://huggingface.co/Qwen/Qwen2-7B-Instruct/resolve/main/merges.txt?download=true && wget -O model-00001-of-00004.safetensors -c https://huggingface.co/Qwen/Qwen2-7B-Instruct/resolve/main/model-00001-of-00004.safetensors?download=true && wget -O model-00002-of-00004.safetensors -c https://huggingface.co/Qwen/Qwen2-7B-Instruct/resolve/main/model-00002-of-00004.safetensors?download=true && wget -O model-00003-of-00004.safetensors -c https://huggingface.co/Qwen/Qwen2-7B-Instruct/resolve/main/model-00003-of-00004.safetensors?download=true && wget -O model-00004-of-00004.safetensors -c https://huggingface.co/Qwen/Qwen2-7B-Instruct/resolve/main/model-00004-of-00004.safetensors?download=true && wget -O model.safetensors.index.json -c https://huggingface.co/Qwen/Qwen2-7B-Instruct/resolve/main/model.safetensors.index.json?download=true && wget -O tokenizer.json -c https://huggingface.co/Qwen/Qwen2-7B-Instruct/resolve/main/tokenizer.json?download=true && wget -O tokenizer_config.json -c https://huggingface.co/Qwen/Qwen2-7B-Instruct/resolve/main/tokenizer_config.json?download=true && wget -O vocab.json -c https://huggingface.co/Qwen/Qwen2-7B-Instruct/resolve/main/vocab.json?download=true
```

运行上面的命令即可得到权重文件，把它们全部放在 qwen2 文件夹下，然后编写推理程序：

```python
from transformers import AutoModelForCausalLM, AutoTokenizer

model_name = "./qwen2"
device = "cuda" # the device to load the model onto

model = AutoModelForCausalLM.from_pretrained(
    model_name,
    torch_dtype="auto",
    device_map=device
)
tokenizer = AutoTokenizer.from_pretrained(model_name)


while True:
    prompt = input("问题: ")

    messages = [
        {"role": "system", "content": "You are a helpful assistant."},
        {"role": "user", "content": prompt}
    ]
    text = tokenizer.apply_chat_template(
        messages,
        tokenize=False,
        add_generation_prompt=True
    )
    model_inputs = tokenizer([text], return_tensors="pt").to(device)

    generated_ids = model.generate(
        **model_inputs,
        max_new_tokens=512
    )
    generated_ids = [
        output_ids[len(input_ids):] for input_ids, output_ids in zip(model_inputs.input_ids, generated_ids)
    ]

    response = tokenizer.batch_decode(generated_ids, skip_special_tokens=True)[0]
    print("回答：",response)
```

运行结果如下：

<div align=center>
<img src="https://picx.zhimg.com/80/v2-696e77bc1742c6aa5aabc6fbb21fb869_1440w.png" style="width: 70%;"/>
</div>

---

## 参考资料

- https://gist.github.com/jhabr/64de581b4fd19ba44fbf1ffff66bcaa1
- https://developer.nvidia.com/embedded/learn/get-started-jetson-agx-orin-devkit