## 为什么需要pipreqs
requirements.txt是一个合格的python项目、库或者软件包必备的文件，里面涵盖了所有项目使用到的第三方库以及对应的版本号。对于拿到requirements.txt的用户来说，直接一行`pip install -r requirements.txt`，毫不拖泥带水，一行安装完所有需要的依赖库。

但是对于开发者来说，去写requirements.txt可能是一个费时费力的事情，毕竟你或许记得正在使用的Python版本号，但是一定不可能记全自己项目里使用的所有的库对应的版本号。这时，你可以使用一个神器`pipreqs`。

---
## 安装与使用

首先安装：
```bash
pip install pipreqs
```
`pipreqs`可以被python解释器当作模块执行。打开你当前想要得到requirements.txt的项目的根目录。输入`pipreqs ./`，静静等候。

<div align=center>
<img src="https://img-blog.csdnimg.cn/2021021020452253.png" style="width: 80%;"/>
<p class="img-caption">自动获取需要的库</p>
</div>

有以上消息输出，说明生成成功。

> 速度比较慢，因为pipreqs默认是去检索PyPi库的内容，国内访问PyPi比较慢。

检查当前根目录，发现多出了一个`requirements.txt`，这就是生成的。

<div align=center>
<img src="https://img-blog.csdnimg.cn/20210210204616541.png" style="width: 20%;"/>
<p class="img-caption">生成的requirements.txt</p>
</div>

---

## 注意事项(如果你运行失败了请看这里)
使用`pipreqs`时的注意事项：

- 编码问题：加参数`--encoding`，我的项目都是utf-8编码的，所以输入`pipreqs ./ --encoding=utf-8`
- 网络问题：`pipreqs`指令貌似需要访问PyPi去获取指定库的版本号或者牵连的依赖项，所以请保证运行指令时你的计算机能够访问PyPi(没联网或者DNS污染会出现这样的情况)