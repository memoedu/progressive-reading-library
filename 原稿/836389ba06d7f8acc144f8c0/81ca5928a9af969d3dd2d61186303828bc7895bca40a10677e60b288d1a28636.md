## 前言

在树莓派上玩相机算是最常见的做法了，但是今天玩的时候却发现，网上的教程大部分都过时了，于是我又来更新一下知识了。



---

## 原料

为了完成本次教程，你需要：

- 树莓派
- 相机模块
- 图像处理的基础知识
- 计算机网络媒体流协议的知识
- 信号处理的基础知识

---

## 安装相机

一般我们有两种类型的相机可供购买，一种就是 USB 相机，大部分通过 USB Type-A 和系统连接，这种用起来比较简单；树莓派官方给的相机则是通过 CSI 排线连接的。

开始前，我们先把树莓派断电。

第一件要做的事情，就是先将 CSI 软排插入相机模块中，注意 CSI 软排接口的挡板很脆弱，不要扯坏了：

<div align=center>
<img src="https://pica.zhimg.com/80/v2-b916cc1f6c25f3687e454f2695ee8a90_1440w.jpeg" style="width: 60%;"/>
</div>


然后树莓派使用的相机的CSI软排线是一头窄，一头宽，很多商家会给你提供多种尺寸的CSI软排线，使用前先检查一下宽度。然后就是 CSI 软排裸露铜线的方向和设备CSI接口导电的方向保持一致。

比如在 B5 中，CSI 就是这么连接的：（这和 B3,B4 的方向相反，请一定先了解 CSI 软排的接口原理后，再去使用！）

<div align=center>
<img src="https://pic1.zhimg.com/80/v2-c53cf0328eca30f2a375a06173437f73_1440w.jpeg" style="width: 60%;"/>
</div>


安装后重新启动树莓派即可。

---

## 验证摄像头

