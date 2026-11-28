## 前言

这一章主要讲讲 GPIO 的输入和对应的驱动代码如何编写。

项目基本结构

```
📁User
📁System
📁Start
📁Library
📁Hardware
```

---

## 例子1：按键控制 LED

按钮一共有四种接入方式，注意，在下面的四个图中，PA0 作为输入端使用。

<div align=center>
<img src="https://pic1.zhimg.com/80/v2-26d86589c4aa06e40482a0e140c06f76_1440w.png" style="width: 70%;"/>
</div>

简单说，上面两种接法，按键按下去是PA0输入低电平，松手是高电平，此时的 PA0 需要配置为上拉输入模式 `GPIO_Mode_IPU`。下面两种接法则刚好相反，此时 PA0 需要配置为下拉输入模式 `GPIO_Mode_IPD`。本次实验采用左下角的接法。并且将两盏 LED 接到 PA0 和 PA1 上，两个按钮接到 PB11 和 PB1 上。最终接法如下：

<div align=center>
<img src="https://picx.zhimg.com/80/v2-dd23b1319296a06c9a4ba51f45e7553e_1440w.png" style="width: 60%;"/>
</div>



LED 为低电平接入法，按钮为高电平为高电平接入法。即，低电平点亮 LED，按下按钮输入高电平。

:::info

我们采用的这种按钮，按下去时和松手时会产生一定的抖动，从而让 PA0 接受的信号不稳定，为了消除这种不稳定，我们需要在检测到按钮按下的时候和松开的时候给系统做个 20 ms 的延时。用来避开抖动带来的影响。

:::



为了方便进行管理，我们可以把和硬件相关的驱动代码写进 `Hardware` 中进行封装。

### LED 驱动代码

我们先封装 LED 的驱动代码，在 `Hardware` 中创建 `LED.h` 和 `LED.c`：

```c
// LED.h

#ifndef __LED_H
#define __LED_H

void LED_Init();
void LED_ON(uint16_t GPIO_Pin);
void LED_OFF(uint16_t GPIO_Pin);

#endif
```

```c
// LED.c

#include "stm32f10x.h"

// 使能 PA0 和 PA1，设置为推挽输出模式，完成 LED 驱动的初始化
void LED_Init() {
    RCC_APB2PeriphClockCmd(RCC_APB2Periph_GPIOA, ENABLE);
    GPIO_InitTypeDef GPIO_InitStructure;
    GPIO_InitStructure.GPIO_Mode = GPIO_Mode_Out_PP;
    GPIO_InitStructure.GPIO_Pin = GPIO_Pin_0 | GPIO_Pin_1;
    GPIO_InitStructure.GPIO_Speed = GPIO_Speed_50MHz;

    GPIO_Init(GPIOA, &GPIO_InitStructure);

    // 默认熄灭 LED
    GPIO_WriteBit(GPIOA, GPIO_Pin_0 | GPIO_Pin_1, Bit_SET);   
}

// 打开对应端口的 LED 灯
void LED_ON(uint16_t GPIO_Pin) {
    GPIO_ResetBits(GPIOA, GPIO_Pin);
}

// 熄灭对应端口的 LED 灯
void LED_OFF(uint16_t GPIO_Pin) {
    GPIO_SetBits(GPIOA, GPIO_Pin);
}
```

这样，我们直接调用 `LED_ON` 和 `LED_OFF` 就能直接开启/关闭指定地方的 LED 灯光了。

### 按键驱动代码

然后，我们需要为按钮提供驱动，我们写成 `KEY.c` 和 `KEY.h`，我们需要实现一个 `Key_GetNum` 函数，用于返回第几个按钮被按下了，如果没有按钮按下，就返回 0. 代码如下：

```c
// KEY.h

#ifndef __KEY_H
#define __KEY_H

void KEY_Init();
uint8_t Key_GetNum();

#endif
```

