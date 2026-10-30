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
将 PA0、PA1、PA2 配置为 GPIO 推挽输出，在 main.c 的 while(1) 循环中，使用 HAL_GPIO_WritePin() 控制各 LED 状态，基础版使用 HAL_Delay(500) ，加分版使用HAL_GetTick()实现延时。

流水顺序：红灯 → 黄灯 → 绿灯 → 红灯，循环运行，每次切换间隔 500ms。

## 四、实验结果
工程在 CLion 中编译成功，使用 ST-LINK V2 和 STM32CubeProgrammer 烧录程序，三颗 LED 按顺序循环闪烁。

## 五、实验总结
本次作业完成了 STM32 GPIO 输出控制和三灯流水灯实验，熟悉了 CubeMX 配置、CLion 编译及 ST-LINK 烧录流程。

## 六、加分项：基于 HAL_GetTick() 的非阻塞流水灯

### 1. 实现原理

基础版流水灯采用 `HAL_Delay(500)` 实现延时，该函数会阻塞程序执行。

加分项使用 `HAL_GetTick()` 获取系统运行时间，通过比较当前时间与上次切换时间的差值，判断是否经过 500ms。

当时间间隔达到 500ms 时，切换到下一颗 LED；未达到时，主循环继续运行，不需要阻塞等待。

### 2. 核心代码

在 `main.c` 的初始化部分定义计时变量，并点亮第一颗 LED：

```c
uint32_t previousTick = HAL_GetTick();
uint8_t ledIndex = 0;

HAL_GPIO_WritePin(LED0_GPIO_Port, LED0_Pin, GPIO_PIN_RESET);
HAL_GPIO_WritePin(LED1_GPIO_Port, LED1_Pin, GPIO_PIN_RESET);
HAL_GPIO_WritePin(LED2_GPIO_Port, LED2_Pin, GPIO_PIN_RESET);
HAL_GPIO_WritePin(LED0_GPIO_Port, LED0_Pin, GPIO_PIN_SET);
```

在 `while(1)` 循环中使用以下非阻塞控制逻辑：

```c
if ((uint32_t)(HAL_GetTick() - previousTick) >= 500U)
{
    previousTick += 500U;

    HAL_GPIO_WritePin(LED0_GPIO_Port, LED0_Pin, GPIO_PIN_RESET);
    HAL_GPIO_WritePin(LED1_GPIO_Port, LED1_Pin, GPIO_PIN_RESET);
    HAL_GPIO_WritePin(LED2_GPIO_Port, LED2_Pin, GPIO_PIN_RESET);

    ledIndex = (ledIndex + 1U) % 3U;

    if (ledIndex == 0U)
    {
        HAL_GPIO_WritePin(LED0_GPIO_Port, LED0_Pin, GPIO_PIN_SET);
    }
    else if (ledIndex == 1U)
    {
        HAL_GPIO_WritePin(LED1_GPIO_Port, LED1_Pin, GPIO_PIN_SET);
    }
    else
    {
        HAL_GPIO_WritePin(LED2_GPIO_Port, LED2_Pin, GPIO_PIN_SET);
    }
}
```

### 3. 实验结果

程序在 CLion 中编译成功，并通过 ST-LINK V2 和 STM32CubeProgrammer 烧录到 STM32F103C8T6 开发板。

红色、黄色、绿色 LED 按顺序循环点亮，每隔约 500ms 切换一次。

与基础版相比，非阻塞版本不再使用 `HAL_Delay()` 等待时间，可以在主循环中继续执行其他任务。

### 4. 实验总结

通过本次加分项实验，理解了阻塞延时与非阻塞计时的区别，掌握了使用 `HAL_GetTick()` 和时间差判断实现定时任务的方法，为后续多任务嵌入式程序设计打下了基础。