上电后，我们 ssh 进树莓派，此处，我使用了 oh-my-zsh 强化了控制台，让我们打命令效率事半功倍，做法看我的笔记 [Linux终端美化（oh-my-zsh）](https://kirigaya.cn/blog/article?seq=66)

我们输入如下命令检查：

```bash
libcamera-hello
```

如果出现了如下的打印效果，说明摄像头安装成功。

<div align=center>
<img src="https://picx.zhimg.com/80/v2-052b1ede89afa09893e13b5ce499e46e_1440w.png" style="width: 50%;"/>
</div>v




根据版本不同，B5 默认已经开启了摄像头，如果无法连接，使用 `sudo raspi-config` 去 interface 这个选项去使能对应功能。

B4，B3这些老版本在 interface 选项中会多出来一个 camera，需要使能后才能使用。而 B5 中是没有这个选项的。如下图所示。

<div align=center>
<img src="https://pic1.zhimg.com/80/v2-047a4bb090dcfbeb7a9122fd6c91f192_1440w.png" style="width: 60%;"/>
</div>


---

## 树莓派 CSI 相机基础使用

下面，我们来简单使用以下树莓派的相机吧！

基础指令有如下几条：

| 命令            | 说明                                                         |
| --------------- | ------------------------------------------------------------ |
| libcamera-hello | 它启动相机，显示相机可以看到的内容的预览窗口，然后关闭。这个指令常用作相机的 hello world，用于检测为我们的相机是否正常工作 |
| libcamera-jpeg  | 将图像捕获到 jpeg 文件                                       |
| libcamera-still | 类似于 libcamera-jpeg，但是它支持传统 `raspistill` 命令中的更多功能 |
| libcamera-vid   | 将视频捕获到 H264 容器                                       |
| libcamera-raw   | 直接从相机录制原始帧，没有预览窗口                           |

我们先尝试获取一张图片吧！

运行如下命令：

```bash
mkdir -p ~/project/hello-camera
cd ~/project/hello-camera
libcamera-jpeg -t 100 -o test.jpeg
```

该命令意思为延迟 100 ms 后，捕获影像到 `test.jpeg` 中。

因为我们的树莓派没有屏幕，为了预览图片，我们可以使用 http 服务：

```bash
python -m http.server
```

out

```
Serving HTTP on 0.0.0.0 port 8000 (http://0.0.0.0:8000/) ...
```

这意味着我们的树莓派正在向局域网广播自己的 8000 端口服务，我们只需要在 PC 上打开浏览器，在地址栏输入 `<主机名>.local:8000` 即可：

<div align=center>
<img src="https://pic1.zhimg.com/80/v2-322429470a15537c46c282cbb0429368_1440w.png" style="width: 60%;"/>
</div>

>  如果读者没有web开发的经验，但是却对上面这个过程的原理好奇，可以移步我曾经的 [知乎回答](https://www.zhihu.com/question/658026837/answer/3519853866) 。

点击上图的 `test.jpeg` 即可阅览当前图像：

<div align=center>
<img src="https://picx.zhimg.com/80/v2-3659d45c3e4595b58ac85c4a27bc667b_1440w.jpeg" style="width: 50%;"/>
</div>

图像尺寸为 2592 × 1944，确实是 500 万像素。就是质量嘛。。。一言难尽了，下次买一个更好的吧。现在能亮就行。

---

## 让我们更进一步吧！在我们的PC机上实时预览树莓派的摄像头

只是显示一张图片未免太无聊了，我们继续来一些好玩的吧！

为了 python 可以驱动视频缓冲，我们需要安装 python 编译需要的 C 库 和 opencv 和 picamera2

> tips: 树莓派自带的 python 环境只能通过 `sudo apt install python3-XXX` 来安装库 `XXX`。如果你希望更加自由的安装方式，比如用 anaconda，那么请见文章末的附录，折腾开心！

```bash
sudo apt install libcamera-dev libcap-dev python3-libcamera python3-kms++ python3-picamera2 python3-opencv
```

验证一下：

```bash
python -c "import picamera2;import cv2;print(cv2.__version__)"
```

没有报错就算成功。

想要在我们的PC上实时预览树莓派，有多种方法可以做到，这里就要提到大家喜闻乐见的多媒体应用层协议了，不熟悉的同学可以重修计算机网络了。

<div align=center>
<img src="https://pic1.zhimg.com/80/v2-73343f6c8af32505288c2239ef0a49bf_1440w.jpeg" style="width: 50%;"/>
</div>

### 方法一：MJPG 流

所谓 MJPG 流，说人话就是通过 http 将每一个独立的 jpg 图像返回到浏览器中，从而形成可供预览的视频流。这种方法实现起来非常简单，就是效率太低啦。

当然，写代码之前需要一个很不错的编辑器，可以参考这篇文章：[在树莓派上编译和使用 neovim](https://kirigaya.cn/blog/article?seq=242) 。

下面的代码来自 https://picamera.readthedocs.io/en/release-1.13/recipes2.html，先进入我们的项目，而且创建 python 文件

```bash
cd ~/project/hello-camera
vim mjpg-video.py
```

写入：

```python
import io
import logging
import socketserver
from http import server
from threading import Condition

from picamera2 import Picamera2
from picamera2.encoders import JpegEncoder
from picamera2.outputs import FileOutput

PAGE = """\
<html>
<head>
<title>picamera2 MJPEG streaming demo</title>
</head>
<body>
<h1>Picamera2 MJPEG Streaming Demo</h1>
<img src="stream.mjpg" width="640" height="480" />
</body>
</html>
"""

class StreamingOutput(io.BufferedIOBase):
    def __init__(self):
        self.frame = None
        self.condition = Condition()

    def write(self, buf):
        with self.condition:
            self.frame = buf
            self.condition.notify_all()


class StreamingHandler(server.BaseHTTPRequestHandler):
    def do_GET(self):
        if self.path == '/':
            self.send_response(301)
            self.send_header('Location', '/index.html')
            self.end_headers()
        elif self.path == '/index.html':
            content = PAGE.encode('utf-8')
            self.send_response(200)
            self.send_header('Content-Type', 'text/html')
            self.send_header('Content-Length', len(content))
            self.end_headers()
            self.wfile.write(content)
        elif self.path == '/stream.mjpg':
            self.send_response(200)
            self.send_header('Age', 0)
            self.send_header('Cache-Control', 'no-cache, private')
            self.send_header('Pragma', 'no-cache')
            self.send_header('Content-Type', 'multipart/x-mixed-replace; boundary=FRAME')
            self.end_headers()
            try:
                while True:
                    with output.condition:
                        output.condition.wait()
                        frame = output.frame
                    self.wfile.write(b'--FRAME\r\n')
                    self.send_header('Content-Type', 'image/jpeg')
                    self.send_header('Content-Length', len(frame))
                    self.end_headers()
                    self.wfile.write(frame)
                    self.wfile.write(b'\r\n')
            except Exception as e:
                logging.warning(
                    'Removed streaming client %s: %s',
                    self.client_address, str(e))
        else:
            self.send_error(404)
            self.end_headers()


class StreamingServer(socketserver.ThreadingMixIn, server.HTTPServer):
    allow_reuse_address = True
    daemon_threads = True


picam2 = Picamera2()
picam2.configure(picam2.create_video_configuration(main={"size": (640, 480)}))
output = StreamingOutput()
picam2.start_recording(JpegEncoder(), FileOutput(output))

try:
    address = ('', 7123)
    server = StreamingServer(address, StreamingHandler)
    server.serve_forever()
finally:
    picam2.stop_recording()
```

然后，在浏览器中输入：

```
lagrange.local:7123
```

按下回车就可以看到实时视频了：

<div align=center>
<img src="https://picx.zhimg.com/80/v2-123c22a330f01e3e2d404f53223d0c0e_1440w.png" style="width: 50%;"/>
</div>

### 方法二：RTSP 流

可以使用 RTSP 协议将树莓派的摄像头进行推流，可以使用这个项目：https://github.com/aler9/rtsp-simple-server

因为在局域网内玩耍，我对于 RTSP 推流并没有啥需求，就懒得去折腾了。



### 方法三：h264 + tcp 直接推流

由于树莓派提供的 `libcamera-vid` 可以直接生成 h264 编码的媒体流，我们可以执行如下的语句将 h264 流用 tcp 协议推到 广播段的 8888 端口上：

```bash
libcamera-vid -t 0 --nopreview --codec h264 --profile high --listen -o tcp://0.0.0.0:8888
```

然后，我们可以在PC上通过 VLC 这个软件，软件是免费的，[下载链接](https://www.videolan.org/vlc/download-windows.zh_CN.html)，打开后，点击 “媒体” -> “打开网络串流”，输入

```
tcp/h264://<主机名>.local:8888
```

<div align=center>
<img src="https://pic1.zhimg.com/80/v2-eedfcb7326d242fb87628eea2e106234_1440w.png" style="width: 50%;"/>
</div>

有关这其中会遇到的各种问题，以及为啥不用 udp，在这个 issue 中已经被充分讨论过了： https://github.com/blakeblackshear/frigate/discussions/3097

如果你的设备型号不是最新的，可以在这篇教程里找到非常详细推流方法总结：https://www.raspberrypi.com/documentation/computers/camera_software.html#stream-video-over-a-network-with-rpicam-apps

---

## 玩具：基于树莓派+背景高斯建模的动态环境监控

前面铺垫了这么多，现在可以开始做玩具了，最近想做一个监控摄像头，但是嘛，常规的监控摄像头会一直做记录，大部分记录的视频帧其实是完全一样的。对于没有活物的视频我不感兴趣，我只想要自动记录有动态物体的背景，如果背景是静态的，就不需要记录。

如果你是一名合格的计算机视觉的学生，那么第一个想到的适用于嵌入式设备的动态检测算法应该就是高斯背景建模或者Kanda光流法了。此处为了偷懒，就简单使用高斯背景建模来构造动态物体的二值视频流吧！

高斯背景建模的原理也非常简单，程序初始化时，我们给图像的每一个像素点构造一个高斯分布，我们视频流输入是 640 * 480，也就是创建 30 万个高斯分布。当然，可以用视频流 preload 时的 10帧来初始化高斯分布的均值和方差。然后当视频流开始被采集时，对于当前帧的每一个像素点 p，计算它在对应的高斯分布 PDF 的值，值小于某个阈值，就认为偏离太多， p 就会被模型标记为一个前景点；如果值比较大，那么就认为当前点属于高斯分布，不仅把它标记为一个背景点，还会用 p 来更新当前点高斯分布的均值和方差。

具体更新均值和方差的方法有很多，比较简单的一种方法就是每一个高斯分布都维护一个长度为K的队列，每次需要更新的点就入队，然后拿队列的所有值计算均值和方差。

> 如果阁下看这段描述有点迷糊，可以出门重修 数据结构 + 概率论 + 数字图像处理 了

### 实现 mjpg 推流

好啦，原理很简单，是时候开始写代码了，为了把结果推流到内网中进行预览，我们先封装一个简单的推流程序，文件起名为 `mjpg.py`，代表使用 mjpg 作为推流协议，我比较懒，不高兴玩 rstp 了：

```python
import io
import logging
import socketserver
from http import server
from threading import Condition

from picamera2 import Picamera2

import cv2

PAGE = """\
<html>
<head>
<title>picamera2 MJPEG streaming</title>
</head>
<body>
<h1>Picamera2 MJPEG Streaming Demo</h1>
<img src="stream.mjpg" width="640" height="480" />
</body>
</html>
"""

class StreamingHandler(server.BaseHTTPRequestHandler):
    def do_GET(self):
        camera = streaming_config['camera']
        html = streaming_config['html']
        frame_handler = streaming_config['frame_handler']

        if self.path == '/':
            self.send_response(301)
            self.send_header('Location', '/index.html')
            self.end_headers()
        elif self.path == '/index.html':
            content = html.encode('utf-8')
            self.send_response(200)
            self.send_header('Content-Type', 'text/html')
            self.send_header('Content-Length', len(content))
            self.end_headers()
            self.wfile.write(content)
        elif self.path == '/stream.mjpg':
            self.send_response(200)
            self.send_header('Age', 0)
            self.send_header('Cache-Control', 'no-cache, private')
            self.send_header('Pragma', 'no-cache')
            self.send_header('Content-Type', 'multipart/x-mixed-replace; boundary=FRAME')
            self.end_headers()
            try:

                while True:
                    frame = camera.capture_array()
                    frame = frame_handler(frame)
                    frame = cv2.cvtColor(frame, cv2.COLOR_RGB2BGR)
                    _, frame = cv2.imencode('.jpg', frame)
                    frame_bytes = frame.tobytes()
                    self.wfile.write(b'--FRAME\r\n')
                    self.send_header('Content-Type', 'image/jpeg')
                    self.send_header('Content-Length', len(frame_bytes))
                    self.end_headers()
                    self.wfile.write(frame_bytes)
                    self.wfile.write(b'\r\n')
            except Exception as e:
                logging.warning(
                    'Removed streaming client %s: %s',
                    self.client_address, str(e))
        else:
            self.send_error(404)
            self.end_headers()


class StreamingServer(socketserver.ThreadingMixIn, server.HTTPServer):
    allow_reuse_address = True
    daemon_threads = True

def default_handler(frame):
    return frame

streaming_config = {
    'camera': None,
    'frame_handler': default_handler,
    'html': PAGE
}
```



### 实现高斯背景建模

实现算法的文件就叫 `main.py` 好了，代码如下：

```python
from collections import Counter

import cv2
import numpy as np
from picamera2 import Picamera2

from mjpg import streaming_config, StreamingServer, StreamingHandler

camera_width = 640
camera_height = 480

camera = Picamera2()
camera_config = camera.create_preview_configuration(
    main={
        'size': (camera_width, camera_height)
    }
)
camera.configure(camera_config)
camera.start()

kernel = cv2.getStructuringElement(cv2.MORPH_ELLIPSE, (5, 5))
bg_model = cv2.createBackgroundSubtractorMOG2()

margin = np.full((camera_height, 100, 4), 255, dtype=np.uint8)

def frame_handler(frame: np.ndarray) -> np.ndarray:
    bg_mask = bg_model.apply(frame)
    bg_mask = cv2.morphologyEx(bg_mask, cv2.MORPH_OPEN, kernel)
    _, bg_mask = cv2.threshold(bg_mask, 100, 255, cv2.THRESH_BINARY)
    bg_mask = np.dstack((bg_mask, bg_mask, bg_mask, bg_mask))
    render_frame = np.concatenate((frame, margin, bg_mask), axis=1)
    return render_frame

PAGE = f"""\
<html>
<head>
<title>PiCamera Modeling</title>
</head>
<body>
<h1>PiCamera Modeling</h1>
<img src="stream.mjpg" width="{camera_width * 2 + margin.shape[1]}" height="{camera_height}" />
</body>
</html>
"""

if __name__ == '__main__':
    streaming_config['camera'] = camera
    streaming_config['frame_handler'] = frame_handler
    streaming_config['html'] = PAGE

    try:
        address = ('', 7123)
        server = StreamingServer(address, StreamingHandler)
        print('通过 http://lagrange.local:7123/index.html 来访问推流')
        server.serve_forever()
    finally:
        print('结束推流')
        camera.stop_recording()
```

输入 `python main.py` 即可开始监控 + 计算 + 推流，记得把 54 行的说明改成你自己的主机名：

<div align=center>
<img src="https://pic1.zhimg.com/80/v2-4703c6899c68d81554fb90ba306eb838_1440w.png" style="width: 50%;"/>
</div>

效果：

<div align=center>
<img src="https://picx.zhimg.com/80/v2-5b280f0e4dd0dc2fd444c5b379eb6f20_1440w.gif" style="width: 60%;"/>
</div>

可以看到，效果还是很不错的。

### 记录背景动态度 diagram

接下来，我们需要指定一个策略，来让程序知道当前流需要被记录。为此，我们需要一个指标来衡量当前的“环境动态度”，此处我采用了背景建模的mask中，形态学开操作 + 二值化 后的二值图像中的前景区块所占比例 `ratio` 作为衡量指标，该值越大，说明背景中的运动趋势更加明显。但是很显然，这个过程中会存在大量噪音。所以我打算使用类似于施密特触发器的方法对收获到的 `time-ratio` diagram 离散信号进行整流。

为此，我们需要先获取 `time-ratio` 的数据，我们在刚刚的 `main.py` 的 `frame_handler` 函数上下加一些东西：

```python
import atexit

iteration_count = 0
ratios = []

@atexit.register
def when_exit():
    np.save('fg-ratio.npy', ratios)

pixel_size = camera_height * camera_width

def frame_handler(frame: np.ndarray) -> np.ndarray:
    global iteration_count
    bg_mask = bg_model.apply(frame)
    bg_mask = cv2.morphologyEx(bg_mask, cv2.MORPH_OPEN, kernel)
    _, bg_mask = cv2.threshold(bg_mask, 100, 255, cv2.THRESH_BINARY)

    iteration_count += 1
    # picamera fps 为 30，大概每 100 ms 记录一次吧
    if iteration_count % 3 == 0:
        counter = Counter(bg_mask.flatten())
        ratio = counter.get(255, 0) / pixel_size
        ratios.append(ratio)

    bg_mask = np.dstack((bg_mask, bg_mask, bg_mask, bg_mask))
    render_frame = np.concatenate((frame, margin, bg_mask), axis=1)
    return render_frame
```

然后，我们运行这个程序，在程序运行期间，我们对着相机做三次很明显的动作，每次动作与动作之间都保留几秒的静止。等到你觉得数据收集的差不多了，按下 ctrl c 中断程序，此时程序会自动导出 `fs-ratio.npy` 。我们新建一个 `plot.py` 来绘制这个图像：

```python
# sudo apt install python3-matplotlib
import matplotlib.pyplot as plt
import numpy as np

ratios : np.ndarray = np.load('./fg-ratio.npy')
plt.plot(ratios, '-o')
plt.grid(True)
plt.title(f'min {ratios.min()}, max{ratios.max()}')
plt.savefig('plot.png')
```

输出结果使用 http 广播到内网中，查看如下：

<div align=center>
<img src="https://pica.zhimg.com/80/v2-5974b3459a69c8654cfad554e6bd678a_1440w.png" style="width: 50%;"/>
</div>

我们一共做了三个动作，图中也大概存在三个模点。噪音不少呢，我们要如何去除噪音呢？作为一个普通学生，我第一个想到的方法就是一维卷积 + 施密特整流。



### 去除噪音：一维卷积 + 施密特整流

如果要做一维卷积，我们需要知道窗口的大小为多少。在我们上面的设定中，每个点相距 100ms 记录一次，如果阁下学过人体生理学的话，就应该知道普通人的反应时间和我们的 Na-K 通道的去极化-极化周期相关，普通人的反应时间大概是 500ms，做个两倍容错，差不多是 1100ms，所以我们把窗口大小设为 11。完成后，再使用施密特触发器进行整流，我们可以在绘制的代码中模拟这一切：

```python
import matplotlib.pyplot as plt
import numpy as np
from scipy.signal import convolve

ratios : np.ndarray = np.load('./fg-ratio.npy')

plt.figure(dpi=120, figsize=(20, 5))

plt.subplot(1, 3, 1)
plt.plot(ratios, '-o')
plt.grid(True)
plt.title(f'min {ratios.min()}, max {ratios.max()}')

plt.subplot(1, 3, 2)
# 每个点相距 100 ms，普通人的反应速度为 500ms，做两倍容错，为 1100ms
kernel = np.hanning(11)
conv_result = convolve(ratios, kernel, mode='same')
plt.plot(conv_result, '-o')
plt.grid(True)
plt.title(f'After 1D Conv Filter, max {conv_result.max()}')

def schmidt_rectification(data, low_threshold, high_threshold):
    output = np.zeros_like(data)
    state = 0
    for i in range(len(data)):
        if state == 0:
            if data[i] > high_threshold:
                state = 1
                output[i] = 1
            else:
                output[i] = 0
        elif state == 1:
            if data[i] < low_threshold:
                state = 0
                output[i] = 0
            else:
                output[i] = 1
    return output

plt.subplot(1, 3, 3)
sch_result = schmidt_rectification(conv_result, 0.2, 0.45)
plt.plot(sch_result)
plt.grid(True)
plt.title(f'After Schmidt Filter')

plt.savefig('plot.png')
```

绘制结果：

<div align=center>
<img src="https://picx.zhimg.com/80/v2-e01643b0242840fa9f9ecbd59073f97d_1440w.png" style="width: 80%;"/>
</div>

我们一共挥手挥了三次，差不多也是三次的响应，说明差不多了。

### 父子进程结构，完成动态物体自动监控

接下来要做的，就是把上述过程写进一个生产者消费者模型中，此时我才用的是 python 自带的进程队列，A 进程负责读取相机视频流，并通过进程队列将数据交给 B 进程，B 进程则根据上述步骤进行滤波和整流，并将其中合格的视频片段保存到本地。代码如下：

```python
from collections import Counter
import atexit
import os
from multiprocessing import Process, Queue

import cv2
import numpy as np
from datetime import datetime
from scipy.signal import convolve

@atexit.register
def when_exit():
    worker.close()

def schmidt_rectification(data, low_threshold, high_threshold):
    output = np.zeros_like(data)
    state = 0
    slices = []
    start, end = -1, -1
    for i in range(len(data)):
        if state == 0:
            if data[i] > high_threshold:
                state = 1
                output[i] = 1
                start = i
            else:
                output[i] = 0
        elif state == 1:
            if data[i] < low_threshold:
                state = 0
                output[i] = 0
                end = i
                slices.append((start, end))
            else:
                output[i] = 1
    return output, slices

def dump_video(frames: list[np.ndarray], filename: str):
    frame = frames[0]
    frame_height = frame.shape[0]
    frame_width = frame.shape[1]
    fourcc = cv2.VideoWriter_fourcc(*'MP4V')
    writer = cv2.VideoWriter(filename, fourcc, 10.0, (frame_width, frame_height), isColor=True)

    for frame in frames:
        frame = cv2.cvtColor(frame, cv2.COLOR_RGB2BGR)
        writer.write(frame)

    writer.release()

def cache_handler(q: Queue, chunk_size: int, save_root: str):
    kernel = cv2.getStructuringElement(cv2.MORPH_ELLIPSE, (5, 5))
    bg_model = cv2.createBackgroundSubtractorMOG2()
    hanning = np.hanning(11)

    ratios = []

    frames = []
    times = []
    iterations = 0
    gap_ratio = 3

    while True:
        frame: np.ndarray = q.get()
        now = datetime.now()
        camera_size = frame.shape[0] * frame.shape[1]

        frames.append(frame)
        times.append(now.strftime('%Y_%m_%d_%H_%M_%S'))

        if iterations % gap_ratio == 0:
            bg_mask = bg_model.apply(frame)
            bg_mask = cv2.morphologyEx(bg_mask, cv2.MORPH_OPEN, kernel)
            _, bg_mask = cv2.threshold(bg_mask, 100, 255, cv2.THRESH_BINARY)
            ratio = Counter(bg_mask.flatten()).get(255, 0) / camera_size
            ratios.append(ratio)
            if iterations > 99999999:
                iterations = 0

        iterations += 1

        if len(ratios) >= chunk_size:
            conv_signal = convolve(ratios, hanning, mode='same')
            sch_signal, slices = schmidt_rectification(conv_signal, 0.2, 0.45)
            for start, end in slices:
                real_start = start * gap_ratio
                real_end = end * gap_ratio
                dump_frames = frames[real_start: real_end + 1]
                filename = f'{times[real_start]}-{times[real_end]}.mp4'
                save_path = os.path.join(save_root, filename)
                dump_video(dump_frames, save_path)
                print(f'保存视频到 {save_path}')
            frames.clear()
            times.clear()
            ratios.clear()

if __name__ == '__main__':
    from picamera2 import Picamera2

    save_root = 'videos'
    os.makedirs(save_root, exist_ok=True)

    camera_width = 640
    camera_height = 480

    camera = Picamera2()
    camera.video_configuration.controls.FrameRate = 10.0
    camera_config = camera.create_preview_configuration(
        main={
            'size': (camera_width, camera_height)
        }
    )
    camera.configure(camera_config)
    camera.start()

    chunk_size = 300
    queue = Queue(maxsize=chunk_size)

    worker = Process(target=cache_handler, args=(queue, chunk_size, save_root))
    worker.start()

    while True:
        frame = camera.capture_array()
        queue.put(frame)
```

如果你自己玩，一定要注意相机的 fps 和 导出视频的 fps 保持一致。

出去散步了半小时，看看最终结果：

<div align=center>
<img src="https://pic1.zhimg.com/80/v2-1dceebcd4d9c87b05fab5d1853eedddb_1440w.png" style="width: 45%;"/>
</div>

http server 中出现了自动保存的视频，点击即可观看。

期间，树莓派的系统负载如下：

<div align=center>
<img src="https://picx.zhimg.com/80/v2-7e28d4bc6b16330b469da069ed0e914d_1440w.png?source=d16d100b" style="width: 45%;"/>
</div>

改进方向：可以尝试将间隔比较小的视频进行合并。

----

## 附录：在树莓派如何折腾 anaconda

如果你使用的是和我一样的anaconda，那么使用 picamera2 就有一点点麻烦了。因为 picamera2 调用的 libcamera 驱动并不包含在这个 python 库的安装中，原作者可能有点偷懒了吧。当我们运行 `sudo apt install python3-libcamera` 后， apt 实际上会在 `/usr/lib/python3/dist-packages` 下安装 libcamera 的驱动胶水，咱们把这部分代码移花接木到我们自己的 python 解释器的 `site-package` 下：

```bash
cd ~/anaconda3/lib/python3.11/site-packages  # 根据你的 anaconda 的安装路径来决定
sudo cp -r /usr/lib/python3/dist-packages/libcamera libcamera
```

成功后，我们验证一下相机驱动是否可以正常使用：

```bash
python -c "import picamera2"
```

如果没有报错，说明 ok 了，我们可以愉快地使用 python 开发树莓派相机应用了。

如果报了 GLIBC 相关的错，比如：

```
ImportError: /home/kirigaya/anaconda3/bin/../lib/libstdc++.so.6: version `GLIBCXX_3.4.30' not found (required by /lib/aarch64-linux-gnu/libcamera.so.0.2)
```

我一开始以为树莓派自己的 C++ 的 glib 库版本过低，需要升级一下。后来发现，原来是 anaconda 自己自带的 C++ 的 glibc 动态链接覆盖了树莓派自带的，anaconda 自带的这个版本比较低。我们可以简单看一下：

```bash
cd ~/anaconda3/lib
ll libstdc++.so.6
strings libstdc++.so.6 | grep GLIBCXX_3.4.30
```

什么输出都没有，说明 anaconda 自己的 c++ glib 版本不支持到 30。

解决方法很简单，我们只需要把 树莓派 自带的 dll 覆盖 anaconda 的就行了，注意，这一步对于 linux 新手而言比较危险，请在你的 linux 大佬朋友的协助下完成。

```bash
cd ~/anaconda3/lib
sudo rm libstdc++.so.6
sudo ln -s /usr/lib/aarch64-linux-gnu/libstdc++.so.6 libstdc++.so.6
```











