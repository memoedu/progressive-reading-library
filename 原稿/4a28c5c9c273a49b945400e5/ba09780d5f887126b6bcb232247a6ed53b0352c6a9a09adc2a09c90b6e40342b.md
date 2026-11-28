最近在使用VITS制作L4D2的中文语音包，现在在制作npc的部分，部分npc的声音具有电话听筒效果，现在尝试使用python的pydub实现这个效果。

## 基本原理

做两次EQ实现，低通+高通。

## 代码

先读入音频文件：

```python
from pydub import AudioSegment
from pydub.effects import normalize
import pydub.scipy_effects as scipy_effects

npc_audio: AudioSegment = AudioSegment.from_file('./sound/comeupstairs01.wav')
npc_audio
```

效果如下：

<audio controls>
	<source src="https://kirigaya.cn/files/audio/blog/blog_phone_like_input.wav" type="audio/mpeg">
	Your browser does not support the audio tag.
</audio>

<br>

核心音频处理代码如下：

```python
def phone_like(wav: AudioSegment, low_center=1600, high_center=300) -> AudioSegment:
    audio = scipy_effects.low_pass_filter(wav, low_center)
    audio = scipy_effects.high_pass_filter(audio, high_center)
    audio = normalize(audio)
    audio.export('output_audio.wav')
    return audio

phone_like(npc_audio)
```

<audio controls>
	<source src="https://kirigaya.cn/files/audio/blog/blog_phone_like_output.wav" type="audio/mpeg">
	Your browser does not support the audio tag.
</audio>