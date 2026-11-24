## 前言

GPIO（General Purpose Input Output）通用输入输出口，是 stm32 中非常重要的外设，如果你玩过树莓派，那么对于 GPIO 一定也非常熟悉。树莓派的 GPIO 就是下面红圈圈中的两排引脚。我们可以用线材把 GPIO 连接到灯泡，马达，甚至别的单片机或者嵌入式系统中，实现操作外设，设备通信等等非常炫酷的功能。

<div align=center>
<img src="https://pic1.zhimg.com/80/v2-6a8e54c3d1624c33b5bf8446522f3f19_1440w.png" style="width: 40%;"/>
</div>


而在 stm32 中，也提供了丰富的 GPIO 来供我们使用。

在之前的文章中，我们已经看到了 stm32 的系统结构，如下图所示：

<div align=center>
<img src="https://pic1.zhimg.com/80/v2-a9b0b74a33768e857e881c53b7dd18c1_1440w.png" style="width: 60%;"/>
<p class="img-caption">STM32系统结构</p>
</div>


从图中可以看到，所有的 GPIO（A-G）都是挂载在 APB2 外设总线上的。因此，我们在编写程序时，需要先使能 APB2 外设。

对于每个 GPIO，都有 16 个引脚，编号从 0 到 15。比如对于 GPIOA 而言，它一共有 16 个引脚，也就是 A0 到 A15，你可以在下图中找到这 16 个引脚的编号。

<div align=center>
<img src="https://pica.zhimg.com/80/v2-ec5e30227ba5514000fe0daa58569d2d_1440w.jpeg" style="width: 60%;"/>
</div>




:::info

在 stm32 的参考手册上，还会使用 PA0 代表 GPIOA 的第 0 号引脚，而在stm32的库函数中，则使用宏 `GPIO_Pin_0` 来代表第 0 号引脚。

:::



STM32F103C8T6 芯片拥有**2个**通用目的的输入/输出口（GPIO）组，分别为 `GPIOA` 和 `GPIOB`，每组GPIO端口有**16个**GPIO引脚。因此，STM32F103C8T6 总共有32个GPIO引脚（16个在GPIOA，16个在GPIOB）。

:::info

之前我们操作的 f103 的 GPIOC 属于片上的接口，并不在我们使用的 GPIO 中

:::



下面就通过两个例子LED和蜂鸣器来演示一下如何通过 GPIO 让 stm32 输出信号，从而来控制外设。

---

## LED 流水灯

### GPIO 点亮 LED 灯的基本电路

为了方便讲述，我们此处实验使用GPIOA的0号引脚，也就是 PA0。PA0 驱动 LED 灯一共有两种电路，如下图，分别为低电平驱动（上）和高电平驱动（下）。

<div align=center>
<img src="https://pic1.zhimg.com/80/v2-fbfb5b86bda1494782e519d24b78edb5_1440w.png" style="width: 50%;"/>
</div>


对于低电平驱动电路，当 PA0 输出低电平时（对应引脚写 0），此时电流从右往左，二极管导通，LED灯亮起来。中间的 R1 是限流电阻，用于防止电流过大烧毁LED，也可以用来调整 LED 的亮度；当 PA0 输出高电平时（对应引脚写 1，此时为 3.3V），两边电压相同，没有电流产生，LED熄灭。

高电平电路同理。下面的实验采用低电平驱动。`GPIO_ResetBits` 函数可以用于把对应位置的 Pin 赋值为 0，`GPIO_SetBits` 则可以把对应位置的 Pin 赋值为 1.因此，下面的代码就可以直接点亮低电平驱动的 PA0 连接的 LED：

```c
GPIO_ResetBits(GPIOA, GPIO_Pin_0);
```



具体的线路连接如下图所示

<div align=center>
<img src="https://pica.zhimg.com/80/v2-ec5e30227ba5514000fe0daa58569d2d_1440w.jpeg" style="width: 60%;"/>
</div>




:::info

led 灯长的那一端引脚代表正极，短的代表负极；灯泡内小的那一半代表正极，大的那一半代表负极。

:::



