最新在制作求生之路的一个声音类型的mod，需要使用python进行一些基础音频操作，我曾经做过声音方面的相关工作，但是忘得差不多了hhh，简单记录一下。

能实现基础功能的有三个库：

- `pydub`：用于音频处理的库，提供了丰富的功能。
- `soundfile`：用于音频IO的基础库，可以读取音频的元信息。
- `librosa`：音频信号处理算法库，包括重采样、FFT、基础滤波等操作。

现在有一个demo，用于将一个文件夹下的所有44.1Khz的wav转换成16K的并输出到当前的`sound`文件夹下。下面分别用pydub和librosa实现。

## pydub

首推`pydub`，简单好用，所有的音频都被包装成了`AudioSegment`对象，而且可以在ipynb中直接预览`AudioSegment`对象。基础用法如下：

```python
from pydub import AudioSegment

import os

SRC_FOLDER = r"D:\Download\steam\steam\steamapps\common\Left 4 Dead 2\left4dead2\sound\player\survivor\voice\producer"
TARGRT_FOLDER = 'sound'

for file in os.listdir(SRC_FOLDER):
    sound_path = os.path.join(SRC_FOLDER, file)
    target_path = os.path.join(TARGRT_FOLDER, file)

    audio: AudioSegment = AudioSegment.from_file(sound_path)
    audio = audio.set_frame_rate(16000)
    audio.export(target_path, format='wav')
```


## librosa

```python
from pydub import AudioSegment

import os

SRC_FOLDER = r"D:\Download\steam\steam\steamapps\common\Left 4 Dead 2\left4dead2\sound\player\survivor\voice\producer"
TARGRT_FOLDER = 'sound'

for file in os.listdir(SRC_FOLDER):
    sound_path = os.path.join(SRC_FOLDER, file)
    target_path = os.path.join(TARGRT_FOLDER, file)

    audio: AudioSegment = AudioSegment.from_file(sound_path)
    audio.set_frame_rate(16000)
    audio.export(target_path, format='wav')
```

这只是一个简单的模板，其他的操作完全可以从中修改。