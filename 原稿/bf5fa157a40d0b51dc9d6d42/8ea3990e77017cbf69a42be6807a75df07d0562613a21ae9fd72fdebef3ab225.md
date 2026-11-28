<div align='center'>
<iframe
height=86 width=330
src="//music.163.com/outchain/player?type=2&id=4919962&auto=1&height=66"
></iframe>
<p class="img-caption">Promise</p>
</div>


最近在朋友的安利下，正式注册了一个openai账号（之前嫌麻烦一直用的镜像），不过我最感兴趣的倒不是这玩意儿性能多好，我比较关心openai有没有提供可供国内直接调用的API，去官网上一看，还真有，虽然截止2023.2.14，openai还没有公开chatgpt的API（未来的读者在看的时候应该已经有了，有了记得戳我一下）。但是openai开放了功能丰富的其他功能，其中completion的功能就和chatgpt类似，可以用于生成对应的句子。对于聊天没有需求的朋友功能应该够用了。

我先展示一下最终的效果：

[查看知乎文章内嵌视频](https://zhuanlan.zhihu.com/p/605791705)

简单支持了一下人格识别：


<div align=center>
  <img src="https://pic3.zhimg.com/80/v2-b5c475c2d73d4f6358e09e84c60a2d62_1440w.jpg" style="width: 70%;"/>
</div>



这篇文章就将简单讲讲如何将openai的服务接入QQ中。

对应服务端的代码我已经上传到我的GitHub上，欢迎star和提issue：[go-cqhttp-python-server](
​github.com/LSTM-Kirigaya/go-cqhttp-python-server)

---

# 原料

为了实现本篇文章的效果，你需要：

- 一个openai账号
- 一个QQ小号
- 一台可以正常使用的Linux云服务器
- python>=3.5
- 足够的耐心

---


## Step1 获取openai api key

openai提供了三种方式来支持用户基于API的调用：python，js，命令行工具发起请求，这些在openai的官网上写得很清楚了。但是无论是哪种，都需要openai api key才能成功访问，在注册完openai的账号后，可以访问如下链接来创建openai api key：OpenAI API


<div align=center>
  <img src="https://pic2.zhimg.com/v2-34d3863396c8e9ece90a7d35394def09_r.jpg" style="width: 70%;"/>
    <p class="img-caption">点击红框中按钮生成key</p>


</div>


将生成的key保存在电子记事本中，然后注册进入环境变量中，打开.bashrc或者.zshrc，在末尾写入：

```bash
export OPENAI_API_KEY=<openai api key>
```

保存后刷新。

---

## Step2 测试openai的文本生成API

openai的api在国内就可以使用（现在不行了。。。），不需要给你的应用程序指定代理服务器，这一点还是很省心的。

新建文件，输入：

```python
import openai
import os

openai.api_key = os.environ['OPENAI_API_KEY']

if __name__ == '__main__':
    question = input('enter your question:')
    response = openai.Completion.create(
        model="text-davinci-003",
        prompt=question,
        temperature=0.7,
        max_tokens=1024,
        top_p=1,
        frequency_penalty=0,
        presence_penalty=0
    )
    choices = response['choices']
    first_item = choices[0]
    answer: str = first_item['text']
    print('> ', answer.strip())
```

然后运行上述代码，输入你的问题，看看片刻后是否有结果（如果速度太慢，可以尝试把max_tokens调小一点）：


成功返回结果，说明openai key没问题。

---

## Step3 部署go-cqhttp让服务器登录QQ小号

首先，我们需要下载go-cqhttp的可执行文件，这个项目可以让你的服务器或者PC模拟某种移动设备（IPad，安卓手机，安卓手表）登录QQ账号。

进入release发行界面：[Releases · Mrs4s/go-cqhttp (github.com)](https://link.zhihu.com/?target=https%3A//github.com/Mrs4s/go-cqhttp/releases)

由于我们的是64位的Linux服务器，所以下载下图红框框中的版本：


<div align=center>
  <img src="https://pic1.zhimg.com/v2-27fcb2317cc2a8701a54a6a5ba5bfc8c_r.jpg" style="width: 70%;"/>
</div>


登录你的服务器，新建一个qqbot文件夹，进入这个文件夹：

```bash
wget https://github.com/Mrs4s/go-cqhttp/releases/download/v1.0.0-rc4/go-cqhttp_linux_amd64.tar.gz
tar -zxvf go-cqhttp_linux_amd64.tar.gz
```

为了持久化运行，我们创建一个虚拟终端，没有安装screen的同学可以apt/yum install screen：

```bash
screen -S qqbot
```

解压后运行一次go-cqhttp来生成初始配置文件，我选择的连接方式为http，然后退出：

```bash
./go-cqhttp -faststart
```

此时，文件夹下会出现config.yml，在这个文件中配置一下登录配置，需要修改的地方只有两处。

第一处，账号与密码：

<div align=center>
  <img src="https://pic1.zhimg.com/v2-25da4318bdc888449423bab7988c58a4_r.jpg" style="width: 70%;"/>
</div>



注意，uin是number类型，password是string类型

第二处，反向HTTP的post的url：

<div align=center>
  <img src="https://pic2.zhimg.com/v2-ce6637c97e300e7c8b29e2ed1b7a8831_r.jpg" style="width: 70%;"/>
</div>


其中，5700是go-cqhttp的监听端口，go-cqhttp会监听5700端口，一旦有响应则将捕获到的响应发送出去。后面我们通过python编写本地server服务时可以通过向5700端口发送GET请求来实现QQ消息的发送。

post中的5701则是go-cqhttp用来发送数据的端口，我们如此配置之后，一旦go-cqhttp接收到消息，就会向http://127.0.0.1:5701发送post消息，因此，我们可以通过接受post请求来实现在python server获取QQ消息。

因此，请保证5700和5701端口是开放的，如果被占用了，请换别的端口。

> 综上所述，你无需在防火墙中放行这两个端口

然后重新运行go-cqhttp：

```bash
./go-cqhttp -faststart
```

请根据指示一步步完成登录，其中验证环节使用滑块登录，毕竟你的云服务器和你的手机肯定不在一个局域网内。

先让你的小号加你的大号，运行成功后，如果你给你的小号发消息，你应该可以在命令行上看到你发送的内容。

如果看到了发送的消息，说明go-cqhttp部署成功了，按下`Ctrl+A+D`退出当前虚拟终端。

---

## Step4 编写 python 服务端

根据之前的简述，我们只需要在python端简单地监听POST和发送GET就能完成QQ信息的接受和发送了。首先根据上面的理解，我们需要根据config.yml中的配置来配置我们的flask服务，先来尝试写一个复读机吧！

```python
from flask import Flask, request
import requests
import yaml
from typing import Tuple

app = Flask(__name__)


def get_host_port() -> Tuple[str, int]:
    with open('./config.yml', 'r', encoding='utf-8') as f:
        obj = yaml.load(f.read(), Loader = yaml.FullLoader)
    url = obj['servers'][0]['http']['post'][0]['url']
    host, port = url.replace('http://', '').split(':')
    return str(host), int(port)


@app.route('/', methods=["POST"])
def post_data():
    raw_package: dict = request.get_json()
    if 'message_type' in raw_package:
        message_type = raw_package['message_type']
        user_id = raw_package['sender']['user_id']
        message = raw_package['message']
        get_request = "http://127.0.0.1:5700/send_private_msg?user_id={}&message={}".format(user_id, message)
        # 发送消息
        requests.get(get_request)

    return 'ok'


if __name__ == '__main__':
    from gevent import pywsgi

    host, port = get_host_port()
    server = pywsgi.WSGIServer(
        listener=(host, port),
        application=app,
        log=None
    )
    server.serve_forever()
```

运行后，尝试和机器人聊天：

<div align=center>
  <img src="https://pic4.zhimg.com/v2-7f6b5bca3741fd16bfaea44ac5256057_r.jpg" style="width: 70%;"/>
</div>



然后我们只需要将复读的那个环境，用openai的服务代替即可：

```python
import openai
import os

openai.api_key = os.environ['OPENAI_API_KEY']

def openai_reply(question) -> str:
    response = openai.Completion.create(
        model="text-davinci-003",
        prompt=question,
        temperature=0.7,
        max_tokens=1024,
        top_p=1,
        frequency_penalty=0,
        presence_penalty=0
    )
    choices = response['choices']
    first_item = choices[0]
    return first_item['text']
    

@app.route('/', methods=["POST"])
def post_data():
    raw_package: dict = request.get_json()
    if 'message_type' in raw_package:
        message_type = raw_package['message_type']
        user_id = raw_package['sender']['user_id']
        message = raw_package['message']
        res = openai_reply(message)
        get_request = "http://127.0.0.1:5700/send_private_msg?user_id={}&message={}".format(user_id, res)
        # 发送消息
        requests.get(get_request)

    return 'ok'
```

效果：

<div align=center>
  <img src="https://pic4.zhimg.com/v2-4dbf00e30f04e936604df4935128f5ef_r.jpg" style="width: 70%;"/>
</div>


可以看到，我们的机器人已经可以成功调用openai服务来返回结果了。但是仍然有不少问题：

回答的句子有停用词+若干空格作为前缀。
会重复响应同一个问题。
针对这些问题我们需要进行优化。

## Step5 改进 & 优化

go-cqhttp已经帮我做好了单线程化处理，保证消息都是处理好一条再处理下一条。因此我们只需要优化上述提出的问题。

限于篇幅，我就不讲如何优化上述的问题了，经过优化的项目我已经开源了：[go-cqhttp-python-server](
​github.com/LSTM-Kirigaya/go-cqhttp-python-server)

开箱即用，直接点个赞（狗头）然后clone下来按照readme上面的步骤一步步部署即可。

当然，为了防止一堆人发送请求导致服务我的服务饥饿，设一个机器人的回复权限还是很有必要的：

<div align=center>
  <img src="https://pic3.zhimg.com/v2-9365e3a1e226dbfd89407fab0f91be8e_r.jpg" style="width: 40%;"/>
</div>


---

## 无聊ing

我还加了一个简单的指令系统：

<div align=center>
  <img src="https://pic3.zhimg.com/v2-c28ff1458fd78c1fce56672c367b93ae_r.jpg" style="width: 70%;"/>
</div>


通过`tip.command.XXX`就能调用，想要拓展自己的指令只需要在我的项目的qq_server/tip_command.py编写函数即可，函数需要满足：

- 完整的输入参数的类型标注
- 返回值为字符串或者返回类型自带__str__方法
- 不能以下划线开头

---

## 一些可能的问题

### 云服务器的DNS

这个问题应该只有腾讯云用户有，go-cqhttp会提示你长消息延迟测试失败，请把DNS改成114.114.114.114，只需要修改vim /etc/resolv.conf这个文件即可：

<div align=center>
  <img src="https://pic4.zhimg.com/v2-c616fa14699df3cfd70fbf0cfae05623_r.jpg" style="width: 70%;"/>
</div>




### 密码不对或账号冻结

之前运行时发现go-cqhttp崩了：

<div align=center>
  <img src="https://pic1.zhimg.com/v2-0ca9a7b53f12da52a8a7f5762c088998_r.jpg" style="width: 70%;"/>
</div>



而且无论如何重启都无效，后来去翻了一下issue，那些修改设备的方法也不太行，后来我成功解决了这个问题：

在你的windows PC上下载Windows版本的go-cqhttp，在Windows上扫码登录你的QQ后退出（必须保证你的PC和你的手机在同一局域网下），将生成的session.token和device.json上传到服务器上，就可以直接启动了。

---

# 参考资料

- [Mrs4s/go-cqhttp: cqhttp的golang实现，轻量、原生跨平台. (github.com)](https://link.zhihu.com/?target=https%3A//github.com/Mrs4s/go-cqhttp)
- [【Re：从零开始的QQ机器人搭建】——基于go-cqhttp和python_世界第一可爱不咕鸟的博客-CSDN博客_go-cqhttp python](https://link.zhihu.com/?target=https%3A//blog.csdn.net/u011897679/article/details/114607381)