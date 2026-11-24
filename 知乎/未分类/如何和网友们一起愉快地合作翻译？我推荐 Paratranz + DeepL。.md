# 如何和网友们一起愉快地合作翻译？我推荐 Paratranz + DeepL。

> [原文链接](https://zhuanlan.zhihu.com/p/412325171)

记得上次写原创……还是在上次？哈哈开个玩笑，不知道大家有没有注意到，我最近发布翻译文章的速度快了不少？质量还变高了？本文我就讲讲如何提高**业余**翻译效率。

面向读者：有一定编程基础、用爱发电的业余翻译者

## 更新

感谢 Shom 编写的翻译教程：

[Shom：Thoughts Memo 汉化组 ParaTranz 翻译教程](https://zhuanlan.zhihu.com/p/562739389)## 背景

在 18 年，我就用业余时间搞翻译了，虽然我英语不是很好，但还是做了几十万字的翻译。返回去看那些翻译，质量惨不忍睹。痛定思痛之后，我打算召集一些志愿者，一起翻译，互相校对，兼顾翻译质量和效率。

一开始我们采用 GitHub 作为协作平台，用 git 管理翻译项目，用 pull request 来合并翻译。不过由于 git 对非程序员朋友十分不友好，导致翻译《[我们如何才能开发出变革性的思想工具？ ](https://zhuanlan.zhihu.com/p/394795804)》这篇长达 4 万字的文章，花了我们 3 个人将近 2.5 个月的时间。

![](https://pic4.zhimg.com/v2-9259ec25f0e6fd5bad7bd8c2202ab40b_1440w.jpg)
其中还不乏因为 git 比对差异不符预期，导致未翻译内容覆盖了已翻译的内容。

由于这些问题，协作翻译的摩擦太大，使协作带来的收益不足以覆盖协作的成本。最终使得翻译项目停滞。

## 起因

在群里抱怨翻译太慢之后，有群友向我推荐了 Paratranz 这个翻译平台

![](https://pic1.zhimg.com/v2-63dd6554b944b5a82885a648ea88062e_1440w.jpg)
[大家是如何看待有些译员在翻译文章时，先机翻，然后在机翻的基础上进行修改和润色？机翻是否可取呢？](https://www.zhihu.com/question/453300590/answer/1866580713)这个平台的翻译界面功能很全：

![](https://pic1.zhimg.com/v2-1659e2d07b69b36b25f6e06c1a1e62b6_1440w.jpg)
而且还是国人开发的，本来用于翻译 P 社 mod：

[ParaTranz](https://link.zhihu.com/?target=https%3A//paratranz.cn/about) 我一看，感觉针不戳，就开始上手了。

## 创建项目

首先，我们先按流程来，创建一个项目：

![](https://pic2.zhimg.com/v2-61303bf6c2792c52c030398bd637c483_1440w.jpg)

![](https://pic3.zhimg.com/v2-14f3980c63109040b5ded49365402630_1440w.jpg)
这些内容都按要求填写就行了，所属游戏填「其他」，至于选用什么许可协议，可以看这篇介绍：

[漠伦：“知识共享”（CC协议）简单介绍](https://zhuanlan.zhihu.com/p/20641764)建好了项目后，就可以在「我的」标签下面找到自己创建的项目了：

![](https://pica.zhimg.com/v2-47444d5f29d03520015f7f696673438a_1440w.jpg)
## 添加翻译

有了项目，没有待翻译的文本，我们还不能开始翻译。由于这个平台主要是用于翻译游戏 mod 内的描述文本，用来翻译文章属于歪门邪道的用法，这里我们就需要使用平台翻译的通用格式：.csv 文件

![](https://pic2.zhimg.com/v2-72e876f5614f5bff485197d3a0b2cc6b_1440w.jpg)
格式如图所示，第一列为索引值，第二列是原文，第三列是译文。

可能有人要问了，你不是搞翻译协作么？怎么上传的时候就有译文了？

别急，你可能还会问，把要翻译的文章搞成这种格式不累么？

没事儿，这些都可以自动化，为了提高翻译效率，我写了个 Python 脚本来实现格式转换和自动机翻：

import deepl
import os
import time
import pandas as pd
import shutil
from config import deepl_key

translator = deepl.Translator(deepl_key)

def countspaces(x):
    for i, j in enumerate(x):
        if j != &#39; &#39;:
            return i

def translate(file, filename):
    save = pd.DataFrame(columns=[&#39;Source&#39;, &#39;Translation&#39;])
    queries = file.readlines()
    for query in tqdm.tqdm(queries):
        query = query.rstrip()
        if query == &#39;&#39;:
            continue
        space = &#39; &#39; * countspaces(query)
        try:
            result = translator.translate_text(query.lstrip(), target_lang=&#39;ZH&#39;)
            trans = result.text
            if trans == &#39;&#39;:
                trans = query
            time.sleep(0.1)
            save = pd.concat([save, pd.DataFrame({&#39;Source&#39;: [query], &#39;Translation&#39;: [space + trans]})],
                             ignore_index=True)
        except Exception as err:
            print(err)
    save.to_csv(f&#39;./machine_tranz/{filename}.csv&#39;, header=False)

if __name__ == &#39;__main__&#39;:
    trans_files = os.listdir(&#39;source&#39;)

    for filename in trans_files:
        with open(f&#39;source/{filename}&#39;, &#39;r&#39;, encoding=&#34;utf-8&#34;) as f:
            translate(f, filename)
        shutil.move(f&#39;source/{filename}&#39;, f&#39;./src_fin/{filename}&#39;)

思路很简单

先把想要翻译的文章贴到一个文本文件里面
再用 Python 脚本处理，遍历原文的所有段落
然后对每个段落调用 deepL 的翻译接口
将返回的译文和原文一起存到 pandas 的表中
按照平台要求的格式保存成 .csv 文件
上传至 Paratranz 平台

> 注：DeepL 也可以替换为其他机翻接口
这样讲有点抽象，下面拿一篇文章演示一下

### 图片演示

原文：

![](https://pic4.zhimg.com/v2-98bc164e33c7ef30d2adcbeabab5693d_1440w.jpg)
复制到 typora，并保存到待处理文件的目录下

![](https://picx.zhimg.com/v2-f7f0f61cef501f16e85992fcf49c4031_1440w.jpg)
运行脚本：

![](https://pic3.zhimg.com/v2-77c2c87d61a2ca25e6440890115f2570_1440w.jpg)
最终文件：

![](https://pic3.zhimg.com/v2-18ee3ea6578072584c96800caa0e44a6_1440w.jpg)
上传平台：

![](https://pic2.zhimg.com/v2-9e133b86d152b58cf6753ea780a765bb_1440w.jpg)

![](https://picx.zhimg.com/v2-4bc67282e554464b495487100f95655b_1440w.jpg)

![](https://pic1.zhimg.com/v2-40871f3916e9d66ba0c53d0f4c81362c_1440w.jpg)
查看效果：

![](https://pic1.zhimg.com/v2-dde83ce3a730fca7b713d72dd535ec3e_1440w.jpg)
前前后后大概花不到一分钟就能搞定，如果同时添加多个文件，效率还能更高。

## 开始协作

有了待翻译的文本，还有已经预先上传的机器翻译，接下来只需要邀请你的朋友加入项目就能愉快地开始协作了~

具体的协作教程，已经有人写过了，这里就不赘述了：

[ShirahaneSuoh：汉化组翻译指南节选 - 如何使用云平台](https://zhuanlan.zhihu.com/p/376818812)## 发布译文

和朋友们吭哧吭哧翻译了好多文章，现在想把译文整合一下发布出去？总不能一段一段复制出来吧？这里简单说说怎么把翻译好的文章处理成易于阅读的格式。

![](https://pic2.zhimg.com/v2-dcf0de8202b0de408a96485bd921ce37_1440w.jpg)
首先需要到下载这里，将翻译成果下载下来，解压一下，会得到以下文件：

![](https://picx.zhimg.com/v2-33b28834725c0ac72f7ddfc9e247da4d_1440w.jpg)
点开一看，和我们上传的格式一致，除了译文部分已经被更新成我们人工润色和校对后的内容

![](https://pic4.zhimg.com/v2-0844eaee9a95a2a60dd2f67b452ba0ed_1440w.jpg)
这里我也是写了一个脚本自动处理：

import pandas as pd
import os

if __name__ == &#39;__main__&#39;:

    trans_files = os.listdir(&#39;./utf8/tools-for-thought&#39;)

    for filename in trans_files:
        text = pd.read_csv(f&#39;./utf8/tools-for-thought/{filename}&#39;, header=None, index_col=0)
        text.columns = [&#39;source&#39;, &#39;target&#39;]
        text.dropna(inplace=True)
        with open(f&#39;./proofread/{filename[:-4]}&#39;, &#39;w&#39;, encoding=&#39;utf-8&#39;) as f:
            f.writelines(&#39;\n\n&#39;.join(text[&#39;target&#39;].values))

最终处理的结果：

![](https://pic4.zhimg.com/v2-af0c74d44b8c70a3efba862cc62689a3_1440w.jpg)
然后把这个文本贴到任何你想要发布的平台即可！

## 结语

本文初衷就是分享一下自己是如何用 Paratranz 和 DeepL 提高业余翻译效率的，如果能帮到各位，十分荣幸。

用 CAT 的专业翻译请轻喷，谢谢~

叶峻峣

2021 年 9 月 20 日
