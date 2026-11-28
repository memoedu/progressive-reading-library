## 第一步：下载破解器

先在 [windows_tools](https://kirigaya.cn/kcloud/storage?path=/windows_tools) 中下载准备好的破解器： `amtemu.v0.9.2-painter.exe`

<div align=center>
<img src="https://picx.zhimg.com/80/v2-3dbb5bcbce983ea8ab7f88cdb6f97c63_1440w.png" style="width: 80%;"/>
</div>

## 第二步：安装 Adobe 软件

断开网络连接，安装对应的 Adobe 软件的试用版本。然后不要打开安装完成的软件。

## 第三步：使用破解器

运行 `amtemu.v0.9.2-painter.exe`，选择你安装的 Adobe 软件。比如我要破解的是 Adobe 的 pdf 编辑软件，所以选择 Acrobat。

<div align=center>
<img src="https://pic1.zhimg.com/80/v2-a191d1dc3079d7701f5464f9a88129ce_1440w.png" style="width: 50%;"/>
</div>

在 Advanced 中还可以选择 Force lang for apps，选择 `zh_CN` 就是中文。

然后点击 Install 后，会出现文件选择窗口，找到你安装的 Adobe 软件的 `amtlib.dll` 链接库。默认地址在

```
C:\Program Files (x86)\Adobe\Acrobat 2017\Acrobat
```

其中 `Acrobat 2017` 就是你安装的产品的版本，替换成你自己的即可。在这个地址下，你应该能找到名为 `amtlib.dll` 的文件，在 Install 开启的文件选择器中选择它，然后点击确认即可。

如果此时控制台上出现了如下的字眼，说明安装成功：

<div align=center>
<img src="https://picx.zhimg.com/80/v2-68f193b5ad68d0cf07b037d3eba017af_1440w.png" style="width: 60%;"/>
</div>

此时连接网络后，可以直接使用对应软件了：

<div align=center>
<img src="https://pic1.zhimg.com/80/v2-47cac3556418f857d36ba710fe76b01a_1440w.png" style="width: 60%;"/>
</div>
