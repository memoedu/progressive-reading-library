## 前言

最近在开发一个高度定制化的爬虫系统。对于普通的基于 HTTP 的爬取技术，我会在别的博客中记录。这次的主角是 DDSM 数据集，这项数据集的下载官方只给了 ftp 协议下的下载链接：`ftp://figment.csee.usf.edu/pub/DDSM/cases/normals/normal_01`。

## 文件夹类型的 FTP 链接无法直接下载

有同学看到上面的链接就在想了，不就是一个 FTP 协议链接吗？用万能的 wget 直接带走不就行了吗？很遗憾，使用 `wget` 下载会得到如下的报错：

```bash
$ wget -c ftp://figment.csee.usf.edu/pub/DDSM/cases/normals/normal_01
```
out:
```bash
--2023-10-29 13:40:58--  ftp://figment.csee.usf.edu/pub/DDSM/cases/normals/normal_01
           => ‘normal_01’
Resolving figment.csee.usf.edu (figment.csee.usf.edu)... 131.247.2.111
Connecting to figment.csee.usf.edu (figment.csee.usf.edu)|131.247.2.111|:21... connected.
Logging in as anonymous ... Logged in!
==> SYST ... done.    ==> PWD ... done.
==> TYPE I ... done.  ==> CWD (1) /pub/DDSM/cases/normals ... done.
==> SIZE normal_01 ... done.

==> PASV ... done.    ==> RETR normal_01 ...
No such file ‘normal_01’.
```

其实并不是没有 normal_01 这个文件，我们不妨登录 USF 计算机学院的 FTP 服务器看看：

```bash
# windows 下
# 如果是 Linux，取消 -A 参数，并使用 anonymous 作为用户名，密码不用输入
$ ftp -A figment.csee.usf.edu
```

下面是 FTP 会话：

```bash
连接到 aivi.cse.usf.edu。
220 (vsFTPd 2.2.2)
200 Always in UTF8 mode.
331 Please specify the password.
230-For any issues email at tech@cse.usf.edu
230 Login successful.
对 11934@k 匿名登录成功
ftp> cd /pub/DDSM/cases/normals/normal_01
250 Directory successfully changed.
ftp> pwd
257 "/pub/DDSM/cases/normals/normal_01"
ftp> ls
200 PORT command successful. Consider using PASV.
150 Here comes the directory listing.
case0002
case0003
...
case0160
226 Directory send OK.
ftp: 收到 1113 字节，用时 0.00秒 1113000.00千字节/秒。
```

可以看到，其实不是没有 normal_01 这个路径，而是 上面的 FTP 链接指向的，是一个文件夹，而不是文件。 FTP 协议本身是不提供下载文件夹的服务的。想要下载文件夹，网上有一些方法，比如：https://blog.csdn.net/xy_kok/article/details/80818762 ，但是在我的环境下这些方法均失败。

那么一个直观的想法就是，我们能不能先通过某个方法获取文件夹下的所有文件路径，然后再写个循环，逐一下载。这个想法很直观且简单，我们不妨先看看这个服务器上最深层的数据在哪里：

```bash
ftp> cd case0002
250 Directory successfully changed.
ftp> ls
200 PORT command successful. Consider using PASV.
150 Here comes the directory listing.
A-0002-1.ics
A_0002_1.LEFT_CC.LJPEG
A_0002_1.LEFT_MLO.LJPEG
A_0002_1.RIGHT_CC.LJPEG
A_0002_1.RIGHT_MLO.LJPEG
TAPE_A_0002_1.COMB.16_PGM
226 Directory send OK.
ftp: 收到 144 字节，用时 5.36秒 0.03千字节/秒。
```

可以看到，最深层的数据是一堆的 LJPEG 文件，我们只需要将这些小宝贝下载下来就行了。

那么后面怎么说？当然是直接用 Python 写个脚本一次性爬取所有文件咯！

## 使用 python 递归下载 FTP 服务器上的文件夹

python 提供了 FTP 协议的库 `ftplib`，用起来也超级简单。这里我就简单说了，因为下载时间比较长，我们需要一个日志来记录下载过程，加上 登录 FTP 服务器，代码如下：

