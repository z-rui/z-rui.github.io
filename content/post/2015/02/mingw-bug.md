---
date: "2015-02-09T08:26:00+08:00"
title: "发现了MinGW GCC的一个bug"
category:
  - computing
tags:
  - MinGW
  - GCC
  - bug
---

我今天发现MinGW内置的打印函数```__mingw_printf```在处理```%a```格式的时候会产生不正确的输出。下面这段程序体现了这个问题。

```
#include <stdio.h>
#include <stdarg.h>

int main()
{
	printf("%a %a\n", 1.0, 1.1);
	__mingw_printf("%a %a\n", 1.0, 1.1);
	return 0;
}
```

输出是

```
0x1.000000p+0 0x1.19999ap+0
0x0p-63 0x8.cccccccccccdp-3
```

因为浮点数```1.0```显然不能表示为```0x0p-63```，所以```__mingw_printf```是错的。

这个错误我提交在 http://sourceforge.net/p/mingw-w64/bugs/459/ ，过几天我看看有没有人回应。

<!--more-->

这个bug是在我玩弄Lua解释器的时候发现的，我尝试了

```lua
('%a %a'):format(1, 1.1) --> 0x0p-63 0x8.cccccccccccdp-3
```

结果和预想的不一样。最初我以为是Lua在处理数值是把整型/浮点型给搞错了，后来对Lua解释器进行单步调试，发现Lua没有问题，可是```sprintf```输出的内容就是不对。

于是我另外写了一个演示程序，使用```sprintf```来输出，结果发现输出的内容是正确的，真是奇了怪了。

难道是Lua自己重新实现了一个```sprintf```？找了找代码，没找到。后来鬼使神差地用了```gcc -S```，把汇编代码给翻了出来，一看，好家伙，在Lua的解释器中，gcc用```__mingw_vsprintf```自行实现了```sprintf```，根本就没有调用C标准库里边的```sprintf```。而在我写的演示程序里，gcc又没有这样做。

于是真相大白：问题出在```__mingw_*printf```函数，它对```%a```格式的实现应该是错的。

寻找这个bug的经历说明几个问题：

  1. 有错误的程序不一定是错的。
  2. 很多问题的解决都有偶然因素在里边。
  3. 尽管程序员不可信，有时候编译器也不可信。(本质上还是程序员不可信-_-。)
