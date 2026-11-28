
对之前的基于有道翻译的工作进行了优化，简单记录一下：

```python
import requests as r
def youdao_translate(query, from_lang='AUTO', to_lang='AUTO'):
    url = 'http://fanyi.youdao.com/translate'
    data = {
        "i": query,  # 待翻译的字符串
        "from": from_lang,
        "to": to_lang,
        "smartresult": "dict",
        "client": "fanyideskweb",
        "salt": "16081210430989",
        "doctype": "json",
        "version": "2.1",
        "keyfrom": "fanyi.web",
        "action": "FY_BY_CLICKBUTTION"
    }
    res = r.post(url, data=data).json()
    return res['translateResult'][0][0]['tgt']
```

> 默认输入英文，翻译成简体中文，输入中文，翻译成英文

使用方法：

```python
>>> youdao_translate('你好，今天天气真不错')
Hello, it's very nice weather today
>>> youdao_translate('bye, have a nice day!')
再见,祝你有美好的一天!
```

对于没有中英混杂的领域，响应速度非常快。