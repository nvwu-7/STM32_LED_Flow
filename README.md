# 电控组第二次培训作业

 - **姓名：石婉怡** 

 - ***学号：20263005086*** 

## 一、实验目的
使用 STM32F103C8T6 开发板，通过 STM32CubeMX 配置 GPIO，使用 CLion 编写程序，实现三个 LED 流水灯控制。

## 二、硬件连接
- LED0（红色）：PA0，330Ω 限流电阻
- LED1（黄色）：PA1，330Ω 限流电阻
- LED2（绿色）：PA2，330Ω 限流电阻
- 三个 LED 负极分别连接 GND。

## 三、实现思路
将 PA0、PA1、PA2 配置为 GPIO 推挽输出，在 main.c 的 while(1) 循环中，使用 HAL_GPIO_WritePin() 控制各 LED 状态，并使用 HAL_Delay(500) 实现延时。

流水顺序：红灯 → 黄灯 → 绿灯 → 红灯，循环运行，每次切换间隔 500ms。

## 四、实验结果
工程在 CLion 中编译成功，使用 ST-LINK V2 和 STM32CubeProgrammer 烧录程序，三颗 LED 按顺序循环闪烁。

## 五、实验总结
本次作业完成了 STM32 GPIO 输出控制和三灯流水灯实验，熟悉了 CubeMX 配置、CLion 编译及 ST-LINK 烧录流程。