<center>
<iframe frameborder="no" border="0" marginwidth="0" marginheight="0" width=330 height=86 src="//music.163.com/outchain/player?type=2&id=2098477269&auto=1&height=66"></iframe>
</center>

## 前言

最近需要将 A800 上的海量数据（大概几百个T）搬到数据服务器上，但是直接一次 `scp -r` 搞到后面直接 pipe broken 了，所以我们需要分批次传输，比如一个个数据文件夹传输。

由于 scp 传输是需要输入密码的，所以我们需要编写一个程序，可以自动化输入 scp 密码。在 python 中，可以使用 `pexpect` 这个库实现，安装如下：

```bash
pip install pexpect
```

---

## 代码

我们需要让 scp 的文字重定向到 std out 上面，而且还要注意 timeout 的问题，一个模板代码如下：

```python
import pexpect
import os
import sys

names = [ ... ]
password = 'xxxxxxx'

def scp_transfer(scp_command, password, timeout=6000):
    # logfile=sys.stdout.buffer: 将 scp 的 log 重定向到标准输出的缓冲区
    # timeout=timeout: 单次 scp 的最大传输时间，最好开大点，默认的值很小
    child = pexpect.spawn(scp_command, logfile=sys.stdout.buffer, timeout=timeout)
    child.expect('password:')
    child.sendline(password)
    child.expect(pexpect.EOF)

for name in names:
    scp_command = ... # 根据规则构造 scp 命令
    scp_transfer(scp_command, password)
```