> 我不否认完整的光亮，但是我仍然拥抱破碎；不能因为完整的美丽而去否认破碎的。就像村上春树的《雪国》的结局一样，女主“破碎”了，但是男主看到了星空，是那么耀眼美丽。


由于拿到了宝贝8卡服务器，那必须运行一个python后端来转发AI请求。`flask`用起来非常简单，但是可拓展性太差，遂选择`Django`。简单记录一下Django的使用。

## 安装

直接安装即可：

```bash
# Windows
pip install django

# Linux
pip3 install django

# anaconda
conda install django
```

注意，`django`安装完后，还会注册一个全局的`django-admin`用于管理。请确保该工具可以在命令行中被访问。

---

## 创建项目

使用如下指令创建新的Django项目：

```bash
django-admin startproject <project name>
```

因为我的项目起名为`DustAI`，所以这个指令会在当前目录下创建一个包含如下文件的项目：

```python
.
|-- DustAI
|   |-- __init__.py		#  一个空文件，告诉 Python 该目录是一个 Python 包。
|   |-- asgi.py			#  一个 ASGI 兼容的 Web 服务器的入口，以便运行你的项目。
|   |-- settings.py		#  该 Django 项目的配置文件。
|   |-- urls.py			#  该 Django 项目的 URL 声明; 一份由 Django 驱动的网站"目录"。
|   `-- wsgi.py			#  一个 WSGI 兼容的 Web 服务器的入口，以便运行你的项目。
`-- manage.py			#  一个实用的命令行工具，可让你以各种方式与该 Django 项目进行交互。
```



## 应用模型

当你在Django项目中创建、更改或删除模型（即数据表）时，需要运行`migrate`命令来应用这些更改。这将自动检测并执行需要更新的数据库迁移操作。

在你第一次运行Django项目时，通常需要运行如下命令以创建数据库表：

```bash
python3 manage.py migrate
```

此后，每当更改了模型定义时，都需要再次运行上面的指令，以便将这些更改应用到数据库中。



## 运行项目

通过如下的命令

```python
python3 manage.py runserver 8000
```

如果你希望通过公网IP进行访问，使用`0.0.0.0:8000`，并请在`setting.py`中修改配置`ALLOWED_HOSTS`和`DEBUG`：

```python
# SECURITY WARNING: don't run with debug turned on in production!
DEBUG = False

ALLOWED_HOSTS = ['你的IPv4地址']
```

> 建议将DEBUG修改为`False`，避免暴露服务器隐私信息。
>
> manage.py的`runserver`指令不需要重新运行，和flask相似，是动态刷新热更新的。



## 第一个GET请求

我们将第一个请求写在`DustAI/view.py`中：

```python
from django.http import HttpResponse

def hello(request):
    return HttpResponse("hello world!")
```

然后将这个函数注册进hello路由中，打开urls.py，覆盖写入：

```python
from django.urls import path
import DustAI.view as view

urlpatterns = [
    path("hello", view.hello)
]
```

在浏览器中输入`0.0.0.0:8000/hello`或者`公网IP:8000/hello`，得到如下响应。

```
hello world!
```

> 为了方便调试，可以使用postman进行命令的发起。



## 获取GET参数

```python
from django.http import JsonResponse, HttpRequest

def hello(request: HttpRequest):
    name = request.GET.get('name')
    data = {
        'code': 200,
        'data': {
            'name': name
        }
    }
    return JsonResponse(data)
```

访问`0.0.0.0:8000/hello?name=hello`：

```json
{
    "code": 200,
    "data": {
        "name": "hello"
    }
}
```



## 获取POST参数

```python
from django.views.decorators.http import require_http_methods

@require_http_methods(['POST'])
def login(request: HttpRequest):
    username = request.POST.get('username')
    password = request.POST.get('password')

    data = {
        'code': 200,
        'data': {
            'username': username,
            'password': password
        }
    }
    # 对POST请求进行处理...
    return JsonResponse(data)
```

> 通过`require_http_methods`来约束函数只能使用哪些类型的请求

不要忘记注册进urlpatterns：

```python
urlpatterns = [
    path("hello", view.hello),
    path("login", view.login),
]
```

打开postman，切换成POST请求模式，在Body中填入：

| username | k |
| --- | --- |
| password | 123 |

发送请求后得到结果：

```json
{
    "code": 200,
    "data": {
        "username": "k",
        "password": "12356"
    }
}
```


---

## 参考资料

- [Django 创建第一个项目 | 菜鸟教程 (runoob.com)](https://www.runoob.com/django/django-first-app.html)