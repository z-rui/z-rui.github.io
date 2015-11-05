---
date: 2015-11-04
title: "使用OpenMP做并行计算"
---

[OpenMP](http://openmp.org/wp/)是一套语言扩展规范，可以给C/C++和Fortran提供并行计算的功能。今天我用OpenMP写了[N皇后问题](https://en.wikipedia.org/wiki/Eight_queens_puzzle)的并行计算程序。该程序使用经典的回溯搜索算法。

程序如下：
```c
#include <stdio.h>
#include <omp.h>

unsigned all;
unsigned count = 0;

void search(unsigned row, unsigned ld, unsigned rd)
{
	unsigned pos, p;

	if (row == all) {
#pragma omp critical
		++count;
		return;
	}
	pos = all & ~(row|ld|rd);
	while ((p = pos & -pos)) {
		pos ^= p;
		if (p)
			search(row | p, (ld | p) << 1, (rd | p) >> 1);
	}
}

int main(int argc, char *argv[])
{
	int i;
	int N = 8;

	if (argc == 2)
		sscanf(argv[1], "%d", &N);
	all = (1 << N) - 1;
#pragma omp parallel for
	for (i = 0; i < N/2; i++) {
		unsigned p = 1 << i;
		search(p, p << 1, p >> 1);
	}
	count <<= 1;
	if (N & 1) {
		unsigned p = 1 << (N/2);
		search(p, p << 1, p >> 1);
	}
	printf("%d\n", count);
	return 0;
}
```
<!--more-->

`#pragma omp`系列的杂注是OpenMP的语法。即便去掉它(或者编译器不理解它)，也不影响程序的逻辑，这是使用OpenMP写程序的一个吸引人的地方。

* `#pragma omp critical`表明接下来的一个语句(`++count;`)是临界区段([critical section](https://en.wikipedia.org/wiki/Critical_section))，以防出现竞争条件([race condition](https://en.wikipedia.org/wiki/Race_condition))。去掉这一行，则结果可能是非确定性(nondeterministic)的。
* `#pragma omp parallel for`的作用是使得接下来的`for`循环的循环体在不同的线程中执行，从而实现并行计算。

按照算法分析的经典理论，回溯算法的时间复杂度为\\(O(n!)\\)。这个程序用了一点位运算的小技俩，并且简单地利用了问题的解的对称性，实际上自身的运行速度已经要远高于书本上常见的回溯算法程序。感兴趣的人可以尝试自己写一个求解N皇后的解的个数（不需要记录解）的程序，然后设置N=16，看看需要运行多长时间。算法的正确性可以通过对照OEIS [A000170](http://oeis.org/A000170)来实现。

我的测试工具是：

* Intel(R) Core(TM) i5-3337U CPU @ 1.80GHz，1801 Mhz，2 个内核，4 个逻辑处理器
* gcc version 5.2.0 (Rev4, Built by MSYS2 project)

测试若干种情况：

* 编译器优化设置为`-O0`, `-O`, `-O2`, `-O3`。
* 开启或关闭`-fopenmp`。
* N=16。

测试结果如下：

<table>
<tr>
<th></th><th><code>-fopenmp</code>关 (1)</th><th><code>-fopenmp</code>开 (2)</th><th>(1)/(2)</th>
</tr>
<tr>
<th><code>-O0</code></th><td>11.165s</td><td>4.255s</td><td>262%</td>
</tr>
<tr>
<th><code>-O1</code></th><td>5.894s</td><td>2.875s</td><td>205%</td>
</tr>
<tr>
<th><code>-O2</code></th><td>5.685s</td><td>2.569s</td><td>221%</td>
</tr>
<tr>
<th><code>-O3</code></th><td>6.674s</td><td>2.702s</td><td>247%</td>
</tr>
</table>

看起来这个版本的gcc的`-O3`选项采用了较差的优化策略，使得程序的运行时间不降反升。我曾写过一个利用显式栈替代递归的等价的程序，对于那个程序，`-O3`选项可以极大地缩短程序的运行时间。

从以上数据可以看出，在这个例子中，编译器优化是很有用的，可以减少约一半的程序运行时间。不使用OpenMP的程序在运行时，CPU的占用率维持在25%，这大概是因为4个线程只有1个占用了100%而另外3个都处于空闲状态。相反，使用OpenMP的程序在运行时，CPU的占用率维持在100%，说明4个线程全部处于繁忙状态。

尽管开启了4个线程，但是只获得了2倍的运行速度，这是因为并行程序并不是完全并行的，有一部分必须按照顺序执行。另一方面，所谓4个逻辑处理器，指的是2个核心(core)的每个利用[超线程](http://www.intel.com/content/www/us/en/architecture-and-technology/hyper-threading/hyper-threading-technology.html)技术实现2个线程同时运行。

声明：我并不很了解超线程的具体机制，根据我的猜测，这种技术应该不能实现两个线程真正的“同时”运行，而只是能够在两个线程之间迅速地进行切换而已。例如，在一个线程的指令陷入stall状态的时候，就可以迅速地切换至另一个线程，相对没有这种技术的情况下，可以提升运行性能。

如果处理器具有更多的内核，那么运行速度大体上也会按比例提高。所以，现在处理大规模计算的一种办法是，把计算任务平均分配给非常多的内核（甚至可以在不同的计算机上），让它们同时进行计算，以规模换速度。这和平时所说的“人多力量大”应该是一个道理。

当然，算法层面的优化也是很重要的，例如这个程序利用解的对称性，立马就减少了约一半的计算量，也就是获得了2倍的运行速度。对于一些问题，如果算法上有所突破，获得了渐进时间复杂度层面的改进，那么所获得的运行速度的提升就不是以“多少倍”就能衡量的了的了。
