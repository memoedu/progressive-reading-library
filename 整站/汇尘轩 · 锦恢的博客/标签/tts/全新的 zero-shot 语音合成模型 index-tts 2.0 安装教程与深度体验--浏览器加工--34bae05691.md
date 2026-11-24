## 前言

虽然锦恢经常写一些AI技术相关的文章，但是我其实有一个只有少部分人知道的身份，那就是我其实也是一个 steam 求生之路社区的 MOD 作者，下面是我的 steam 主页：

https://steamcommunity.com/profiles/76561199183298707/

我在制作游戏语音包时一直有一个痛点，那就是客户每次让我制作一个新的游戏/动漫角色的语音包，我就得先去获取这个角色的语音样本，然后重新训练对应的权重部分。

而 index-tts 2.0 的推出帮我解决了这个问题，这是一个新的语音合成模型，输入角色 A 的几秒的语音样本和需要合成的文本，就能直接生成角色 A 音色的合成效果。

项目是完全开源的（代码，权重都开源，甚至 huggingface 和 modelscope 一式两份），下面是开源地址。

https://github.com/index-tts/index-tts

这篇文章，我会讲解如何安装和使用 index-tts，来用任意你想要的角色的音色来合成语音。

![](https://picx.zhimg.com/80/v2-6708623a107fa739560a0825dab5a57a_1440w.png)



## 安装和部署

先保证你的环境为：

- python 3.10 +
- 显卡为 Nvidia RTX 20 架构以及更新的架构，显存至少 12 g

### STEP 1. 安装基本的脚手架

我们需要先安装一些基本的脚手架，后面会用到这些工具

```bash
pip install uv modelscope
```

### STEP 2. 克隆 index-tts 并完成初始化

```bash
git clone https://github.com/index-tts/index-tts
cd index-tts
uv sync
```

> 习惯使用 pip 的朋友也可以通过 `pip install -r requirements.txt` 安装依赖。

### STEP 3. 下载模型权重

既然在国内，我们就从 modelscope 上下载模型吧！

```bash
modelscope download --model IndexTeam/IndexTTS-2 --local_dir checkpoints
```


> 习惯用 huggingface 的朋友也可以通过 `huggingface-cli login` 登录，然后通过下面的命令下载模型：

    ```bash
    huggingface-cli download IndexTeam/IndexTTS-2 \
    bpe.model config.yaml feat1.pt feat2.pt gpt.pth qwen0.6bemo4-merge s2mel.pth wav2vec2bert_stats.pt
    --local-dir checkpoints
    ```

### STEP 4. 运行 webui

运行如下的语句就可以打开一个 web 服务：

```bash
uv run webui.py
```

> 遇到问题请看 https://github.com/index-tts/index-tts/issues/283

![](https://picx.zhimg.com/80/v2-aafa66a7e94b63ef8f4568310ca46530_1440w.png)


## 高级使用

如果你希望把 index-tts 集成到你的项目中，可以如下安装它：

```bash
git clone https://github.com/index-tts/index-tts.git
cd index-tts && pip install -e .
```

index-tts 提供了两种模式进行语音合成。

### 语音合成 - 普通模式

普通模式下，你只需要提供需要合成的 speaker 音色和合成的文本即可，最简代码如下：

```py
from indextts.infer_v2 import IndexTTS2
tts = IndexTTS2(cfg_path="checkpoints/config.yaml", model_dir="checkpoints", is_fp16=False, use_cuda_kernel=False)
text = """
我的本科机器学习老师在讲解西瓜书第三章的时候讲过一个另我醍醐灌顶的论述：机器学习的训练一定是不可能 100% 收敛到最优解的，因为假设它一定能收敛到最优解，那么利用机器学习模型就能将 NP 难引到 P，引发悖论。
在工程实践中，解决NP难的主要方法都是通过引入额外的假设来在特定范围内解决，比如大模型训练框架的流水线并行 sharding 算法，就是假设 OP 都是 transformer，否则任意 OP 下的模型 sharding 以达到指定调度机的负载均衡，是一个NP难问题。
而机器学习的假设都在任务定义和数据集本身上，地球上所有物体分类器做不了，但是猫狗分类器能做，假设被埋入了问题定义和数据集本身的 domain 上。
本质上都是一样的。
"""
tts.infer(spk_audio_prompt='examples/plosis.mp3', text=text, output_path="plosis.gen.mp3", verbose=True)
```

这里我才用了明日方舟的角色白面鸮的解包语音作为输入（`examples/plosis.mp3`）

`examples/plosis.mp3` 如下：

https://kirigaya.cn/files/audio/blog/plosis.mp3

合成后的 `plosis.gen.mp3` 效果如下：

https://kirigaya.cn/files/audio/blog/plosis.gen.mp3


### 语音合成 - 情绪引导模式

如果需要合成带有激烈情绪的样本，可以通过 `emo_audio_prompt` 来导入含有情绪的音频文件，让生成结果的情绪更加逼真。

比如你要合成一段情绪低沉的音频，除了 speaker 的语音和文本外，还可以输入一段语调悲凉的声音作为 emo audio prompt，代码如下：

```py
from indextts.infer_v2 import IndexTTS2
tts = IndexTTS2(cfg_path="checkpoints/config.yaml", model_dir="checkpoints", is_fp16=False, use_cuda_kernel=False)
text = """
白头若是雪可替，世上何来苦心人。
等不来的缘，若是强求，也就不自洽了。
这么简单的道理，世上又有多少人，年入半百，方才醒悟。
"""
tts.infer(spk_audio_prompt='examples/shu.mp3', text=text, output_path="shu.gen.mp3", emo_audio_prompt='examples/emo_sad.wav', verbose=True)
```

这里我们用明日方舟干员「黍」的声音来做演示，`examples/emo_sad.wav` 是 index-tts 项目中自带的一段悲伤的语音。

`examples/shu.mp3`

https://kirigaya.cn/files/audio/blog/shu.mp3

合成音频 `shu.gen.mp3` 如下：

https://kirigaya.cn/files/audio/blog/shu.gen.mp3


> 实际测试下来算是发现了一个 index-tts 目前的不足，如果。emo_audio_prompt 的音色和 spk_audio_prompt 的音色相去甚远，那么合成的音频的音色收到的扰动就会很大。


## FAQ

### 运行时遇到一堆奇怪的问题

看我总结的 https://github.com/index-tts/index-tts/issues/283

### 运行速度有点慢

一方面你可以配置 nvcc 编译器来编译他们提供的 cuda kernel。这里你需要解决一些编译时的小问题。

不过 index-tts 默认是使用全部卡进行推理的，而 index-tts 本身并不大，分布式推理只会降低性能，所以我建议你在使用 index-tts 进行推理前设置一下可见显卡为 1:

```bash
export CUDA_VISIBLE_DEVICES=0 # 选择 0 号卡为可见卡
```