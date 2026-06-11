# HAL_12_DMA_ADC_ConvCpltIT

## 简介

本项目基于 STM32F103 系列 MCU 和 STM32 HAL 库，演示了使用 **DMA 和 ADC 转换完成中断** 进行多通道模拟采样的应用。
项目配置 ADC1 扫描模式采集 4 个通道的数据，通过 DMA 自动传输结果到内存缓冲区，支持中断驱动数据处理。
采集结果通过 OLED 屏幕实时显示。项目采用 STM32CubeMX 生成的 CMake 构建配置。

## 主要功能

- 初始化 STM32F103 HAL 外设、时钟和 GPIO
- 配置 ADC1 多通道扫描转换模式（4 个模拟输入通道）
- 启动 ADC 校准，配置 DMA 自动数据传输
- 使用 `HAL_ADC_Start_DMA()` 启动 ADC+DMA 协作采样
- 采集 ADC_PA0（通道 0）、ADC_PA1（通道 1）、ADC_PA3（通道 3）、ADC_PA6（通道 6）
- 将实时采样结果显示在 OLED 屏幕上，更新周期 500ms

## 关键文件

- `CMakeLists.txt`：项目根 CMake 构建脚本
- `CMakePresets.json`：配置和构建预设，支持 `Debug` 和 `Release`
- `config.ioc`：STM32CubeMX 项目配置
- `Core/Src/main.c`：主程序入口，初始化 OLED、ADC、DMA，启动转换并在主循环中更新显示
- `Core/Src/adc.c`：ADC1 初始化与多通道配置
- `Core/Inc/adc.h`：ADC 模块接口声明
- `Core/Src/dma.c`：DMA 控制器初始化与中断配置
- `Core/Inc/dma.h`：DMA 模块接口声明
- `Core/Src/stm32f1xx_it.c`：中断服务程序（DMA、ADC 转换完成）
- `Core/Src/OLED.c`：OLED 控制逻辑与软件 I2C 实现
- `Core/Inc/OLED.h`：OLED 功能接口声明
- `Core/Inc/OLED_Font.h`：OLED 字库数据
- `cmake/user_sources.cmake`：自定义源文件和 include 路径注册
- `Drivers/`：STM32 HAL 库源文件和 CMSIS 头文件

## 构建环境与依赖

- CMake 3.22 及以上
- Ninja 构建器
- ARM GCC 交叉编译器（例如 `arm-none-eabi-gcc`）
- STM32 HAL 库（包含在 `Drivers/` 目录中）

## 构建步骤

推荐使用 VS Code 的 CMake 工具或命令行：

```bash
cd d:/Electronics/HAL_Projects/HAL_12_DMA_ADC_ConvCpltIT
cmake --preset Debug
cmake --build --preset Debug
```

## 运行与下载

1. 生成固件之后，使用 ST-Link 或其他支持的下载工具烧录生成的 ELF/HEX/BIN 文件到目标板。
2. 重新上电后，OLED 屏幕会显示各 ADC 通道的采样值，每 500ms 更新一次。

## 技术特点

- **DMA 自动数据传输**：无需 CPU 干预，DMA 自动将 ADC 转换结果传输到内存
- **中断驱动**：ADC 转换完成后触发中断，用于状态管理和数据处理
- **扫描模式**：单次转换周期完成所有 4 个通道的采样
- **软件触发**：每个转换周期由软件或 DMA 完成中断自动触发下一轮

## 硬件说明

- 目标 MCU：STM32F103 系列
- ADC1 多通道输入：
  - `ADC1_IN0`：PA0
  - `ADC1_IN1`：PA1
  - `ADC1_IN3`：PA3
  - `ADC1_IN6`：PA6
- OLED 软件 I2C 默认引脚：
  - `PB8`：SCL
  - `PB9`：SDA

## 许可

MIT License
