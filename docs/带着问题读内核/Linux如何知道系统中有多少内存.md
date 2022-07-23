<!-- MDTOC maxdepth:6 firsth1:1 numbering:0 flatten:0 bullets:1 updateOnSave:1 -->

- [Linux如何知道系统中有多少内存](#linux如何知道系统中有多少内存)   
   - [开题](#开题)   
   - [ARM Linux内存信息获取方式](#arm-linux内存信息获取方式)   

<!-- /MDTOC -->
# Linux如何知道系统中有多少内存

## 开题

首先，linux自举，内容也是相当丰富。当逃不脱的还是指令数据，万变归宗，

```
加载指令/数据（混合体）到内存，PC指令赋值到起点指令，无休无尽的运行
```

![20220716_093707_10](image/20220716_093707_10.png)

就像四驱车一样，开电源（开机），放赛道上（PC指令复制到指定位置），赛道上疾驰（马不停蹄运行），除你按关机键（断电）

回到标题所指，运行时如何检测内存？这里猜测一下，其实可以有很多方案。所有你想的，在合理范围都是可以软硬件实现。

* BIOS(uboot一样)准备好内存信息，提供接口，内核自举阶段调用接口，直接获取
* 内核通过硬件接口，自行探测，前提，硬件需要暴露接口

一个是吃软饭，一个硬饭软吃。无论何种方案，少不了一个东西，接口。软件与硬件的交互，从来都是协议先行。相互指定规则，软件通过相应规则，去获取信息即可。

常规硬件信息，多是通过IO指令映射地址空间，通过一系列控制寄存器、配置寄存器、数据寄存器来间接操作硬件。

以嵌入式最简单的GPIO为例，也得安排一下IO地址，然后赋值使用。不管你是直接IO指令操作，还是映射到虚拟地址空间，还是搞成文件读写。所到底，底层，一个套路，读写内存而已。


## ARM Linux内存信息获取方式

* ARM比较懒，需要人直接将信息喂到嘴里。那就是DTS（Device Tree Source）
* 起初，嵌入式设备多如牛毛，设备类型多样，接口多样，内核为此合入超多混杂的设备代码，因此被linus吐槽。不可能稍微一个设备改改，一个产品改改，整个内核代码都跟着动，做法愚蠢，必须整改
* 既然设备多入牛毛，那就简单粗暴，直接喂内核数据，不要让内核自己探测。这类探测代码实际意义不大？虽然每设备探测都有多如牛毛的细则，再者每次探测都是有一定耗时，尤其是兼容性而言。让内核来，不如让硬件厂商相互协定，这里，硬件厂商毫无疑问，```主板<->外设``` 之间，各类新型接口协议的定制，都需要时间、人力开销。
*




## ARM Vexpress简介

**Arm Versatile Express boards(Vexpress)**

* <https://qemu-project.gitlab.io/qemu/system/arm/vexpress.html>

```
QEMU models two variants of the Arm Versatile Express development board family:

vexpress-a9 models the combination of the Versatile Express motherboard and the CoreTile Express A9x4 daughterboard

vexpress-a15 models the combination of the Versatile Express motherboard and the CoreTile Express A15x2 daughterboard
```

* ARM Versatile Express开发板（简称VE板）是ARM公司推出的，提供给厂商评估ARM内核的高效的软硬件平台。在中国地区，各厂商可以通过米尔科技获得ARM授权的正版ARM Versatile Express开发板。

* <http://bbs.myir-tech.com/thread-6248-1-1.html>


![20220723_112708_52](image/20220723_112708_52.png)

![20220723_112713_54](image/20220723_112713_54.png)

![20220723_112717_70](image/20220723_112717_70.png)

![20220723_112722_85](image/20220723_112722_85.png)









---
