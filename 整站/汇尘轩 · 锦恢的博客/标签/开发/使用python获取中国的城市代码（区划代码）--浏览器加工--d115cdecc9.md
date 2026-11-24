由于需要使用国家气象局的天气预报的API，所以需要知道国内各个城市的城市代码（也叫区划代码）。通过对`http://www.weather.com.cn/weather1d/101191101.shtml`简单抓包后，找到了方法。代码在文章最后。

> 如果多年后这篇文章的请求目标失效，请从上方的天气局的链接中重新抓包。


## 发起请求

发起如下的GET请求：

```bash
https://j.i8tq.com/weather2020/search/city.js
```

其中请求头设置如下的三个参数：

| 请求头名   | 值                                                           |
| ---------- | ------------------------------------------------------------ |
| User-Agent | Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/115.0.0.0 Safari/537.36 Edg/115.0.0.0 |
| Referer    | http://www.weather.com.cn/                                   |
| Host       | j.i8tq.com                                                   |



## 预处理

- 获取到的是js代码，并不是json，需要把开头的`var city_data = `去除。
- 得到的是一个具有层级结构的字典，查询时不太方便，需要展开。



## 代码

基于以上认识，我们得到了如下的python代码用于获取干净的json：

```python
import requests as r
import json
from typing import Dict

response = r.request(
    method='get',
    url='https://j.i8tq.com/weather2020/search/city.js',
    headers={
        'User-Agent': 'Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/115.0.0.0 Safari/537.36 Edg/115.0.0.0',
        'Referer': 'http://www.weather.com.cn/',
        'Host': 'j.i8tq.com'
    }
)

raw_text = response.text
first_bracket_index = raw_text.index('{')
raw_json = raw_text[first_bracket_index:]
weather_json: dict = json.loads(raw_json)

stack = [weather_json[k] for k in weather_json]
name2code: Dict[str, str] = {}

while len(stack) > 0:
    q: Dict[str, Dict] = stack.pop()
    for sq in q.values():
        area_id = sq.get('AREAID', '')
        name_cn = sq.get('NAMECN', '')
        if area_id:
            name2code[name_cn] = area_id
        else:
            stack.append(sq)

areas = sorted(name2code, key=lambda x:name2code[x])
name2code = {a: name2code[a] for a in areas}
with open('city_code.json', 'w', encoding='utf-8') as fp:
    json.dump(name2code, fp, indent=4, ensure_ascii=False)
```

运行上述代码，最终得到的`city_code.json`预览如下：

```json
{
    "北京": "101010100",
    "海淀": "101010200",
    "朝阳": "101010300",
    "顺义": "101010400",
    "怀柔": "101010500",
    "通州": "101010600",
    "昌平": "101010700",
    "延庆": "101010800",
    ...
}
```

基于这个json构建映射函数轻而易举，于此不再赘述。