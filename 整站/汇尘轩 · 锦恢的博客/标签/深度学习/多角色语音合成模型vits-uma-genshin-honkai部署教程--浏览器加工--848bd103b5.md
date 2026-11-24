一个好用的TTS是每个想做玩具的人都不可缺少的，当然，技术不能行恶，这一点还是很重要的。网上关于直接部署一个可用的多角色TTS模型的教程比较少，我简单写一下最近找到的一个很好的项目：`vits-uma-genshin-honkai`。

这个项目提供了hf的线上推理版本：https://huggingface.co/spaces/zomehwh/vits-uma-genshin-honkai

## 安装

这个项目存在于huggingface上，你当然无法在GitHub上找到，先下载并安装依赖库：

```bash
git clone https://huggingface.co/spaces/zomehwh/vits-uma-genshin-honkai
cd vits-uma-genshin-honkai
pip install -r requirements.txt
```

其中的`gradio`的版本存在问题，如果你在后续运行中出现错误，可以安装老版本的`gradio`：

```bash
pip install gradio==3.17.0
```
---

## 基本使用

这个项目用了gradio，那么很明显，我们会去寻找`app.py`，果然有，二话不多说，直接run：

```bash
python app.py --device cpu
# 如果有cuda，那么就写 --device cuda
```

默认会在http://127.0.0.1:7860/ 开启服务，在浏览器中打开这个链接就可以看到了：

<div align=center>
<img src="https://pic1.zhimg.com/80/v2-988f47e49713e7e46b42b05ac37fab4c_1440w.png" style="width: 80%;"/>
<p class="img-caption">vits-uma-genshin-honkai界面</p>
</div>

基础使用很简单，通过speaker选择角色，点击Submit生成对应的语音。

你可以通过下面三个参数控制生成结果，当然，同一组参数，每次生成的结果也会有些许不同。

---

## 程序推理

如果你希望通过python程序进行推理，从而将功能集成到你的项目中，可以修改我在下方给出的代码进行整合：

```python
import os
import gradio as gr
import utils
import argparse
import commons

from models import SynthesizerTrn
from text import text_to_sequence
import torch
from torch import no_grad, LongTensor
import soundfile as sf
import json
import tqdm

audio_postprocess_ori = gr.Audio.postprocess
limitation = os.getenv("SYSTEM") == "spaces"  # limit text and audio length in huggingface spaces

parser = argparse.ArgumentParser()
parser.add_argument('--device', type=str, default='cpu')
args = parser.parse_args()

device = torch.device(args.device)

hps_ms = utils.get_hparams_from_file(r'./model/config.json')
net_g_ms = SynthesizerTrn(
    len(hps_ms.symbols),
    hps_ms.data.filter_length // 2 + 1,
    hps_ms.train.segment_size // hps_ms.data.hop_length,
    n_speakers=hps_ms.data.n_speakers,
    **hps_ms.model)
_ = net_g_ms.eval().to(device)
speakers = hps_ms.speakers
model, optimizer, learning_rate, epochs = utils.load_checkpoint(r'./model/G_953000.pth', net_g_ms, None)

def get_text(text, hps):
    text_norm, clean_text = text_to_sequence(text, hps.symbols, hps.data.text_cleaners)
    if hps.data.add_blank:
        text_norm = commons.intersperse(text_norm, 0)
    text_norm = LongTensor(text_norm)
    return text_norm, clean_text

def vits(text, language, speaker_id, noise_scale, noise_scale_w, length_scale):
    if not len(text):
        return "输入文本不能为空！", None, None
    text = text.replace('\n', ' ').replace('\r', '').replace(" ", "")
    if len(text) > 100 and limitation:
        return f"输入文字过长！{len(text)}>100", None, None
    if language == 0:
        text = f"[ZH]{text}[ZH]"
    elif language == 1:
        text = f"[JA]{text}[JA]"
    else:
        text = f"{text}"
    stn_tst, clean_text = get_text(text, hps_ms)
    with no_grad():
        x_tst = stn_tst.unsqueeze(0).to(device)
        x_tst_lengths = LongTensor([stn_tst.size(0)]).to(device)
        speaker_id = LongTensor([speaker_id]).to(device)
        # tensor([132]) 0.1 0.668 1.1
        audio = net_g_ms.infer(x_tst, x_tst_lengths, sid=speaker_id, noise_scale=noise_scale, noise_scale_w=noise_scale_w,
                               length_scale=length_scale)[0][0, 0].data.cpu().float().numpy()

    return 22050, audio



if __name__ == '__main__':
    sr, audio = vits('你好呀', 0, torch.tensor([132]), 0.1, 0.668, 1.2)
    sf.write('output_file.wav', audio, samplerate=sr)
```

---

好了，玩得开心！