```c
// KEY.c

#include "stm32f10x.h"
#include "Delay.h"

void KEY_Init() {
    RCC_APB2PeriphClockCmd(RCC_APB2Periph_GPIOB, ENABLE);
    
    GPIO_InitTypeDef GPIO_InitStructure;
    GPIO_InitStructure.GPIO_Mode = GPIO_Mode_IPD;
    GPIO_InitStructure.GPIO_Pin = GPIO_Pin_0 | GPIO_Pin_10;
    GPIO_InitStructure.GPIO_Speed = GPIO_Speed_50MHz;

    GPIO_Init(GPIOB, &GPIO_InitStructure);
}

uint8_t Key_GetNum() {
    uint8_t KeyNum = 0;
    if (GPIO_ReadInputDataBit(GPIOB, GPIO_Pin_0) > 0) {
        // 刚刚按下按键，会发生一个20ms的抖动，通过 delay 消除抖动
        Delay_ms(20);
        // 直到松手
        while (GPIO_ReadInputDataBit(GPIOB, GPIO_Pin_0) > 0);
        // 松手后也会产生一段抖动，通过 delay 消除
        Delay_ms(20);
        KeyNum = 1;
    }

    if (GPIO_ReadInputDataBit(GPIOB, GPIO_Pin_10) > 0) {
        // 刚刚按下按键，会发生一个20ms的抖动，通过 delay 消除抖动
        Delay_ms(20);
        // 直到松手
        while (GPIO_ReadInputDataBit(GPIOB, GPIO_Pin_10) > 0);
        // 松手后也会产生一段抖动，通过 delay 消除
        Delay_ms(20);
        KeyNum = 2;
    }

    return KeyNum;
}
```



### 用两个按钮控制一盏灯

然后我们可以用两个按钮控制一盏灯，一个按钮按下就亮，另一个按下就熄灭，这个代码非常简单：

```c
#include "stm32f10x.h"
#include "Delay.h"
#include "LED.h"
#include "KEY.h"

uint8_t KeyNum;

int main(void) {
    LED_Init();
    KEY_Init();
    
    while (1) {
        KeyNum = Key_GetNum();
        
        if (KeyNum == 1) {
            LED_ON(GPIO_Pin_0);
        }
        if (KeyNum == 2) {
            LED_OFF(GPIO_Pin_0);
        }
    }
}
```



### 用一个按钮控制一盏灯

但是这么做非常不优雅，一个按钮明明就可以控制一盏灯，一个最简单的做法就是设置一个变量，用来维护每一个灯的状态。但是在 stm32 中，还有一个更加优雅的方法，那就是通过 `GPIO_ReadOutputDataBit` 来直接读取某一个输出状态的 GPIO 当前的输出寄存器中的取值。

从下图的 GPIO 电路图中也能看出， GPIO 的【输出数据寄存器】不仅可以写，还可以读，`GPIO_ReadOutputDataBit` 就是用于读的函数。

<div align=center>
<img src="https://pic1.zhimg.com/80/v2-ceae5278e5e8b2bf87375951627cc9be_1440w.png" style="width: 70%;"/>
</div>

我们只需要往 LED 的驱动代码中加入一个函数即可：

```c
void LED_TURN(uint16_t GPIO_Pin) {
    if (GPIO_ReadOutputDataBit(GPIOA, GPIO_Pin) == 0) {
        GPIO_SetBits(GPIOA, GPIO_Pin);
    } else {
        GPIO_ResetBits(GPIOA, GPIO_Pin);
    }
}
```

然后主函数修改为：

```c
#include "stm32f10x.h"
#include "Delay.h"
#include "LED.h"
#include "KEY.h"

uint8_t KeyNum;

int main(void) {
    LED_Init();
    KEY_Init();
    
    while (1) {
        KeyNum = Key_GetNum();
        
        if (KeyNum == 1) {
            LED_TURN(GPIO_Pin_0);
        }
        if (KeyNum == 2) {
            LED_TURN(GPIO_Pin_1);
        }
    }
}
```

