---
date: 2016-09-22T18:57:00+08:00
title: 软件控制数码管动态扫描显示
---

[TL;DR]({{<ref "#tl-dr" >}})

数码管一般用来显示的数字，通常由7个笔画构成(有的还有1个小数点)，所以也称为7段数码管。之前构建的[Nios II系统]({{<ref "/post/2016/03/nios.md" >}})就用了6个数码管输出信息。

传统上，要使用数码管，需要译码器将二进制编码的数字转换成7个笔画的编码，例如TTL的7447和CMOS的4511集成电路。

给每个数码管配置一个译码器，并且将每一位数字同时传送到各译码器上，这种方案叫做静态显示。静态显示原理简单，但是浪费硬件资源。

将译码器分时复用则构成动态显示电路：在一个时刻只点亮一个数码管，并使用一个扫描电路轮流扫描各个数码管。当扫描频率足够高时，显示的效果就像是所有数码管同时都被点亮。

<!--more-->

我的开发板上有一个6位的共阳数码管，为了节省引脚个数，元件本身只能使用动态显示的方案（仅给出了7个笔画的引脚`DIG`和一个6位的掩码引脚`SEL`(低电平有效)，共13个引脚；如果采用静态显示，则需要8×6=48个引脚）。

要实现动态显示，一种办法是设计硬件扫描电路。

```
module seg7ctl(
	input CLK,
	input [47:0] D,
	output reg [5:0] SEL,
	output reg [7:0] SEG
);

initial
	SEL = 6'b111110;

always
	case (SEL)
		6'b111110:
			SEG = D[7:0];
		6'b111101:
			SEG = D[15:8];
		6'b111011:
			SEG = D[23:16];
		6'b110111:
			SEG = D[31:24];
		6'b101111:
			SEG = D[39:32];
		6'b011111:
			SEG = D[47:40];
		default:
			SEG = 8'bxxxxxxxx;
	endcase

always @(posedge CLK)
	SEL = {SEL[4:0], SEL[5]};

endmodule
```

这个模块相当于一个把动态显示数码管转换成静态显示数码管的适配器。为了显示一些特殊的内容，我没有使用译码器，而是直接把数码管的8×6=48位原始内容暴露给CPU。因为超过了Nios II的总线宽度，我用两个PIO模块`seg_pio_hi`和`seg_pio_lo`进行通信。

```
void display(const char s[6])
{
	alt_u8 lo[4] = {s[5], s[4], s[3], s[2]};
	alt_u8 hi[4] = {s[1], s[0]};

	IOWR_ALTERA_AVALON_PIO_DATA(SEG_PIO_HI_BASE, *(alt_u32 *) hi);
	IOWR_ALTERA_AVALON_PIO_DATA(SEG_PIO_LO_BASE, *(alt_u32 *) lo);
}
```

这样，调用

```
display("\x89\x86\xc7\xc7\xa3\x7d");
```

就可以显示出类似`HeLLo!`的字样。

硬件电路可定制性高，但是消耗硬件资源：48位的PIO需要48个寄存器，扫描电路需要6个寄存器，此外还需要一个单独的硬件时钟。

## TL;DR

使用软件定时器也可以实现动态扫描。当Nios II系统中含有系统时钟时，可以添加定时器，使得某个回调函数函数每隔一段时间被调用一次。我可以在回调函数中直接向`SEG`和`DIG`引脚写入数据，从而无需额外的硬件电路。

创建一个14位宽的PIO模块`pio_disp`，它的低8位连接到`DIG`引脚，高6位连接到`SEL`引脚。在软件中定义一个6字节的缓冲区`char DISPLAY_BUFFER[6];`。回调函数可以这么写：
```
alt_u32 on_alarm(void *context)
{
	static int i = 0;
	alt_u8 data[4] = {DISPLAY_BUFFER[i], 0x7DF >> i};
	IOWR_ALTERA_AVALON_PIO_DATA(PIO_DISP_BASE, *(alt_u32 *) data);
	i = (i == 5) ? 0 : i + 1;
	return (alt_u32) context;
}
```

在`main()`函数的开始部分调用初始化函数`display_init()`，其内容为：

```
int display_init(alt_u32 nticks)
{
	static alt_alarm the_alarm;
	return alt_alarm_start(&the_alarm, 0, &on_alarm, (void *) nticks);
}
```

这样定时器会每个固定的时间被调用一次，数码管的内容被更新。要改变显示的内容，只需更新缓冲区的内容即可。

```
memcpy(DISPLAY_BUFFER, "xxxxxx", 6);
```

在软件的实现中，PIO模块仍然需要14个寄存器，没有专用的适配器电路。数码管的48位信息保存在`DISPLAY_BUFFER`中，实质上是在RAM里面，而不是48个专用的寄存器。

系统时钟可能还需要一些硬件资源，但是系统时钟在其他很多场合也很有用。

总体来说，硬件和软件的实现方式各有优劣。在这个例子中软件实现并不比硬件实现存储空间。但是软件更多使用RAM而不是寄存器。我的FPGA芯片中，寄存器只有约6000个，但是RAM有270000多比特。宝贵的硬件资源最好还是用来做高性能计算或者硬件接口，一般的逻辑用软件实现应该已经足够。
