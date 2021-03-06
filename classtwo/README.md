#汇编语言寄存器（CPU内部工作原理）

####寄存器

>一个典型的CPU由运算器、控制器、寄存器等器件组成，这些器件靠内部总线相连

>内部总线实现CPU内部各个器件之间的联系

>外部总线实现CPU和主板上其它器件的联系

>8086CPU有14个寄存器，它们的名称为:AX、BX、CX、DX、SI、DI、SP、
>BP、IP、CS、SS、DS、ES、PSW

>8086CPU所有的寄存器都是16位的，可以存放两个字节

>AX、BX、CX、DX通常存放一般性数据被称为通用寄存器，譬如寄存器AX是16bit地址是0-15从底地址到高地址16位

>一个16位数据存储一个16位的数据，譬如18转换成二进制10010，存储就是
**0000000000010010**

>通用寄存器为了兼顾上一代CPU,将寄存器分为两个独立的8位寄存器使用

   1. AX分为AH和AL
   1. BX分为BH和AL
   1. CX分为CH和AL
   1. DX分为DH和AL

>分成了高位和低位

####几条汇编指令
![指令](https://raw.github.com/widuu/assembly/master/classtwo/zhiling.png)

####8086CPU给出物理地址的方式
>8086cpu有20位外部地址总线，可传送20位地址，寻址能力是1M

>8086CPU内部是16位结构，它只能传送16位的地址，寻址能力为64K，那么怎么转换成20位呢？

>8086采用两个16位通过地址加法器地址合成一个20位地址的方法--段地址+偏移地址=20bit的物理地址	

>**物理地址=段地址x16+偏移地址**

>**example:**

>123C8H 分为两个16位地址就是 1230H 00C8H 段地址就是1230乘以16变为12300H，便宜地址是00C8H，然后相加12300H+00C8H=123C8H一个16进制数就是4位，5个就是20位的物理地址也就是123C8H

>当然不仅是这个了 还可以划分成 1200H 03C8H 这样划分为段地址和偏移地址

####段寄存器

>段寄存器就是提供段地址的，8086有四个段寄存器：CS、DS、SS、ES

>当8086CPU要访问内存时，由这4个段寄存器提供内存单元的段地址

#####CS和IP

>cs和ip是8086cpu中最关键的寄存器，他们只是了CPU当前要读取指令的地址

 - cs为代码段寄存器
 - ip位指令指针寄存器

>8086PC工作过程的简要描述

- 从CS：IP指向的内存单元读取指令，读取的指令进入指令缓冲器
- IP=IP+所读取指令的长度，从而指向下一条指令；
- 执行指令。转到步骤（1）重复这个过程

>一个简单的例子

 - 在8086CPU通电启动或者重启，CS和IP被设置为CS=FFFFH，IP=0000H
 - 在8086CPU刚启动时，CPU从内存读取FFFF0H段元中读取指令执行
 - FFFF0H单元中的指令是8086PC开机后执行的第一条指令

**JMP 段地址：偏移地址**

 - JMP 2AE3:3--->2AE33的地址

**JMP修改IP地址的内容 JMP某一合法寄存器**
 
 - JMP ax  `MOV ax 200` `JMP ax`
 - JMP bx

>代码段-可以将长度为N（N<=64KB）的一组代码，存在一组地址连续、其实地址为16的倍数的内容单元中，这段内存时用来存放代码的，从而定义一个代码段

***温馨提示：暴力破解就是JMP过注册的地址***