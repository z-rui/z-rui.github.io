---
date: 2022-09-10T00:13:37-07:00
title: 8位微处理器实践(1) - 彩色LED控制器
---

这一篇研究怎样使用PIC控制彩色LED。

彩色LED可以使用4引脚的[RGB共阴发光二极管](https://www.sparkfun.com/products/105)。
阴极接Vss，RGB三个阳极经过限流电阻分别接三个GPIO引脚。PIC的GPIO可以输出25mA电流，足够直接驱动LED，无需额外的晶体管。

<!--more-->

我希望实现色彩循环渐变的效果，因此我把循环周期等分为三个相位，对于某一种颜色：

1. 亮度从0增加到最大
2. 亮度从最大减小到0
3. 亮度保持为0

红、绿、蓝三个颜色各错开一个相位，这样就可以产生各种颜色的组合。

{{<figure src="/media/pic-1-2.png" alt="相位图">}}

LED亮度的控制，通常采用脉冲宽度调制（PWM）的办法：在很短的周期内，输出一段高电平（亮）和低电平（灭）。根据高电平所占时间（占空比）不同，人眼观察到的是不同的亮度。

这个项目我使用一片[PIC16F15214](https://www.microchip.com/en-us/product/PIC16F15214)。它有8个引脚，我使用其中3个作为输出。它有4个PWM外设，因此PWM的控制可以完全由硬件完成。

{{<figure src="/media/pic-1-1.png" alt="原理图">}}

这个项目比较简单，可以用PIC汇编实现。在MPLAB X中，将编译器设为pic-as，并创建一个主文件`main.S`。

## 配置字

在汇编文件的最开头，应当设置PIC的配置字 (Configuration words)。MPLAB X提供了一个图形界面 (Production&rarr;Set Configuration Bits)，当编译器是pic-as时，可以生成相应的汇编代码。当编译器是XC8时，则生成C代码。

```asm
; PIC16F15214 Configuration Bit Settings

; Assembly source line config statements

; CONFIG1
  CONFIG  FEXTOSC = OFF         ; External Oscillator Mode Selection bits (Oscillator not enabled)
  CONFIG  RSTOSC = HFINTOSC_1MHZ; Power-up Default Value for COSC bits (HFINTOSC (1 MHz))
  CONFIG  CLKOUTEN = OFF        ; Clock Out Enable bit (CLKOUT function is disabled; I/O function on RA4)
  CONFIG  VDDAR = HI            ; VDD Range Analog Calibration Selection bit (Internal analog systems are calibrated for operation between VDD = 2.3V - 5.5V)

; CONFIG2
  CONFIG  MCLRE = EXTMCLR       ; Master Clear Enable bit (If LVP = 0, MCLR pin is MCLR; If LVP = 1, RA3 pin function is MCLR)
  CONFIG  PWRTS = PWRT_OFF      ; Power-up Timer Selection bits (PWRT is disabled)
  CONFIG  WDTE = OFF            ; WDT Operating Mode bits (WDT disabled; SEN is ignored)
  CONFIG  BOREN = ON            ; Brown-out Reset Enable bits (Brown-out Reset Enabled, SBOREN bit is ignored)
  CONFIG  BORV = LO             ; Brown-out Reset Voltage Selection bit (Brown-out Reset Voltage (VBOR) set to 1.9V)
  CONFIG  PPS1WAY = ON          ; PPSLOCKED One-Way Set Enable bit (The PPSLOCKED bit can be set once after an unlocking sequence is executed; once PPSLOCKED is set, all future changes to PPS registers are prevented)
  CONFIG  STVREN = ON           ; Stack Overflow/Underflow Reset Enable bit (Stack Overflow or Underflow will cause a reset)

; CONFIG3

; CONFIG4
  CONFIG  BBSIZE = BB512        ; Boot Block Size Selection bits (512 words boot block size)
  CONFIG  BBEN = OFF            ; Boot Block Enable bit (Boot Block is disabled)
  CONFIG  SAFEN = OFF           ; SAF Enable bit (SAF is disabled)
  CONFIG  WRTAPP = OFF          ; Application Block Write Protection bit (Application Block is not write-protected)
  CONFIG  WRTB = OFF            ; Boot Block Write Protection bit (Boot Block is not write-protected)
  CONFIG  WRTC = OFF            ; Configuration Registers Write Protection bit (Configuration Registers are not write-protected)
  CONFIG  WRTSAF = OFF          ; Storage Area Flash (SAF) Write Protection bit (SAF is not write-protected)
  CONFIG  LVP = ON              ; Low Voltage Programming Enable bit (Low Voltage programming enabled. MCLR/Vpp pin function is MCLR. MCLRE Configuration bit is ignored.)

; CONFIG5
  CONFIG  CP = OFF              ; User Program Flash Memory Code Protection bit (User Program Flash Memory code protection is disabled)

// config statements should precede project file includes.
#include <xc.inc>
```

## 引脚映射

`xc.inc`包含了寄存器定义。为了程序的可读性，我自己也给一些寄存器定义别名：

```asm
R_PPS   equ RA2PPS
G_PPS   equ RA4PPS
B_PPS   equ RA5PPS
R_PR    equ CCPR1H
G_PR    equ CCPR2H
B_PR    equ PWM3DCH
```

我用`RA2`、`RA4`、`RA5`引脚分别控制红、绿、蓝输出，它们的PWM分别由`CCP1`、`CCP2`、`PWM3`控制。

```asm
    psect main_code,class=CODE,delta=2
    global _main

_main:
    ; initialize pins
    banksel ANSELA
    movlw   0xca
    andwf   ANSELA,f    ; R2,R4,R5 are digital..
    banksel TRISA
    andwf   TRISA,f     ; ..output pins

    banksel R_PPS   ; Output PPS regs are in the same bank
    movlw   1
    movwf   R_PPS   ; R_PIN uses CCP1
    movlw   2
    movwf   G_PPS   ; G_PIN uses CCP2
    movlw   3
    movwf   B_PPS   ; B_PIN uses PWM3
```

在pic-as中，所有汇编代码必须指定其所在的程度段 (psect)。链接程序 (linker) 将为这些 psect 分配地址。这和MPASM不同，MPASM里所有代码都使用绝对地址。使用psect的好处是方便整合C和汇编代码，但也给写纯汇编代码增添了一些麻烦。

## 初始化

设置好引脚后，下一步是初始化定时器和PWM。

```asm
    ; initialize timer2 - PWM clock source
    banksel T2CON
    movlw   5
    movwf   T2CLKCON    ; TMR2 uses MFINTOSC - 500kHz
    movlw   0x00        ; PSYNC=0,CPOL=0,CSYNC=0,MODE=0 (normal timer)
    movwf   T2HLT
    movlw   255
    movwf   T2PR        ; period = 256 clock period
    movlw   0x90
    movwf   T2CON       ; ON, 1:2 prescaler, 1:1 postscaler
    ; PWM frequency is about 977Hz

    ; initialize PWMs
    banksel CCP1CON
    movlw   0x9c
    movwf   CCP1CON     ; EN, FMT=1 (left aligned), MODE=PWM
    movwf   CCP2CON     ; same
    movlw   0x80
    movwf   PWM3CON     ; EN, POL=normal
```

这些设置比较枯燥，需要参考厂商发布的数据表 (datasheet)。如果用C开发，那么也可以用Microchip提供的代码生成器[MCC (Microchip Code Configurator)](https://www.microchip.com/en-us/tools-resources/configure/mplab-code-configurator)。

这里结果是把Timer2设置为大约1kHz的定时器作为PWM的时钟。占空比由`x_PR/T2PR`确定。简单地说，要控制亮度，只需要写一个0-255的数值进`x_PR`寄存器，而无需担心PWM波形是怎么生成的——硬件PWM模块负责这一部分。

PIC汇编的一个缺点是访问寄存器之前必须选择正确的内存bank。如果遗漏了这个指令（手写汇编时很容易发生），那么访问的是别的寄存器，代码也就无法正常工作。

## 主程序

主程序相对来说比较直接，就是上文描述的三个相位：
```asm
    ; initialize RGB output
    banksel R_PR
    movlw   255
    movwf   R_PR
    clrf    G_PR
    clrf    B_PR

    ; mainloop
phase_A:    ; R\ G/ B_
    call    delay
    incf    G_PR
    decfsz  R_PR
    goto    phase_A
phase_B:    ; R_ G\ B/
    call    delay
    incf    B_PR
    decfsz  G_PR
    goto    phase_B
phase_C:    ; R/ G_ B\.
    call    delay
    incf    R_PR
    decfsz  B_PR
    goto    phase_C
    goto    phase_A
```

`delay`是一个延时程序，可以控制颜色变化的速度。可以用一个简单的计数器循环实现，代码我就省略不写了。

## 重置向量

用C开发时，`main`函数是程序的入口。实际上，重置向量 (reset vector) 才是真正的入口——这是微控制器在通电时或者重置时跳转到的地址。在PIC上，重置向量是`0x0`，并且只能放4条指令（因为`0x4`是中断向量）。所以，一般在重置向量的位置只放跳转指令，跳转到真正的入口。C编译器在幕后做了这些事情，而用汇编的时候则需要自己手动做。

```asm
    psect resetVec,class=CODE,delta=2
_resetVec:
    pagesel _main 
    goto    _main
```

在MPLAB X中，需要手动添加链接器选项`-presetVec=0h`，告诉链接器把这个psect放在这个地址。
