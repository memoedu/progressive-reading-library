## 前言

日志系统是一项会将程序运行过程中各种状态信息以一种非常高效的方式 IO 到磁盘上，以此进行系统运维、实验结果持久化等任务的有力工具。如果你从事如下工作，可能会遇到使用日志的情况：

- 系统运维（做运维还不会日志？你改行吧。）
- 深度学习模型训练（控制台 tqdm 在打着进度条呢，日志可以让你随时查看当前的训练状态，甚至可以渲染下降曲线。什么？你急着说你会 tensorboard，visdom 和wandb？那你还看我的文章干嘛？钓鱼好玩是吧？）

<div align=center>
<img src="https://picx.zhimg.com/80/v2-39fea3771721e6f7f40a747400042f15_1440w.png" style="width: 30%;"/>
</div>

- 分布式系统开发与维护（std io 都给你扬喽，看你怎么断点或者 print）
- 非常耗时的科学计算程序（组胺模拟代码写错了导致有一部分参数结果无效？无所谓，有效的部分都保存在日志系统里面了，你随便洗）

Python 自带的日志模块 logging 并不好用， 强烈推荐大家用了都说好的 `loguru` 工具，我已经用了三年了，非常好用~ 。

<div align=center>
<img src="https://picx.zhimg.com/80/v2-48e015a75177cd2b2880919616c4d8db_1440w.png" style="width: 30%;"/>
</div>

---

## 安装

安装一行话：

```bash
pip install loguru
```

---


## 基础配置

使用 `loguru` 之前，我们一般需要简单配置，配置完成后， `loguru` 会非常高效且安全地完成一个日志系统该做的事情：

```python
from loguru import logger

logger.add(
    sink='./logs/train.log',  
    level='INFO',
    rotation='00:00',      
    retention='7 days',     
    compression='zip',        
    encoding='utf-8',  
    enqueue=True,
    format="{time:YYYY-MM-DD HH:mm:ss} | {level} | {message}"
)
```

解释一下上面的各项参数：

- `sink`: 创建日志文件的路径。
- `level`: 记录日志的等级，低于这个等级的日志不会被记录。等级顺序为 debug < info < warning < error。设置 INFO 会让 `logger.debug` 的输出信息不被写入磁盘。
- `rotation`: 轮换策略，此处代表每天凌晨创建新的日志文件进行日志 IO；也可以通过设置 "2 MB" 来指定 日志文件达到 2 MB 时进行轮换。
- `retention`: 只保留 7 天。
- `compression`: 日志文件较大时会采用 zip 进行压缩。
- `encoding`: 编码方式
- `enqueue`: 队列 IO 模式，此模式下日志 IO 不会影响 python 主进程，建议开启。
- `format`: 定义日志字符串的样式，这个应该都能看懂。

## loguru 只输出磁盘，不输出控制台

loguru 后续默认会将信息 std io 到控制台上，在深度学习训练中，有时我们会使用 `tqdm` 这样的进度条，所以不希望有东西被打印到控制台上。为了达到这个效果，我们可以在 `logger.add` 的前面写上：

```python
logger.remove(handler_id=None)
```

这样 loguru 后续的 IO 只会出现在磁盘上，而不会出现在控制台上。

---

## 基础使用

在配置完成后，使用起来非常简单：

```python
logger.info(f'training loss={0.01}')
logger.debug('out.shape')  # level 低于 INFO 所以不会出现在磁盘里
logger.warning(f'detect A divided by zero')
logger.error(f'shape of two images are not same! skip')
```

查看文件 `./logs/train.log`：

<div align=center>
<img src="https://picx.zhimg.com/80/v2-2a79786609e0cb1f9fd65079e73dc33c_1440w.png" style="width: 70%;"/>
</div>

通过今天的博客，希望诸位以后避免大张旗鼓地在自己的代码里写一堆的 print，每次复现别人的代码，我看着就觉得手里的红茶香味减半。

<div align=center>
<img src="https://pic1.zhimg.com/80/v2-c55b37734458c145a3d33c1371ad26e3_1440w.png" style="width: 30%;"/>
</div>

