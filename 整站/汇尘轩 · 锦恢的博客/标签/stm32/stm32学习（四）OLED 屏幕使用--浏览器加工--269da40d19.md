## 前言

在我们之前的学习中，如果遇到 bug 了，你会发现无法像 python 或者 C 语言学习那样，用 IO 函数来 输出内容，从而调试，毕竟 stm32 并没有屏幕。为了更方便进行调试，本节我们给 stm32 装一块 OLED 屏幕，这样就可以通过 OLED 打字来展示 stm32 运行的过程，从而 debug 了。

:::info

当然，我一般是这么调试的：先插一个 LED 上去，然后通过之前写的 `LED_ON` 函数来判断 stm32 是否运行了某个函数，是不是在某个函数的地方 hang 住了。不过这样只适用于测试只有两个状态的判断语句，想要执行更加复杂逻辑下的 debug，我们可以通过串口连接到电脑上，或者像本节一样连接一块 OLED 屏幕。

:::

OLED 的定义和特点如下：

- OLED（Organic Light Emitting Diode）：有机发光二极管
- OLED显示屏：性能优异的新型显示屏，具有功耗低（每一个像素点都是单独的发光二极管）、相应速度快、宽视角、轻薄柔韧等特点
- 0.96寸OLED模块：小巧玲珑、占用接口少、简单易用，是电子设计中非常常见的显示屏模块
- 供电：3~5.5V，通信协议：I2C/SPI，分辨率：128*64

下面是四种常见的 0.96 寸 OLED 模块，从左到右分布为：4 引脚 OLED、7 引脚 OLED、蓝色 OLED、第一行固定为黄色下面四行固定为蓝色的 OLED。

<div align=center>
<img src="https://picx.zhimg.com/80/v2-1ae5a4d5d83d00d4d460f575cc8e4eaf_1440w.png" style="width: 70%;"/>
</div>

---

## OLED 引脚定义

4 引脚的 OLE 中 GND 和 VCC 不用多说，各自接入负极和正极即可，SCL 和 SDA 是 I2C 通信的接口，直接接入单片机 I2C 通信引脚上即可。在后面的代码中，我们可以用 stm32 的两个 GPIO 来模拟 I2C 通信，那么 SCL 和 SDA 直接接入 stm32 模拟 I2C 通信的两个 GPIO 上即可。

<div align=center>
<img src="https://picx.zhimg.com/80/v2-2579a7ae752c0d7a165bb8a23d8fa677_1440w.png" style="width: 30%;"/>
</div>

7 引脚也差不多，除了 GND 和 VCC，剩下的 5 个都是 SPI 通信协议的引脚。

<div align=center>
<img src="https://picx.zhimg.com/80/v2-bfd8c75781ca722e37c21974aa7ade15_1440w.png" style="width: 30%;"/>
</div>

本次使用的为 4 引脚版本的 OLED，接线图如下：

<div align=center>
<img src="https://picx.zhimg.com/80/v2-62ccaf910033f589a58564424a28bbbf_1440w.jpeg" style="width: 50%;"/>
</div>

stm32 只需要配置 PB8 和 PB9 即可，PB6 和 PB7 不配置，默认处于浮空状态。

:::info

当然，你不接左边的两根跳线，直接让 PB6 和 PB7 充当电源正负极也是可以的，但是 GPIO 用来进行供电这一行为非常不规范，在实际工程中强烈不推荐。 

:::



---

## OLED 驱动代码

I2C 的东西我懒得在这里写，所以直接使用现成的 OLED 驱动即可。一共三个函数，`OLED.c`，`OLED.h` 和 `OLED_Font.c` ，代码可以在这里找到：https://github.com/LSTM-Kirigaya/stm32f103-template/tree/main/oled/Hardware

在给出的驱动代码中，我们会将 OLED 划分成一个 4 × 16 的矩阵，如下图：

<div align=center>
<img src="https://picx.zhimg.com/80/v2-433428633428092f09a24e81444a777a_1440w.png" style="width: 50%;"/>
</div>

比如 `OLED_ShowChar(1, 1, 'A');` 就是上上图的 1 行 1 列的位置展示字符 `A`；`OLED_ShowString(1, 3, "Hello World!");` 就是在上图的 1 行 3 列的位置展示字符串 `HelloWorld!`。

我们不妨测试一下驱动函数，主函数如下：

```c
#include "stm32f10x.h"
#include "Delay.h"
#include "OLED.h"

int main(void) {
    OLED_Init();

    OLED_ShowChar(1, 1, 'A');
    OLED_ShowString(1, 3, "Hello World!");

    while (1) {

    }
}
```

效果如下：

<div align=center>
<img src="https://picx.zhimg.com/80/v2-bc1dffb31eaea7a944e42b995976cdf7_1440w.jpeg" style="width: 50%;"/>
</div>

为了方便进行嵌入式开发和调试，驱动库还提供了一些函数用于打印不同进制的数字：

```c
#include "stm32f10x.h"
#include "Delay.h"
#include "OLED.h"

int main(void) {
    OLED_Init();

    OLED_ShowChar(1, 1, 'A');
    OLED_ShowString(1, 3, "HelloWorld!");
    OLED_ShowNum(2, 1, 12345, 5);
    OLED_ShowSignedNum(2, 7, -66, 2);
    OLED_ShowHexNum(3, 1, 0xAA55, 4);
    OLED_ShowBinNum(4, 1, 0xAA55, 16);

    while (1) {

    }
}

```

效果如下：

<div align=center>
<img src="https://pic1.zhimg.com/80/v2-ecf3c98364f8f6bc8c6a9e6c687cd226_1440w.jpeg" style="width: 50%;"/>
</div>