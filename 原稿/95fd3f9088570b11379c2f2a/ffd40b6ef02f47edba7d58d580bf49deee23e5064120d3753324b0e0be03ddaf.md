相信诸位在实际生产生活中都遇到过一个需求：当你执行了一个非常耗时的任务，希望服务器或者什么联网的实验装置在运行结束后可以自动提醒你任务结束，或者在运行到一半时，告诉你当前的实验进度，这样就可以节省一部分服务器空转的时间了。

> 可能的应用场景：
- 训练深度学习模型。
- 在服务器上运行大型爬虫程序。
- 在服务器上进行大型 CI 或者 UT。
- 在局域网设备上进行什么奇奇怪怪的可以用程序检测结束的真实世界的物理、化学或者生物实验。（比如在连接电脑的 PET 上做什么我也不太懂的 DNA 试剂合成，你可以用 CV 算法来检测何时反应完成）

其实用过一些昂贵实验装置，比如超级计算机或者GPU 集群服务器的同学应该都熟悉这两个大家伙的提醒策略：发邮件。比如 Slurm 集群，在用于提交的任务结束后，会自动发邮件（这个可以设置）给用户，让用户来认领他的实验结果，毕竟邮件会出现弹窗，就算你在实验室打星际，也能知道有人在找你。。。

所以这篇博客，我打算用尽可能短的篇幅，写一下如何直接 copy 一个开箱即用的可用程序控制发送的自动邮箱发送程序。

## 第一步：注册邮箱

首先注册一个邮箱，并开通这个邮箱的 SMTP 服务。然后确定这个邮箱的 SMTP 服务器的端口，不知道的去问管理员或者猜（25，465或587）。

为了后续方便起见，我建议将邮箱名加入环境变量 `EMAIL_USER` 中，将邮箱密码加入环境变量 `EMAIL_PASSWORD` 中。

```bash
# windows 用户直接搜索环境变量，然后在 “用户环境” 变量这一栏写入变量名和值

# Linux & Mac 用户在 .bashrc 或者 .zshrc 中加入
export EMAIL_USER=<你的邮箱用户名（带@的）>
export EMAIL_PASSWORD=<邮箱密码>
```

## 第二步：复制我的代码

复制下面的代码
```python
import os
from types import TracebackType
from typing import List
import smtplib
from email.mime.text import MIMEText
from email.mime.multipart import MIMEMultipart
from email.mime.image import MIMEImage

class SmtpEmailSender():
    def __init__(self, host: str, port: int, account: str, password: str, nickname: str=None) -> None:
        self.host = host
        self.port = port
        self.user = account
        self.password = password
        self.nickname = nickname if nickname is not None else account

        try:
            self.smtp_client = smtplib.SMTP()
            self.smtp_client.connect(host, port)
            self.smtp_client.login(account, password)
        except Exception as e:
            print('邮件初始化错误，原因如下：')
            print(e)
            exit()
    
    def send(self, receivers: str | List[str], title: str, content: str | None = None, attechments: List[MIMEText] | None = None):
        if isinstance(receivers, str):
            receivers: List[str] = [receivers]
        message_string = self.resolve_message_string(receivers, title, content, attechments)
        self.smtp_client.sendmail(self.user, receivers, message_string)
    
    def resolve_message_string(self, receivers: List[str], title: str, content: str | None = None, attechments: List[MIMEText] | None = None) -> str:
        assert len(receivers) > 0 and isinstance(receivers, list)

        mime_message = MIMEMultipart()
        mime_message['From'] = self.nickname
        mime_message['To'] = receivers[0] if len(receivers) == 1 else receivers
        mime_message['Subject'] = title

        if content:
            content_message = MIMEText(content, 'plain', 'utf-8')
            mime_message.attach(content_message)
        if attechments and isinstance(attechments, list) and len(attechments) > 0:
            for attechment in attechments:
                mime_message.attach(attechment)
        
        return mime_message.as_string()

    def make_image_attachment(self, image_path: str) -> MIMEText:
        assert os.path.exists(image_path), f'{image_path} 图像文件不存在'

        image_name = os.path.basename(image_path)
        with open(image_path, 'rb') as fp:
            mime = MIMEImage(fp.read())
            mime['Content-Type'] = 'application/octet-stream'
            mime['Content-Disposition'] = f'attachment;filename="{image_name}"'
        return mime

    
    def __exit__(self, exc_type: type[BaseException] | None, exc_value: BaseException | None, tb: TracebackType | None) -> None:
        super().__exit__(exc_type, exc_value, tb)
        self.close()
```

## 第三步：发送邮件

先选好一个你要发送的人的邮箱，然后：
```python
if __name__ == '__main__':
    email = SmtpEmailSender(
        host='email.ustc.edu.cn',
        port=25,
        account=os.environ['EMAIL_USER'],
        password=os.environ['EMAIL_PASSWORD']
    )

    email.send(
        receivers='倒霉蛋@qq.com',
        title='下载程序完成（附带表情包）',
        content='下载程序完成，参数如下：xxx xxx'
    )
```

运行程序后，打开 `倒霉蛋@qq.com` 的邮箱，你会发现，邮箱已经发给 `倒霉蛋@qq.com` 的邮箱了：

<div align=center>
<img src="https://pica.zhimg.com/80/v2-c7d93abb7687c20c15b335149e66f880_1440w.png?source=d16d100b" style="width: 60%;"/>
</div>