这篇博客将就我发布的中文AI配音插件，讲讲如何实现。如果你希望做出对应的优化和自己喜欢的角色的AI配音，或许可以看看。但是更多地，这是写给我自己看的。效果预览如下：

- [基于VITS的原神人物音色的求生之路2 自动配音](https://www.bilibili.com/video/BV1nX4y187Xt/)
- [使用原神人物给求生之路2的NPC做中文配音](https://www.bilibili.com/video/BV1XW4y1X7it/%23reply954574630)

如果你希望游玩我的MOD，欢迎前往steam创意工坊进行订阅，并且follow我的steam账号： [Kirigaya » 创意工坊物品 » Left 4 Dead 2](https://steamcommunity.com/profiles/76561199183298707/myworkshopfiles/%3Fappid%3D550)

该项目目前已经开源： [LSTM-Kirigaya
/
VITS-Voice-Replace-L4D2](https://github.com/LSTM-Kirigaya/VITS-Voice-Replace-L4D2)

---

## 参考

[1] 如何在L4D2中制作声音类型的mod并上传：https://www.youtube.com/watch?v=WdfpcJ13fjI

[2] 如何获取L4D2中角色的台词文本：https://steamcommunity.com/sharedfiles/filedetails/?id=2477138247

[3] L4D2如何上传任意尺寸的图片作为封面图：https://steamcommunity.com/shar

---

## 基本思路

根据台词，合成对应的音频，并制作MOD，强烈推荐先阅读参考资料[1]来知晓如下的事实：求生之路mod制作本质就是替换静态资源。

所以你需要先知道角色语音的静态资源（wav）在哪里。答案：`D:\Download\steam\steam\steamapps\common\Left 4 Dead 2\left4dead2\sound\player\survivor\voice`

这个路径下有四个文件夹：

- coach：角色Coach的所有语音
- gambler：角色Nick的所有语音
- mechanic：角色Ellis的所有语音
- producer：角色Rolleche的所有语音
以制作Rolleche的声音MOD为例，只需要做出和producer文件夹下名字相同的wav文件即可。

根据我的尝试，我发现替换的你自己制作的wav文件必须要满足如下的性质：

- 文件名相同
- 采样率相同（producer下的wav文件的采样率并不都是44.1Khz，这点很坑）
- 时间不能比原本的wav文件短（短的话会用刺耳的噪音替代，长的话对于部分会被截断）
避免这些问题的后处理代码我已经写好了。

---

## Step.1 获取人物台词

>  请不要重新运行下方代码，台词文件我已经开源到了github，任君自取：[https://link.zhihu.com/?target=https%3A//github.com/LSTM-Kirigaya/VITS-Voice-Replace-L4D2/tree/main/transcription](https://link.zhihu.com/?target=https%3A//github.com/LSTM-Kirigaya/VITS-Voice-Replace-L4D2/tree/main/transcription)


### 1.1 解包台词文件

首先需要获取人物台词，根据[2]中提供的资料，找到pak01_dir.vpk，并用GCFScape解包，然后找到./resource/subtitles_schinese.txt（其他语音同理）。

<div align=center>
<img src="https://pic3.zhimg.com/v2-844046776fef1d2520524022e8849932_r.jpg" style="width: 80%;"/>
</div>


把这个文件提取出来，很容易发现台词文件中的文本规律，写段代码就可以提取角色的wav文件和台词的对应关系。

### 1.2 缺失台词使用语音识别+翻译模型

但是问题是，这不全，producer文件夹下有的波形文件名字不存在于台词文件中。所以我找出了这些不存在对应台词的producer下的波形文件，然后使用huggingface上的语音识别+英中翻译深度学习模型进行了处理。

语音识别的代码如下，它会将sound文件夹下的wav进行识别，并导出json文件：

```python
from transformers import WhisperProcessor, WhisperForConditionalGeneration
import os
import soundfile as sf
import json
import tqdm

processor = WhisperProcessor.from_pretrained("openai/whisper-medium")
model = WhisperForConditionalGeneration.from_pretrained("openai/whisper-medium")
model.config.forced_decoder_ids = None
model = model.cuda()

l4d2_transcriptions = {}

for file in tqdm.tqdm(os.listdir('sound')):
    sound_path = os.path.join('sound', file)
    src_signal, sample_rate = sf.read(sound_path)

    input_features = processor(src_signal, sampling_rate=sample_rate, return_tensors="pt").input_features.cuda()
    predicted_ids = model.generate(input_features)

    transcription = processor.batch_decode(predicted_ids, skip_special_tokens=True)
    l4d2_transcriptions[file] = transcription

with open('transcription.json', 'w', encoding='utf-8') as fp:
    json.dump(l4d2_transcriptions, fp=fp, ensure_ascii=False, indent=4)
```

英中翻译的代码如下：

```python
import json
import requests as r
import tqdm
from transformers import AutoTokenizer, AutoModelForSeq2SeqLM

with open('transcription.json', 'r', encoding='utf-8') as fp:
    data = json.load(fp)

tokenizer = AutoTokenizer.from_pretrained("Helsinki-NLP/opus-mt-en-zh")
model = AutoModelForSeq2SeqLM.from_pretrained("Helsinki-NLP/opus-mt-en-zh")
model = model.cuda()

def translate(words):
    tokens = tokenizer(words, return_tensors='pt')
    for k in tokens:
        tokens[k] = tokens[k].cuda()
    translated = model.generate(**tokens, max_new_tokens=600)
    zh = tokenizer.decode(translated[0], skip_special_tokens=True)
    return zh

zh_data = {}

for wave_file in tqdm.tqdm(data):
    en_words = data[wave_file]
    try:
        zh_words = translate(en_words)
        zh_data[wave_file] = zh_words
    except:
        print('when translate {} error happen'.format(wave_file))
        zh_data[wave_file] = en_words

with open('transcription_zh.json', 'w', encoding='utf-8') as fp:
    json.dump(zh_data, fp, ensure_ascii=False, indent=4)
```

由于VITS中文的词表没有英文单词（只有字母的），因此，需要将所有发音的英文单词写成中文，这些单词全都是人名和地名，写段程序就能解决，最终得到了producer.json，大概长这个样子：

<div align=center>
<img src="https://pic2.zhimg.com/v2-23fdcc3504a60c34c9c7899360045d45_r.jpg" style="width: 40%;"/>
</div>

### 1.3 本地化翻译

然后就是一些贴合中文的翻译。我使用了ChatGPT+基于文本单词的替换+人工的方法，大概花了2个小时，完成了对9673条文本完成了。

比如把“西装”替换成“套装”；“甜心”、“宝贝”替换成“朋友”等等。

### 1.4 语音元信息

为了方便后续进行生成语音的采样率与长度调整，最好事先准备好语音的元信息，也就是记录了所有的语音的长度（毫秒数）和采样率（L4D2很坑爹，角色语音的采样率居然不一致！）。这部分代码很简单，我写在了notebook/transription.ipynb中。producer.meta.json的预览如下：


<div align=center>
<img src="https://pic4.zhimg.com/v2-79ce92d5a1bf49a3176bb457ed441b6f_r.jpg" style="width: 40%;"/>
<p class="img-caption">sr代表采样率，length代表语音持续的毫秒数</p>
</div>

---

## Step.2 部署VITS语音合成模型

这一部分我在之前的博客中写过了：[多角色语音合成模型vits-uma-genshin-honkai部署教程](https://zhuanlan.zhihu.com/p/634853001)

不再赘述。

有了之前处理过的中文台词文件和现在的中文语音合成模型，就可以合成最基础的语音了。

为了防止生成语音太长，被截断。在第一轮生成后，我会根据生成长度和原语音长度的比例，同比缩放`noise_scale_w`和`length_scale`这两个参数。并重新生成那些长度超出元信息中的生成语音。

代码可见`./make_raw_voice.py`。

---

## Step.3 语音增强+后处理


但是生成的语音有如下的几个问题：

- 声音有点小，在游戏里听不到
- 采样率和原本可能不一致
- 长度可能太长，导致被截断

为了解决这几个问题，有必要在Step2的生成结果后加一个后处理+语音增强模块。

### 语音增强

你可以做几个实验，代码详见`./notebook/sound.ipynb`。里面记录了几个简单的增强方案和最终的效果。

节省时间，我最终使用了增益+EQ+标准化的方法实现语音增强，在不破音的情况下，让生成的角色语音更加响亮。

### 后处理

根据之前得到的语音元信息，对音频进行后处理。先调整采样率，然后对齐长度。长度不足的，填充空信号，让长度一致。长度太长的（超出原音频的20%的），输出到一个以`.exceed.json`结尾的日志中，后续交给LLM或者人工进行文本浓缩。重复Step2。

不断迭代，我大概迭代了三轮，得到一个比较让人满意的结果。

---

最后不要忘了加入vpk进行打包。