这样，PB10 上的按钮就可以控制 PA1 上的灯，PB0 上的按钮就可以控制 PA0 上的灯了。

---

## 例子2：光敏传感器控制有源蜂鸣器

在了解了 GPIO 基本的输入模式的情况下，接入其他的传感器进行控制也就大同小异了。此处我们使用光敏传感器来控制有源蜂鸣器，下面是基本的接线图。

<div align=center>
<img src="https://pic1.zhimg.com/80/v2-c584f37cf6fd3ded94f7b96fbf74b661_1440w.jpeg" style="width: 70%;"/>
</div>

和之前一样，我们来为蜂鸣器与光敏传感器来编写对应的驱动。

### 蜂鸣器驱动



```c
// Buzzer.h

#ifndef __BUZZER_H
#define __BUZZER_H

void Buzzer_Init();
void Buzzer_ON();
void Buzzer_OFF();

#endif
```

```c
// Buzzer.c

#include "stm32f10x.h"

void Buzzer_Init() {
    RCC_APB2PeriphClockCmd(RCC_APB2Periph_GPIOB, ENABLE);

    GPIO_InitTypeDef GPIO_InitStructure;
    GPIO_InitStructure.GPIO_Mode = GPIO_Mode_Out_PP;
    GPIO_InitStructure.GPIO_Pin = GPIO_Pin_12;
    GPIO_InitStructure.GPIO_Speed = GPIO_Speed_50MHz;

    GPIO_Init(GPIOB, &GPIO_InitStructure);

    // 默认关闭
    GPIO_WriteBit(GPIOB, GPIO_Pin_12, Bit_SET);
}

void Buzzer_ON() {
    GPIO_WriteBit(GPIOB, GPIO_Pin_12, Bit_RESET);
}

void Buzzer_OFF() {
    GPIO_WriteBit(GPIOB, GPIO_Pin_12, Bit_SET);
}
```



### 光敏电阻驱动

光敏电阻可以使用上拉输入，这是因为当光线充足时，光敏电阻的 DO 连接的 LM393 比较器会输出一个比较低的电平，此时需要上拉。

当然，因为光敏电阻内部 DO 引脚接到了一个 10k 的上拉电阻，所以直接使用浮空输入也是没问题的。

```c
// LightSensor.h

#ifndef __LIGHTSENSOR_H
#define __LIGHTSENSOR_H

void LightSensor_Init();
uint8_t LightSensor_GET();

#endif
```

```c
// LightSensor.c

#include "stm32f10x.h";

void LightSensor_Init() {
    RCC_APB2PeriphClockCmd(RCC_APB2Periph_GPIOB, ENABLE);

    GPIO_InitTypeDef GPIO_InitStructure;
    GPIO_InitStructure.GPIO_Mode = GPIO_Mode_IPU;
    GPIO_InitStructure.GPIO_Pin = GPIO_Pin_13;
    GPIO_InitStructure.GPIO_Speed = GPIO_Speed_50MHz;

    GPIO_Init(GPIOB, &GPIO_InitStructure);
}

uint8_t LightSensor_GET() {
    return GPIO_ReadInputDataBit(GPIOB, GPIO_Pin_13);
}
```



### 主函数

主函数编写就非常简单了：

```c
#include "stm32f10x.h"
#include "Delay.h"
#include "Buzzer.h"
#include "LightSensor.h"


int main(void) {
    Buzzer_Init();
    
    while (1) {
        // 此时光线比较暗，鸣响蜂鸣器
        if (LightSensor_GET() == 1) {
            Buzzer_ON();
        } else {
            Buzzer_OFF();
        }
    }
}

```

编译下载后，只需要遮住光敏传感器，蜂鸣器就会响，松开手，它就不会响。

