---
date: "2015-02-26T07:01:00+08:00"
title: "减小Lua的二进制体积"
categories:
  - hack
  - Lua
---

[Lua](http://www.lua.org)本身已经是一个十分轻量级的脚本语言。在我的64位Windows上编译出来的```lua53.dll```仅248K。(参照物：```python34.dll```为3963K。)

[Lua Technical Note 1](http://www.lua.org/notes/ltn001.html)介绍了通过删减功能来进一步减小Lua的二进制体积的方法。这篇文章针对的是Lua 3.2，16年前的版本，尽管如此，它仍然给了我很大的启发。

首先看Lua 5.3中哪些模块占用的二进制体积最多：
```
size	%all	%core	file
23571	7%	12%	lauxlib.o
20909	6%	11%	lparser.o
20256	6%	11%	lapi.o
18403	5%	10%	lvm.o
14413	4%	8%	llex.o
13526	4%	7%	lcode.o
12584	4%	7%	lgc.o
10214	3%	5%	ldebug.o
8967	3%	5%	lobject.o
8776	2%	5%	ldo.o
7002	2%	4%	ltable.o
5087	1%	3%	lundump.o
4887	1%	3%	lstate.o
3970	1%	2%	ltm.o
3832	1%	2%	ldump.o
2818	1%	1%	lstring.o
2525	1%	1%	lfunc.o
2170	1%	1%	linit.o
1810	1%	1%	lopcodes.o
1498	0%	1%	lmem.o
1406	0%	1%	lzio.o
860	0%	0%	lctype.o
189484	54%	100%	(core)
```
<!--more-->
```
size	%all	%lib	file
37418	11%	27%	lstrlib.o
16909	5%	12%	liolib.o
13517	4%	10%	lbaselib.o
12406	4%	9%	ldblib.o
12291	3%	9%	loadlib.o
10916	3%	8%	lmathlib.o
9813	3%	7%	ltablib.o
7973	2%	6%	loslib.o
5969	2%	4%	lutf8lib.o
5241	1%	4%	lcorolib.o
4922	1%	4%	lbitlib.o
137375	39%	100%	(lib)
```
```
size	%all	file
13135	4%	luac.o
12431	4%	lua.o
25566	7%	(exe)
```

所有二进制文件的大小之和是344K，其中，核心部分为185K，标准库为134K。

可见Lua的二进制文件中，标准库占了很大的比例。有些不太重要的标准库，可以直接去掉。

* 如果不需要调试功能，可以把debug库(```ldblib.o```)去掉。
* 如果不需要处理UTF-8字符串，也可以把utf8库(```lutf8lib.o```)去掉。
* 既然Lua 5.3已经提供了位运算符，如果不需要兼容性的话，可以把bit32库(```lbitlib.o```)也去掉。

这样，总共就减小了23K。要去掉某个特定的标准库非常简单，只要不要编译对应的```.c```文件，并且在```linit.c```中注释掉相应的条目就可以了。标准库不存在依赖关系，所以你可以随心所欲地去掉任意一个。

下面看看Lua的核心部分有没有什么可以削减的东西。Technical Note 1中提到，编译器是最值得去掉的部分，因为它们占了很大的体积。这句话同样适用于Lua 5.3：语法分析器(```lparser.o```)、词法分析器(```llex.o```)和代码生成器(```lcode.c```)的体积总共有48K之多。

去掉了编译器，Lua解释器怎么解释脚本呢？我们可以先使用独立的编译器程序```luac```(另外编译)，把Lua脚本编译成字节码，然后让解释器运行。一个副作用是脚本中无法动态编译，有些所谓的元编程(meta programming)技巧无法施展。此外，独立的解释器```lua```也将不能在交互模式下运行。

去掉编译器比去掉标准库要麻烦一些，因为Lua的核心部分对编译器有一些依赖。一个是词法分析器的初始化```luaX_init```，另一个就是语法分析器```luaY_parser```。如果只是简单地不编译上述的若干文件，则会因为缺少符号而无法通过链接。我们所要做的，就是定义这两个函数，但是不包含任何实质性的功能。调用```luaY_parser```时，抛出一个错误即可。

将下方的代码保存为```stub.c```。

```c
#include "llex.h"
#include "lparser.h"
#include "lauxlib.h"

void luaX_init(lua_State *L) { /* nothing */ }

LClosure *luaY_parser(lua_State *L, ZIO *z, Mbuffer *buff,
		Dyndata *dyd, const char *name, int firstchar) {
  luaL_error(L, "parser not loaded");
  return NULL;
}
```

然后编译(MinGW 64位)：
```
$ gcc -shared -Os -o lua53.dll -DBUILD_AS_DLL \
lapi.c lctype.c ldebug.c ldo.c ldump.c lfunc.c lgc.c \
lmem.c lobject.c lopcodes.c lstate.c lstring.c ltable.c \
ltm.c lundump.c lvm.c lzio.c stub.c \
lauxlib.c lbaselib.c linit.c

$ strip --strip-unneeded lua53.dll
```
我已在```linit.c```中去掉了几乎所有的标准库。这样编译出来的```lua53.dll```只有131KB，比正常安装的版本小了将近一半。去掉了这么多东西，Lua的基本功能还是存在的，像下面这样的程序：

```lua
local function search(n)
  local allbits = (1 << n) - 1
  local count = 0
  local function rec(row, ld, rd)
    if row == allbits then
      count = count + 1
      return
    end
    local avail = allbits & ~(row | ld | rd)
    while avail ~= 0 do
      local pos = avail & -avail
      avail = avail ~ pos
      rec(row | pos, (ld | pos) << 1, (rd | pos) >> 1)
    end
  end
  rec(0, 0, 0)
  return count
end

print(search(tonumber(arg[1]) or 8))
```

仍然可以很顺利地求解八皇后问题（输出：92），因为（除了```print```函数以外）它没有用到任何标准库函数，并且也不需要进行动态编译。
