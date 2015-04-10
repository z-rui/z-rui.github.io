---
date: 2015-04-11T16:52:00+08:00
title: "用gnuplot将数据可视化"
categories:
  - computing
  - math
tags:
  - Gnuplot
  - GNU
  - 数值分析
---

今天我做了两件事，其一是了解了样条插值的概念，其二是观察了使用有限差分方法求解的调和场。做这两件事都用到了gnuplot。

[Gnuplot](http://www.gnuplot.info/)是一个可用于作图的免费软件。使用它可以将数值计算的结果以图像的方式呈现，以获得直观的感受。

# 观察插值函数

试想原本有一个连续函数，由于测量的原因现在只知道其中的个别点的位置。

{{< figure src="/media/gnuplot-sample.png" width="100%" >}}

但是我想知道中间某个点处的函数值，我会这么做：用一条光滑的曲线把这些点连接起来，然后根据图像来找到对应的函数值。这就是科学实验中常用的图解法的原理。

<!--more-->

现在用计算机来求解，计算机虽然没法握着铅笔随手画出一条光滑的曲线来，但它可以通过各种计算，得到一个插值函数。这个插值函数经过所有的已知点，并且处处连续。看来插值函数是对原来的函数的一个良好近似。

[GNU Scientific Library](http://www.gnu.org/software/gsl/)可以用于求插值函数。显然，插值函数没有唯一的标准，有各种插值的方法可供使用。下面的程序分别使用[多项式插值](http://zh.wikipedia.org/zh-cn/%E5%A4%9A%E9%A1%B9%E5%BC%8F%E6%8F%92%E5%80%BC)和[三次样条插值](http://zh.wikipedia.org/wiki/%E6%A0%B7%E6%9D%A1%E6%8F%92%E5%80%BC#.E4.B8.89.E6.AC.A1.E6.A0.B7.E6.9D.A1.E6.8F.92.E5.80.BC)算法，求一组数据的插值函数。

```c
#include <stdio.h>
#include <gsl/gsl_spline.h>

#define N 11

static double X[] = {
	-5., -4., -3., -2., -1., 0., 1., 2., 3., 4., 5.,
};

static double Y[] = {
	0.038461538, 0.058823529, 0.1, 0.2, 0.5, 1,
	0.5, 0.2, 0.1, 0.058823529, 0.038461538
};

int main()
{
	double xi, yi;
	gsl_spline *spline, *poly;

	spline = gsl_spline_alloc(gsl_interp_cspline, N);
	poly = gsl_spline_alloc(gsl_interp_polynomial, N);

	gsl_spline_init(spline, X, Y, N);
	gsl_spline_init(poly, X, Y, N);

	for (xi = X[0]; xi <= X[N-1]; xi += 0.01) {
		printf("%g %g %g\n", xi,
			gsl_spline_eval(spline, xi, 0),
			gsl_spline_eval(poly, xi, 0));
	}

	gsl_spline_free(spline);
	gsl_spline_free(poly);
	return 0;
}
```

如果更加高级的数值处理语言，也许一行代码就搞定了。之前我知道多项式插值的原理：两点确定一条直线，过3点确定一条抛物线……过n个样本点可以确定一个n阶的多项式。样条插值是今天头一次听说。和多项式插值很不同的是，在每两个样本点之间，样条插值的解析式都不同，尽管整体上它仍然是一个连续函数。

究竟有什么不同呢，运行上面的程序，就可以得到插值函数在 *各点* 的函数值。（可笑的是，所得到的仍然只是有限个点的函数值，我只不过是把样本点取得更密了一些罢了。看来计算机永远无法做出真正的连续函数——毕竟它在结构上就是离散的。）为了检查所求出来的插值函数究竟是什么形状，这时就要用到gnuplot。

{{< figure src="/media/gnuplot-interpolation.png" width="100%" >}}

从上面的图来看，三次样条插值所得的插值函数更接近我脑海中的“光滑曲线”的样子。多项式插值函数虽然很机智地穿过了所有的样本点，但还是有些不老实。毕竟，所有的多项式都是\\( \lim\_{x\to\infty} P(x) = \infty \\)的，叫它跟随一个渐趋平缓的函数的足迹，怎么可能束缚的住它要发散的本性呢？于是就产生了震荡。

事实上，这些样本点是我在函数\\(y={1 \over 1+x^2}\\)上面取的。那么，三次样条插值的结果和原来的这个函数是否接近呢？

{{< figure src="/media/gnuplot-interpolation2.png" width="100%" >}}

看来还是很接近的。当然，这些样本点未必只能对应原来的那个函数，所以，并不能以插值函数是否接近原来的函数作为衡量插值好坏的标准。

尽管如此，抖动很大的多项式插值在这个地方显然是不好的。通过GNU Scientific Library的计算和gnuplot的作图，我可以很直观地认识到这一点。

# 观察调和场

《工程电磁场》的课程介绍了有限差分法求解拉普拉斯方程的的办法，从而在已知边界条件的情况下，调和场在各点处的值。

作为上机实验，需要编写计算机程序来实现有限差分法。有一个问题是这样的：在一个静电屏蔽的矩形区域（边界电位为0）内，放置一个电位为U的矩形。求区域内的电位并且画出等电位线。

程序很简单，这里不再给出。但是，计算机程序经过运算，给出的是区域内各点的电位值。在这个问题里，区域被分为5000个离散的点，程序的输出也就是这5000个点的电位值。虽然计算精度声称达到了\\(10^{-5}\\)的量级，但是那么多的输出数据，不借助图形化的手段，恐怕是没法让人有直观的理解。

Gnuplot的```splot```命令可用于三维作图。因为我求的是二维调和场，作三维图像可以很直观地展示场的形态。

```
set contour base
splot "output"
```

{{< figure src="/media/gnuplot-potential.png" width="100%" >}}

弥漫在空间中的点表示电位，点越高，则对应的xy坐标处的电势越高。底面上的线是等位线。注意，要作等位线，则输入数据必须以grid-data的形式呈现。（参加帮助文档的splot example datafile。）

有的gnuplot还有```pm3d```功能。使用```pm3d```画出来的图像具有色彩的渐变，视觉效果更好。

```
splot "output" with pm3d
```

{{< figure src="/media/gnuplot-potential2.png" width="100%" >}}

有没有感觉这个场就好像一块布盖在了一个长方体的箱子上的样子？事实可能的确如此。

切换到俯视图，就可以得到题目所要求的“电位分布及等电位线”。

```
unset clabel
set view 0, 0
replot
```

{{< figure src="/media/gnuplot-contour.png" width="100%" >}}

# 奇闻异事

Gnuplot其实不是GNU系列的软件，它的名字里含有Gnu三个字母也许只是巧合。这也就是为什么我不把它叫做GNUplot或者GNU Plot或者其他什么不太对劲的名字。它是一个免费软件，但并不授予你自由分发修改过的代码的权利。这和GNU的General Public License的精神完全不同。

当然，文中提到的GNU Scientific Library，（毫无疑问地）是GNU软件，并且采用GPL授权。从而任何基于该软件的代码都必须采用GPL授权，恐怕本文中的那么一小段也不例外。不过，这么一小段代码价值太小，谁会来较真呢？
