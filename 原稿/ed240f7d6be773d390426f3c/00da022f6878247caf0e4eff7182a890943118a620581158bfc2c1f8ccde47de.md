完整的 VCD 语法，可以参考中科大资源网站上的 [IEEE 1364 标准](http://staff.ustc.edu.cn/~songch/download/IEEE.1364-2005.pdf) 第 18 章的内容。



## 基本信息

一个 vcd 文件会描述波形产生的基本信息，包括，日期，版本，单位时间，注释等等。

### 日期 date

```
$date
	Sat Apr 20 20:06:14 2024
$end
```

与 verilog 类似，`$end$` 是一切 scope 的结束符号，你也可以写成这样：

```
$date Sat Apr 20 20:06:14 2024 $end
```



### 版本 version

```
$version
	Icarus Verilog
$end
```



### 时间单位 timescale

```
$timescale
	1ns
$end
```



### 注释 comment

```
$comment
	Show the parameter values.
$end
```

---

## 变量申明

vcd 中通过如下的方式申明一个 module 内的变量，，每条的格式为 `$var type bitwidth id name`，比如：

```
$scope module ID_EX $end
$var wire 2 ! AluOp [1:0] $end
$var wire 1 " AluSrc $end
$var wire 1 # MemRead $end
$upscope $end
```

以上语句申明了一个模块 `ID_EX` 和内部的三个信号 `AluOp`，`AluSrc` 和 `MemRead`。这三个变量在接下来的 vcd 描述中会被 `!`，`"`，`#` 替代，这也被称为 vcd 描述中，变量的 id.

- 单个 module 的变量申明通过 `$upscope $end` 结束。
- 所有 module 的变量申明通过 `$enddefinitions $end` 结束。



## parameter 赋值

对于一些特殊的变量，比如 `parameter`，它们的值一般是不会被改变的。在 vcd 中，它们会通过 `$dumpall` 被赋值。每个赋值条目的格式为 `二进制数值 id`，比如：

```
...
$var parameter 32 * CAL_WIDTH $end
...
$dumpall
b100 *
$end
...
```

上述代码表明 `CAL_WIDTH` 被赋予的值是 `b100` 也就是 4.

对于位宽为 0 的变量，它的申明格式更加简单，为 `0id` 或者 `1id`。

gtkwave 是支持 `parameter` 的渲染的，所以我们也需要支持：

<div align=center>
<img src="https://pic1.zhimg.com/80/v2-f1e856530685fef9564b135dab947d2b_1440w.png" style="width: 80%;"/>
</div>

具体而言，需要在 LL 文法 中定义这部分的解析逻辑。



## 信号的赋值

信号的赋值与 parameter 类似，但是信号还需要记录每一个值所处的时间戳，所以格式与 parameter 相比，稍有不同。信号赋值往往通过 `$dumpvars`，而且 vcd 标准强制要求记录数值的开头为 0 时刻，所以记录信号值的例子如下：

```
#0
$dumpvars
b0 !

#100
b01 !

#200
b11 !

#300
b0 !
```

上面的例子的意思是，id 为 `!` 的信号（也就是 AluOp）在 0 时刻为 `b0`，在 100 时刻为 `b01`，在 200 时刻为 `b11`，在 300 时刻为 `b0`。

> 时间戳总是升序的

---

## 参考资料

- https://en.wikipedia.org/wiki/Value_change_dump