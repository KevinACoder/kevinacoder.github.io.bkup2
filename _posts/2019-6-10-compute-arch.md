---
layout: post
title: Linux ARM嵌入式系统笔记
---

# 嵌入式系统(Embedded System)

嵌入式系统可以理解为为特定设备服务的软件，硬件可裁剪的计算机系统，ARM(Advanced RISC Machines)嵌入式系统是其中有代表性的体系结构。

嵌入式系统有软件和硬件部分，硬件包括嵌入式微处理器，外部设备，软件包括嵌入式操作系统和应用软件。
操作系统可以通过驱动程序操作外部设备。

## 嵌入式微控制器 vs 嵌入式微处理器

嵌入式微控制器，或者称为单片机，处理能力低，只能用在相对简单的控制领域，但是体积小，很难拥有操作系统，设计较简单。

嵌入式微处理器，将外部设备控制器集成到芯片内部，它可以拥有操作系统。

## 嵌入式系统组成

和其他计算机系统一样，嵌入式系统由指令系统，主要是中央处理器(CPU)，存储系统，总线系统和输入输出系统(I/O)。

## 计算机体系结构

指令系统，存储系统和I/O系统是计算机的主要组成系统。从存储系统的角度，流行的系统结构设计方法包括冯诺依曼结构和哈佛结构。

##### --冯诺依曼结构

运算器，控制器，存储器，输入设备和输出设备5部分。工作时，指令存储在存储器中，从中取出指令，由运算器执行指令，控制器处理输入输出。

##### --哈佛结构

冯诺依曼结构在运算器取指令时，不能同时取数据，导致效率降低。哈佛结构将指令和数据分开存储，使用不同的数据宽度。ARM处理器采用哈佛结构。

##### --CPU

组成：运算器、控制器、寄存器和内部总线
关键参数：工作频率、字长、指令集和缓存大小
    . 工作频率，主频、外频和外部总线频率
    . 字长，一个CPU周期内能处理的数据宽度，32bit
    . 指令集，复杂指令集(CISC)和精简指令集(RISC)
    . 缓存，暂存数据和指令，CPU和外部设备的处理速度差别

##### --存储系统

硬盘/Flash -> 内存RAM -> 高速缓存Cache -> CPU寄存器

##### --总线系统

总线标准包括$I^2C$，SPI和PCI等。总线种类包括，
. DataBus, 从外部设备读写数据
. AddressBus, 向外部设备发送地址
. CtrlBus，向外部设备发送控制信息

##### --输入输出系统

CPU访问外部设备的方式，
. 轮询，影响指令执行，效率低
. 中断控制，响应中断信号，效率高
. DMA(Direct Memory Access)，适合大量数据传输

# ARM处理器与Raspberry Pi

## ARM体系结构命名

```
pi@raspberrypi:~ $ uname -a
Linux raspberrypi 4.14.98-v7+ #1200 SMP Tue Feb 12 20:27:48 GMT 2019 armv7l GNU/Linux
```

## 开发环境构建

##### --安装/卸载软件

更新apt软件源
```shell
sudo apt-get update
sudo apt-get upgrade -y
```
安装主要打包工具
```shell
sudo apt-get install build-essential
gcc --version
gdb --version
```

## shell指令

