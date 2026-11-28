简单记录一下后端常用的两种语言：`python`与`go`进行http请求的常用操作。

## Python

### 基本用法

python不必多说，大部分人应该都知道使用requests就可以很简单地进行http请求：

```python
import requests

res = requests.get(url="https://kirigaya.cn")
print(res.status_code)
print(res.text)
```

其中，`status_code`是状态码，`text`是响应的字符。如果你确定返回的数据是json数据包，可以直接调用`json()`方法进行解析：

```python
res = requests.get(url="https://www.dmoe.cc/random.php?return=json")
if res.status_code == 200:
    print(res.json())
```

out:

```json
{
    'code': '200', 
    'imgurl': 'https://cdn.cdnjson.com/tvax3.sinaimg.cn//large/0072Vf1pgy1foxkc5tuavj31hc0u0kc9.jpg',
    'width': '1920', 
    'height': '1080'
}
```

### 设置请求体与请求头

一个使用python模拟浏览器，进行带请求头的post模板如下：

```python
res = requests.post(
    url='http://xxx/login',
    headers={
        'Content-Type': 'application/json',
        'Host': 'xxx',
        'Origin': 'http://xxx',
        'Referer': 'http://xxx/',
        'User-Agent': 'Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/111.0.0.0 Safari/537.36',
        'Cookie': 'xxx'
    },
    data=json.dumps({"username":"xxx", "password":"xxx"})
)
```

需要注意的是，data参数一定要传入string而不是dict，因为默认情况下requests的data并不是按照dict进行解析的。

### 解析多个返回json

对于返回值是jsonl类型的数据，使用`json()`解析会出错，此时需要通过response的`iter_lines`方法对于返回值进行遍历：

```python
import io
import json

json_objects = []

for line in response.iter_lines():
    if line:
        buf = io.StringIO(line.decode('utf-8'))
        json_obj = json.load(buf)
        json_objects.append(json_obj)
```

---

## Go

节约时间，使用Go进行多返回值解析和请求的模板如下：

```go
package main

import (
	"bytes"
	"encoding/json"
	"fmt"
	"net/http"
)

func main() {
	// 设置请求体
	requestBody := []byte(`{
        "prompt": "hello", 
        "options": {}
    }`)

    req, err := http.NewRequest("POST", "https:/xxx", bytes.NewBuffer(requestBody))
	if err != nil {
		fmt.Println("获取地址错误")
	}

	// 设置请求头
	req.Header.Set("Content-Type", "application/json")
	req.Header.Set("Host", "xxx")
	req.Header.Set("Origin", "httpx://xxx")
	req.Header.Set("Referer", "http://xxx/")
	req.Header.Set("User-Agent", "Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/111.0.0.0 Safari/537.36")
	req.Header.Set("Cookie", "xxx")

	// 发送
	client := &http.Client{}
	response, err := client.Do(req)
	
	if err!=nil {
		fmt.Println("登录错误")
	}
	defer response.Body.Close()
	
	fmt.Println(response.StatusCode)

	decoder := json.NewDecoder(response.Body)
	var id, text string
	for {
		var obj map[string]interface{}
		err := decoder.Decode(&obj)
		if err != nil {
			break 
		}

		id = obj["id"].(string)
		text = obj["text"].(string)
	}

	fmt.Println(id)
	fmt.Println(text)
}
```

