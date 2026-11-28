## 前言
不知道各位有没有遇到过一个需求场景——自己实现了一个算法功能，然后希望团队内的其他成员（都在一个内网中）可以无门槛地直接使用你的算法功能，从而加速整个团队的工作效率。很显然，Web 应用是实现这个需求的一个很好的途径。但是想要实现一个低代码、美观程度尚可、拥有缓存机制的前端应用往往都需要不少的时间。如何在较短时间（5-10分钟内）快速实现一个满足上述要求的 Web 应用呢？

Python 中的 `streamlit` 和 `gradio` 给出了答案。这两个框架就是为了给没有前端经验的深度学习算法工程师一个好用的框架工具，让他们能够在极短时间内根据自己的算法快速构建一个 Web 应用。

`streamlit` 和 `gradio` 的使用和运行逻辑不太一致。我个人更加喜欢 `streamlit` 的编写方式，更加直观。所以这篇文章只介绍 `streamlit` 的使用方式，希望使用 `gradio` 的朋友可以直接移步他们的官方：

- [gradio 官网](https://gradio.app/) 
- [streamlit 官网](https://streamlit.io/)


---

## 我的 Web 应用展示

为了让读者更加切实地了解到什么是 Web 应用，以及有什么用。我简单举两个例子。

下图是我花了 5 分钟用 `streamlit` 构建的图像超分辨率重建的 Web 应用：

<div align=center>
<img src="https://picx.zhimg.com/80/v2-88e205f96009129927eb0330fdfbf74e_1440w.png" style="width: 80%;"/>
</div>

这个项目我已经开源了（好几年前的项目了），阁下可以开箱即用： [super-easy-super-resolution](https://github.com/LSTM-Kirigaya/super-easy-super-resolution) .

前一阵子，我还简单用 `streamlit` 花了 30 分钟（算法 + 前端），构建了一个 IGS 检测程序，加速整个实验室的中心线校准工作，只是一个检测程序，所以做得很粗糙：

<div align=center>
<img src="https://picx.zhimg.com/80/v2-a89205f8c2087df5bc6f1038cca55caa_1440w.png" style="width: 80%;"/>
</div>

相信不少读者已经大致理解了这类应用的设计边界（design boundary）了，就是希望在最短时间内实现一个 UI 尚可（尽可能不去折腾任何的UI设计，主打一个开箱即用），具有基本多媒体交互功能的 Web 应用。如果阁下目前或者将来有类似的需求，那么这篇文章或许能帮到你。

---

## 傻瓜式 Web 框架 streamlit


接下来就是正文了，在讲 `streamlit` 使用方法之前，读者一定一定需要先了解这玩意儿的运行逻辑，如果你能理解 `streamlit` 的运行机制后，您就会瞬间学会如何使用它（甚至可以不用看我的教程）。

在正式开始前，我们不妨先安装一下 `streamlit`，顺便喝口咖啡：

```bash
$ pip install streamlit
```

### 从 Hello World 开始的 streamlit 运行原理介绍

我们先创建一个简单的 `app.py`：

```bash
import streamlit as st 

# 设置全局属性
st.set_page_config(
    page_title='我是标题',
    page_icon='🐳',
    layout='wide'
)

# 正文
st.title('hello world')
st.markdown('> Streamlit 支持通过 st.markdown 直接渲染 markdown')
```

打开命令行，运行：
```bash
$ streamlit run app.py
```

这个命令会自动打开浏览器的一个标签页，该服务默认以8501为端口。

> 如果8501被占用， streamlit会往后遍历，直到找到可用端口为止。开启的服务默认可以在内网上所有的计算机上访问。

如下，我已经将图中的各项说明和代码的对应标注好了：

<div align=center>
<img src="https://picx.zhimg.com/80/v2-8815b24562b2b665e250df27b792d9fd_1440w.png" style="width: 80%;"/>
</div>


下面简单介绍一下 streamlit 是如何渲染出这个页面的。当我们运行 `streamlit run app.py` 时，主程序并不是我们的 `app.py`，而是 streamlit 启动脚本，它负责读取我们的 `app.py`，然后根据其中的内容渲染出页面（HTML）和对应的逻辑（JavaScript），一个简化的模型如下（其中还涉及到缓存机制，写起来比较麻烦，为了我宝贵星际争霸时间，我只展示最基本的一个逻辑）：

<div align=center>
<img src="https://picx.zhimg.com/80/v2-456cde30d98c12fa5be03fcf4212372d_1440w.png" style="width: 60%;"/>
</div>

每当运行 app.py 时， `streamlit` 都会逐行执行 `app.py` 中的代码，遇到了 `streamlit` 的方法时， `streamlit` 就会将这句话翻译成前端的 HTML + JavaScript 去渲染页面；如果是正常的 python 语句，那么就交给 python 解释器去执行。所以我非常喜欢 `streamlit` 的逻辑，因为可以完全按照正常的逻辑去编写可以渲染出前端页面的应用。

> 当然，阁下完全不需要担心逻辑代码阻塞渲染进程的问题， `streamlit` 已经全部帮我们解决了。

需要注意的是，每当网页上发生事件响应，比如有人点击了按钮，上传了文件，修改了单选框的选项等等，`streamlit` 就会去重新执行 `app.py`。在这里 `app.py` 显然是以一个静态资源的形式存在的，因此你可以随意修改，毕竟每次触发事件 `streamlit` 都会重新运行 `app.py`。因此，从设计上，`streamlit` 就是热更新，这一点与 `gradio` 很不一样。

---

## 布局控制

对于快速前端框架而言，优雅地让用户快速自定义布局是非常重要的一件事。 `streamlit` 提供了一种极其简单且优雅的方法来实现对于网页布局的控制。

也就是通过 `with st.控制器x:` 来控制。`with` 下的所有 `streamlit` 组件会被渲染到 `控制器x` 下（scope 下，更加 pythonic 的说法是 context ，但是为了阁下更容易理解，我会用 scope 来指代 with 下的代码区域），我们可以简单看一个例子。

### 侧边栏 sidebar

这是一个简单例子，通过 `with` 可以直接实现一个侧边栏。

```python
## st.sidebar 下的内容会被渲染到侧边栏
with st.sidebar:
    st.title('欢迎来到我的应用')
    st.markdown('---')
    st.markdown('这是它的特性：\n- feature 1\n- feature 2\n- feature 3')

## 默认渲染到主界面
st.title('这是主界面')
st.info('这是主界面内容')
```

<div align=center>
<img src="https://picx.zhimg.com/80/v2-a9237903e9363ebfd8117f82a0ae3c31_1440w.png" style="width: 70%;"/>
</div>


除了使用 `with` ，读者还可以直接通过调用容器组件的方法来实现完全相同的功能，下面代码有着和上面完全一致的渲染效果：

```python
## st.sidebar 下的内容会被渲染到侧边栏
sidebar = st.sidebar
sidebar.title('欢迎来到我的应用')
sidebar.markdown('---')
sidebar.markdown('这是它的特性：\n- feature 1\n- feature 2\n- feature 3')

## 默认渲染到主界面
st.title('这是主界面')
st.info('这是主界面内容')
```



### 列 columns

构建应用时，我们总喜欢让想要展示的数据分成两栏，columns 就能实现这一点。

```python
c1, c2 = st.columns(spec=2)

c1.title('This is Column Ⅰ')
c2.title('This is Column Ⅱ')
```

<div align=center>
<img src="https://pic1.zhimg.com/80/v2-beb0d8ffde1b3c12ed47da56eb72cfa4_1440w.png" style="width: 70%;"/>
</div>

### 标签页 tabs

标签页 tabs 可以用于在小空间内展示若干个平级的事物，比如我们可以展示一下不同 API 读取图像的方法：

```python
st.title('Python 不同的库实现图像读入成 numpy 数组')
tab1, tab2, tab3 = st.tabs(['opencv', 'pillow', 'imageio'])

with tab1:
    '''
    ```python
    import cv2
    image = cv2.imread('image.png')
    ```
    '''

with tab2:
    '''
    ```python
    from PIL import Image
    image = Image.open('image.png')
    ```
    '''

with tab3:
    '''
    ```python
    import imageio
    image = imageio.imread('image.png')
    ```
    '''
```

<div align=center>
<img src="https://picx.zhimg.com/80/v2-c8e6577c86b56d6391493adf20dbc3b9_1440w.png" style="width: 70%;"/>
</div>

### 折叠布局 expander

expander 会创建一个可被折叠的布局，用于隐藏一些不太重要的信息。

```python
st.title('猜猜我在哪里？')
expander_1 = st.expander('region 1')
expander_2 = st.expander('region 2')

expander_1.warning('不在这里！')
expander_1.image('https://pica.zhimg.com/80/v2-5e767eeed478fc195d5df472db538640_1440w.png')

expander_2.warning('也不在这里！')
expander_2.image('https://pic1.zhimg.com/80/v2-79ba9b4bee266815fb4069a5de5afea7_1440w.png')
```

<div align=center>
<img src="https://picx.zhimg.com/80/v2-c789a065b73a6ffb2b3ec78380a59579_1440w.png" style="width: 70%;"/>
</div>

### 多元素容器 container

container 定义了一个容器，用于存放多个元素。你可以将它当成一个 rows，因为 streamlit 默认就是按照行的方式排布元素的。一个简单的示例如下：

```python
row_1 = st.container(border=False)
row_2 = st.container(border=True)

col11, col12 = row_1.columns(2)
col11.info('I am col 11')
col12.info('I am col 12')

col21, col22, col23 = row_2.columns(3)
col21.info('I am col21')
col22.warning('I am col22')
col23.error('I am col23')
```

<div align=center>
<img src="https://picx.zhimg.com/80/v2-2ce61216f112c2149324eb72257aab2a_1440w.png" style="width: 70%;"/>
</div>

### 单元素容器 empty
与 container 相对的就是 empty 了，empty 只能存储一个元素，连续调用empty 容器的渲染方法会替换掉上一次的渲染结果。对于需要动态展示数据变化过程的需求而言，empty 很合适。

比如一个简单的计数器：

```python
import time
placeholder = st.empty()

with placeholder:
    for i in range(10):
        st.write(i, ' s')
        time.sleep(1.)

# 清空 empty 内的元素
placeholder.empty()
```

<div align=center>
<img src="https://pic1.zhimg.com/80/v2-3d516975c58d3496abb471c02410cfca_1440w.png" style="width: 70%;"/>
</div>


### 嵌套布局

streamlit 提供的布局组件非常简单，更加复杂的布局可以通过上述布局组件的嵌套组合实现。

上文布局对象本身也拥有和 `streamlit` 对象几乎完全一致的方法，用于在自己的布局内创建布局或者组件。

```python
c1, c2 = st.columns(spec=2)

with c2:
    st.title('column Ⅱ')
    st.info('content in column Ⅱ')

with c1:
    st.title('column Ⅰ')
    c_1_1, c_1_2 = c1.columns(spec=2)

    with c_1_1:
        st.info('sub column 1 in column Ⅰ')

    with c_1_2:
        st.info('sub column 2 in column Ⅱ')
```

<div align=center>
<img src="https://pic1.zhimg.com/80/v2-f2db409c1da32c8ff0d74274b706482f_1440w.png" style="width: 70%;"/>
</div>

嵌套时需要注意，组件被渲染的先后顺序是由他们的父组件被定义的先后顺序决定的，比如下面这个例子：
```python
# 将被渲染在最下方
st.info('this is a info component')

c1, c2 = st.columns(spec=2)

with c2:
    st.title('column Ⅱ')
    st.info('content in column Ⅱ')

with c1:
    st.title('column Ⅰ')
    c_1_1, c_1_2 = c1.columns(spec=2)

    with c_1_1:
        st.info('sub column 1 in column Ⅰ')

    with c_1_2:
        st.info('sub column 2 in column Ⅱ')
```

<div align=center>
<img src="https://picx.zhimg.com/80/v2-8c16b0409113db2831350db2a5f1c8a5_1440w.png" style="width: 70%;"/>
</div>


虽然 st.title(hello) 被定义在了 st.title('column Ⅱ') 和  st.title('column Ⅰ') 上面，但是因为 column Ⅱ 和  column Ⅰ父组件（c1, c2）被定义地更早，所以 c1, c2 中的内容被渲染的优先级更高：

以上就是 streamlit 的所有布局了。基于with或者方法调用，阁下应该能利用 streamlit 快速塑造出想要的大部分形式的布局了！