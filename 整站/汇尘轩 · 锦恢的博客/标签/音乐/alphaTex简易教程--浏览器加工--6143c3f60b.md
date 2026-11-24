昨天在我的个人网站实现了简单的吉他谱打谱，记录一下alphaTex的用法。

## 和弦
```javascript
\chord <1弦品位> <2弦品位> <3弦品位> <4弦品位> <5弦品位> <6弦品位>
```

如果不需要被拨响，那么就用x来表示。一个标准C和弦可以如下表示：
<div align=center>
<img src="https://pic4.zhimg.com/v2-2e0c0fc1e7a08db2aff2b1bccd927eeb_r.jpg" style="width: 80%;"/>
<p class="img-caption">chord</p>
</div>

## 另起篇章
alphaTex使用一个点.来表示一个音轨的开始。使用\ts x y 来表示拍子和一个自然段的音符数量：

<div align=center>
<img src="https://pic3.zhimg.com/v2-99518cb65b1d50ab95e2900b33391446_r.jpg" style="width: 80%;"/>
<p class="img-caption">节拍</p>
</div>

## 自然段

每个自然段用 | 作为结尾，如果要新起一个自然段，先输入 |：

<div align=center>
<img src="https://pic4.zhimg.com/v2-c576d689d6b069346757c2da7d39c543_r.jpg" style="width: 80%;"/>
<p class="img-caption">自然段</p>
</div>

## 基本音符语法

alphaTex用如下的格式来表示一个音符，对应你弹奏吉他拨响的一个音：

```
品位.弦编号.几分音符
```

其中：

-  弦编号取值1-6
- 几分音符如果不写，渲染器会自动判断你当前这个自然段的各个音符的延长时间。

下面是一个例子

<div align=center>
<img src="https://pic3.zhimg.com/v2-67280b5ec1d7f7c10dd75afc9fd89886_r.jpg" style="width: 80%;"/>
<p class="img-caption">基本音符语法-0</p>
</div>

通过`:8`将后续所有音符申明为8分音符，这样第三个位置就可以不写了：

<div align=center>
<img src="https://pic4.zhimg.com/80/v2-55067eac09712f63899a8ba87d0ede3f_1440w.webp" style="width: 80%;"/>
<p class="img-caption">基本音符语法-1</p>
</div>

## 多重音符
通过括号可以表示同时拨响的音：

<div align=center>
<img src="https://pic1.zhimg.com/v2-beb7d50c284fb423dc841265dd4bc5d8_r.jpg" style="width: 80%;"/>
<p class="img-caption">多重音符</p>
</div>

## 和弦标注

在括号标注的音后紧跟{ch "和弦名称"}可以标注这个多重音符以及后面的一段要按什么和弦进行演奏：

<div align=center>
<img src="https://pic2.zhimg.com/v2-678d31d0aff7ebd00ef2bb8a80e89ce9_r.jpg" style="width: 80%;"/>
<p class="img-caption">其中和弦必须是前面已经申明的了</p>
</div>


## 滑音

使用`{h}`来标注滑音：

<div align=center>
<img src="https://pic4.zhimg.com/v2-7de794a35a461a5fcc9c908792a1f8bb_r.jpg" style="width: 80%;"/>
<p class="img-caption">滑音</p>
</div>

## 休止符

`r`表示休止符：

<div align=center>
<img src="https://pic2.zhimg.com/v2-14fa80f0794ccc050655848ee1e95c8d_r.jpg" style="width: 80%;"/>
<p class="img-caption">休止符</p>
</div>

由于四分休止符常常一起出现，可以通过 * 3来表示连续重复出现的3个休止符：

```javascript
r.4 * 3 0.1.8 2.1.8 |
```

## 重复

使用`\ro`开启一个重复区间，使用`\rc count`收尾，其中`count`是需要重复的次数：

<div align=center>
<img src="https://pic3.zhimg.com/v2-80cb4a1085f390e1e2fcb125cf353506_r.jpg" style="width: 80%;"/>
<p class="img-caption">重复</p>
</div>
