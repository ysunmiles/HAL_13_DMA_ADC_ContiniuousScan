# HAL_13_DMA_ADC_ContiniuousScan

## 简介

本项目基于 STM32F103 系列 MCU 与 STM32 HAL 库，演示 ADC 连续扫描模式下使用 DMA 将多路 ADC 数据持续传输到内存缓冲区的实现。
项目将 ADC1 配置为连续扫描采样多个通道，DMA 在循环模式（Circular）下工作，实现无缝、低 CPU 占用的数据采集。采样结果通过 OLED 实时显示。

## 主要特性

- ADC1 连续扫描（Continuous Scan）多通道采样
- DMA 循环模式（Circular）自动传输 ADC 数据到内存
- 使用 HAL 回调或轮询处理采样数据（可配置）
- OLED 实时显示通道电压或原始 ADC 值，刷新周期可配置
- 基于 STM32CubeMX 生成的工程，使用 CMake 构建

## 关键文件

- `CMakeLists.txt`：构建脚本
- `config.ioc`：CubeMX 配置
- `Core/Src/main.c`：系统与外设初始化，启动 ADC+DMA
- `Core/Src/adc.c` / `Core/Inc/adc.h`：ADC 配置（连续扫描、多通道）
- `Core/Src/dma.c` / `Core/Inc/dma.h`：DMA 初始化与中断配置（环形模式）
- `Core/Src/stm32f1xx_it.c`：中断服务函数
- `Core/Src/OLED.c` / `Core/Inc/OLED.h`：OLED 显示驱动
- `Drivers/`：HAL 库与 CMSIS 文件

## 硬件连接

- MCU：STM32F103 系列
- ADC 通道（示例）：PA0（IN0）、PA1（IN1）、PA3（IN3）、PA6（IN6）
- OLED（软件 I2C）：PB8 (SCL), PB9 (SDA)

## 构建与运行

示例：

```bash
cd d:/Electronics/HAL_Projects/HAL_13_DMA_ADC_ContiniuousScan
cmake --preset Debug
cmake --build --preset Debug
```

使用 ST-Link 烧录固件，复位后 OLED 会显示实时采样数据。

## 注意事项

- 确认 ADC 通道的输入电压范围在 Vref 内（通常 0 ~ Vdda）。
- DMA 缓冲区大小与采样通道数匹配，使用环形模式避免溢出。
- 若使用回调，请在回调中尽量少做阻塞操作，转为标记并在主循环处理。

## 许可

MIT License