```python
from typing import *
import os

import ftplib
from loguru import logger

logger.add(
    sink='logs/DDSM.log', 
    format="{time:YYYY-MM-DD HH:mm:ss} | {level} | {message}", 
    level="INFO"
)

ftp = ftplib.FTP(
    host='figment.csee.usf.edu',
    user='anonymous'
)
```

接下来就是递归获取所有文件绝对路径的函数了，我将它写成了一个生成器：

```python
def walk_dir(ftp: ftplib.FTP, dirname: str) -> Generator[str, None, None]:
    ftp.cwd(dirname)

    callback_result: List[Tuple[str]] = []
    def callback(*lines) -> None:
        print_infos = lines[0].split()
        auth = print_infos[0]
        name = print_infos[-1]
        callback_result.append((auth, name))
    
    ftp.dir(callback)

    for auth, name in callback_result:
        item_path = dirname + '/' + name
        # 这是一个文件夹
        if auth.startswith('d'):
            for sub_item_path in walk_dir(ftp, item_path):
                yield sub_item_path
        else:
            yield item_path
```

使用起来也非常简单：

```python
for path in walk_dir(ftp, '/pub/DDSM/cases/normals/normal_01'):
    print(path)
```

out:

```
/pub/DDSM/cases/normals/normal_01/case0002/A-0002-1.ics
/pub/DDSM/cases/normals/normal_01/case0002/A_0002_1.LEFT_CC.LJPEG
/pub/DDSM/cases/normals/normal_01/case0002/A_0002_1.LEFT_MLO.LJPEG
/pub/DDSM/cases/normals/normal_01/case0002/A_0002_1.RIGHT_CC.LJPEG
/pub/DDSM/cases/normals/normal_01/case0002/A_0002_1.RIGHT_MLO.LJPEG
/pub/DDSM/cases/normals/normal_01/case0002/TAPE_A_0002_1.COMB.16_PGM
/pub/DDSM/cases/normals/normal_01/case0003/A-0003-1.ics
/pub/DDSM/cases/normals/normal_01/case0003/A_0003_1.LEFT_CC.LJPEG
/pub/DDSM/cases/normals/normal_01/case0003/A_0003_1.LEFT_MLO.LJPEG
/pub/DDSM/cases/normals/normal_01/case0003/A_0003_1.RIGHT_CC.LJPEG
/pub/DDSM/cases/normals/normal_01/case0003/A_0003_1.RIGHT_MLO.LJPEG
/pub/DDSM/cases/normals/normal_01/case0003/TAPE_A_0003_1.COMB.16_PGM
...
```

拿到了链接，接下来只需要下载它们就行了，完整的代码我直接给出：

```python
from typing import *
import os

import ftplib
from loguru import logger

logger.add(
    sink='logs/DDSM.log', 
    format="{time:YYYY-MM-DD HH:mm:ss} | {level} | {message}", 
    level="INFO"
)

ftp = ftplib.FTP(
    host='figment.csee.usf.edu',
    user='anonymous'
)

def walk_dir(ftp: ftplib.FTP, dirname: str) -> Generator[str, None, None]:
    ftp.cwd(dirname)

    callback_result: List[Tuple[str]] = []
    def callback(*lines) -> None:
        print_infos = lines[0].split()
        auth = print_infos[0]
        name = print_infos[-1]
        callback_result.append((auth, name))
    
    ftp.dir(callback)

    for auth, name in callback_result:
        item_path = dirname + '/' + name
        # 这是一个文件夹
        if auth.startswith('d'):
            for sub_item_path in walk_dir(ftp, item_path):
                yield sub_item_path
        else:
            yield item_path


for path in walk_dir(ftp, '/pub/DDSM/cases/normals/'):
    save_path = path.replace('/pub', '/storage')
    save_dirname = os.path.dirname(save_path)
    if not os.path.exists(save_dirname):
        os.makedirs(save_dirname)
    ftp_path = 'ftp://' + (ftp.host + path).replace('//', '/')
    try:
        fp = open(save_path, 'wb')
        logger.info('download {} to {}'.format(ftp_path, save_path))
        ftp.retrbinary('RETR ' + path, fp.write, blocksize=1024)
        fp.close()
    except Exception:
        logger.exception('exception happens when downloading ' + ftp_path)
```