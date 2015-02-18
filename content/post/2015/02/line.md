---
date: "2015-02-18T02:01:00+08:00"
draft: true
title: "在位图上画直线"
---

在显示器上画出的各种图形中，直线是最简单的一种。这里说的**直线**，有确定的起点和终点；数学家也许更愿意称其为**线段**。

画直线是如此的简单，以至于人们也许已经忽略了它也是逐个像素点绘制出来的，而把它当作一种原子的操作。我也是如此认为，直到昨天一位同学谈他的计算机图形学课程时，说到了这个问题。

于是我尝试用自己的知识来解决这个问题。显示器上的直线和数学上的线段最明显的差别是，前者是由有限个点构成的，而后者不是。所以，显示器上的直线只是一种近似，而我要使得这种近似的效果尽量好。

为了简化问题，假设显示器显示的是一幅单色的位图。我想到的做法是这样的：

  1. 画起点。
  2. 画完一个点后，在其周围的点中，找到距离给定直线最近的点，然后画这个点。
  3. 重复第2步，直到画了终点为止。

这个做法看上去应该没有问题，只有一些细节部分需要考虑。
<!--more-->

## 点到直线距离

首先要考虑的是，怎样确定一个点到给定直线的距离最近？平面解析几何给出了点\\((x\_0, y\_0)\\)到直线\\(Ax+By+C=0\\)的距离公式：
\\[d={|Ax\_0+By\_0+C|\over\sqrt{A^2+B^2}}\\]

给出的是两个端点，而距离公式中使用的是直线方程，其中的3个待定系数需要求解。根据直线的两点式方程，过\\((x_1, y_1),(x_2,y_2)\\)的直线方程为
\\[(y_1-y_2)x-(x_1-x_2)y+x_1y_2-x_2y_1=0\\]

如果给出第3个点\\((x_3,y_3)\\)，则该点到直线的距离
\\[d = {\left| (y_1-y_2)x_3-(x_1-x_2)y_3+x_1y_2-x_2y_1 \right| \over \sqrt{(y_1-y_2)^2 + (x_1-x_2)^2}}\\]
看上去有些复杂。实际上，对于给定的直线而言，式中的分母是常数，因此无需计算，只需比较分子的大小即可。而分子
\\[\eqalign{
 &\left| (y_1-y_2)x_3-(x_1-x_2)y_3+x_1y_2-x_2y_1 \right| \cr
=&\left|
 \left|\matrix{y_1 & 1 \cr y_2 & 1}\right|x_3
-\left|\matrix{x_1 & 1 \cr x_2 & 1}\right|y_3
+\left|\matrix{x_1 & y_1 \cr x_2 & y_2}\right|
  \right| \cr
=&\left|\det\pmatrix{x_1 & y_1 & 1 \cr x_2 & y_2 & 1 \cr x_3 & y_3 & 1}\right|
}\\]

是一个非常好记的形式。

## 画线的方向

上文使用两个端点确定了直线的方程，但是仍然有一个细节值得注意：画完起点以后，面向终点和背离终点的两个方向都存在一个到直线距离最短的点，因此必须做出取舍。有两种办法处理这个问题。

### 对称化简

把所有对称的情况化归到一种最基本的情况，在这种情况里（比方说），斜率永远在\\([0,1]\\)区间里，而起点是左下角的那个点。这样的话，枚举所谓“周围的点”，只要枚举两个点：右边的点和右上角的点。

### 单调性限制

这种做法在编程中更加方便。其原理是，限制所画的各点的坐标是单调变化的，其单调的方向由给定的两点的相对位置而定。

## 演示程序

> Talk is cheap, show me your code.

```c
#include <stdlib.h>

typedef void *Bitmap; /* 位图类型(代码中未给出实现) */
extern void set_pixel(Bitmap b, int x, int y, int color);
/* 把位图b的(x, y)坐标设置为color颜色 */
extern void is_pixel(Bitmap b, int x, int y);
/* 检查坐标(x, y)是否在位图b的范围内 */

static
int crossp3(int x1, int y1, int x2, int y2, int x3, int y3)
{
	return x1*(y2-y3) + x2*(y3-y1) + x3*(y1-y2);
}

void line(Bitmap b, int x1, int y1, int x2, int y2)
{
	int x, y, xx, yy;
	int dxmin, dymin;

	dxmin = (x1 < x2) ? 0 : -1;
	dymin = (y1 < y2) ? 0 : -1;
	x = x1;
	y = y1;
	for (;;) {
		int xx, yy, dx, dy, x3, y3;
		int crossp, min_crossp = INT_MAX;

		set_pixel(b, x, y, 1);
		if (x == x2 && y == y2)
			break;
		for (dx = dxmin; dx <= dxmin+1; dx++) {
			for (dy = dymin; dy <= dymin+1; dy++) {
				if (!dx && !dy)
					continue;
				if (!is_pixel(b, x3 = x+dx, y3 = y+dy))
					continue;
				crossp = abs(crossp3(x1, y1, x2, y2, x3, y3));
				if (crossp < min_crossp) {
					min_crossp = crossp;
					xx = x3;
					yy = y3;
				}
			}
		}
		x = xx;
		y = yy;
	}
}
```