### 点亮 LED 的代码

点亮 LED 灯的代码如下，其余部分复用之前的 [那篇文章](https://kirigaya.cn/blog/article?seq=239) 中的工程：

```c
#include "stm32f10x.h"                  // Device header

int main(void) {
    RCC_APB2PeriphClockCmd(RCC_APB2Periph_GPIOA, ENABLE);
    
    GPIO_InitTypeDef GPIO_initStructure;
    GPIO_initStructure.GPIO_Mode = GPIO_Mode_Out_PP;
    GPIO_initStructure.GPIO_Pin = GPIO_Pin_0;
    GPIO_initStructure.GPIO_Speed = GPIO_Speed_50MHz;
    GPIO_Init(GPIOA, &GPIO_initStructure);

    GPIO_ResetBits(GPIOA, GPIO_Pin_0);
    // 想要熄灭就使用 GPIO_SetBits 将 PA0 置为高电平
 
    while (1) {

    }
}

```

下载后，效果如下，LED灯被点亮。

<div align=center>
<img src="https://picx.zhimg.com/80/v2-0f4d36932cd0643f04f00743a27652aa_1440w.jpeg" style="width: 60%;"/>
</div>



GPIO 还有一个函数 GPIO_WriteBit，说明如下：

```c
// 下面的写法等价于 GPIO_ResetBits(GPIOA, GPIO_Pin_0);
GPIO_WriteBit(GPIOA, GPIO_Pin_0, Bit_RESET);

// 下面的写法等价于 GPIO_SetBits(GPIOA, GPIO_Pin_0);
GPIO_WriteBit(GPIOA, GPIO_Pin_0, Bit_SET);
```



### 流水灯

为了实现流水灯的效果，我们需要一个延时的基本功能，此处我们使用 `SysTick` 实现，具体原理可以不用在意，延时模块 Delay.h 和 Delay.c 如下所示：

```c
// Delay.h
#ifndef __DELAY_H
#define __DELAY_H

void Delay_us(uint32_t us);
void Delay_ms(uint32_t ms);
void Delay_s(uint32_t s);

#endif
```

```c
// Delay.c
#include "stm32f10x.h"

/**
  * @brief  微秒级延时
  * @param  xus 延时时长，范围：0~233015
  * @retval 无
  */
void Delay_us(uint32_t xus)
{
	SysTick->LOAD = 72 * xus;				//设置定时器重装值
	SysTick->VAL = 0x00;					//清空当前计数值
	SysTick->CTRL = 0x00000005;				//设置时钟源为HCLK，启动定时器
	while(!(SysTick->CTRL & 0x00010000));	//等待计数到0
	SysTick->CTRL = 0x00000004;				//关闭定时器
}

/**
  * @brief  毫秒级延时
  * @param  xms 延时时长，范围：0~4294967295
  * @retval 无
  */
void Delay_ms(uint32_t xms)
{
	while(xms--)
	{
		Delay_us(1000);
	}
}
 
/**
  * @brief  秒级延时
  * @param  xs 延时时长，范围：0~4294967295
  * @retval 无
  */
void Delay_s(uint32_t xs)
{
	while(xs--)
	{
		Delay_ms(1000);
	}
} 
```

记得在 Keil5 中把它们加入工程和C++编译选项中。

然后修改代码如下，就可以让单个 LED 灯隔一段时间亮了：

```c
#include "stm32f10x.h"                  // Device header
#include "Delay.h"

int main(void) {
    RCC_APB2PeriphClockCmd(RCC_APB2Periph_GPIOA, ENABLE);
    
    GPIO_InitTypeDef GPIO_initStructure;
    GPIO_initStructure.GPIO_Mode = GPIO_Mode_Out_PP;
    GPIO_initStructure.GPIO_Pin = GPIO_Pin_0;
    GPIO_initStructure.GPIO_Speed = GPIO_Speed_50MHz;
    GPIO_Init(GPIOA, &GPIO_initStructure);
    
    while (1) {
        GPIO_WriteBit(GPIOA, GPIO_Pin_0, Bit_RESET);
        Delay_ms(500);
        GPIO_WriteBit(GPIOA, GPIO_Pin_0, Bit_SET);
        Delay_ms(500);
    }
}

```

此时，PA0 低电平驱动的 LED 就会每隔 500 ms 亮一次了。

想要逐个点亮N盏灯也就很简单了，我们先将额外的四个 LED 连接到 PA1 - PA4，然后写如下的代码：

```c
#include "stm32f10x.h"                  // Device header
#include "Delay.h"

int main(void) {
    RCC_APB2PeriphClockCmd(RCC_APB2Periph_GPIOA, ENABLE);
    
    GPIO_InitTypeDef GPIO_initStructure;
    GPIO_initStructure.GPIO_Mode = GPIO_Mode_Out_PP;
    GPIO_initStructure.GPIO_Pin = GPIO_Pin_0 | GPIO_Pin_1 | GPIO_Pin_2 | GPIO_Pin_3 | GPIO_Pin_4;
    GPIO_initStructure.GPIO_Speed = GPIO_Speed_50MHz;
    GPIO_Init(GPIOA, &GPIO_initStructure);
    
    while (1) {
        // 此处只是演示 GPIO_Write，你也可以用别的函数
        GPIO_Write(GPIOA, ~0x0001);
        Delay_ms(500);
        GPIO_Write(GPIOA, ~0x0002);
        Delay_ms(500);
        GPIO_Write(GPIOA, ~0x0004);
        Delay_ms(500);
        GPIO_Write(GPIOA, ~0x0008);
        Delay_ms(500);
        GPIO_Write(GPIOA, ~0x0010);
        Delay_ms(500);
    }
}

```

下载烧录后，最终效果如下：

<div align=center>
<img src="https://picx.zhimg.com/80/v2-9a7995deb94407e262a54151be5bf54f_1440w.gif" style="width: 60%;"/>
</div>



---

## 有源蜂鸣器

下面我们要驱动有源蜂鸣器，直接用IO口驱动会导致STMB2负担过重，一个常用的做法就是用三极管来进行控制，IO只负责控制三极管的开启和关闭。下图分别是 PNP（S8550） 和 NPN（S8050） 三极管的控制电路。

<div align=center>
<img src="https://picx.zhimg.com/80/v2-0a8d0c4f095b652c68f5e68a5903c7af_1440w.png" style="width: 40%;"/>
</div>

PNP 中，PA0 给低电平，三极管就会导通，此时蜂鸣器工作。PA0 给高电平则停止工作。下图中的 NPN 也是同理。因此接线图如下

<div align=center>
<img src="https://pica.zhimg.com/80/v2-8ab3b3e65186e74788abc24029e52ba6_1440w.jpeg" style="width: 60%;"/>
</div>

这里我们使用 PB12 来控制蜂鸣器。

:::info

但是需要注意，不要用 PA15，PB3 和 PB4 这三个引脚，从 [stm32 引脚定义图](https://blog.csdn.net/eewj/article/details/127333625) 中可以看到，PA15，PB3 和 PB4 是 JTAG 的默认调试端口

:::



程序编写也非常简单了：

```c
#include "stm32f10x.h"
#include "Delay.h"

int main(void) {
    RCC_APB2PeriphClockCmd(RCC_APB2Periph_GPIOB, ENABLE);
    
    GPIO_InitTypeDef GPIO_initStructure;
    GPIO_initStructure.GPIO_Mode = GPIO_Mode_Out_PP;
    GPIO_initStructure.GPIO_Pin = GPIO_Pin_12;
    GPIO_initStructure.GPIO_Speed = GPIO_Speed_50MHz;
    GPIO_Init(GPIOB, &GPIO_initStructure);

    GPIO_WriteBit(GPIOB, GPIO_Pin_12, Bit_RESET);
    
    while (1) {
    }
}

```

接入后，你就能听到蜂鸣器响了。

---

## 参考资料

- [EIDE 开发环境 NC Lab](https://nc-ai-lab.feishu.cn/wiki/Q2tvwh7x9iUJ8Vk27HQceBw2nXy)