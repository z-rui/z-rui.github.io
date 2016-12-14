---
date: 2016-12-13T21:55:00+08:00
title: 复数和矢量运算
---

在工程领域中，复数\\(\dot{A}=A\mathrm{e}^{\mathrm{i}\varphi}\\)被用来指代一个有效值为\\(A\\)，初相位为\\(\varphi\\)的正弦量\\(\Re\big[\sqrt2\dot{A}\big]=\sqrt2A\cos(\omega t+\varphi)\\)。这样的复数常被称作相量(phasor)，通常会用更简便的Steinmetz记号\\(\newcommand\phase\[1\]{\,\underline{\smash{\kern -.84pt\lower -.43pt{/}#1}}}A\phase\varphi\\)表示，并且使用角度制。

<img style="margin: auto;" src="/media/vector-1.png">

<!--more-->

平面上的矢量和复数之间有一一对应的关系。如图，\\(A\phase\varphi\\)对应矢量\\(\overrightarrow{OP}\\)，它也可以用直角坐标形式表示为\\(x+y\,\mathrm{i}\\)。通常，把\\(A\\)取成非负的，称为模：\\(r = \sqrt{x^2+y^2}\\)；把\\(\varphi\\)取在区间\\((-\pi,\pi]\\)内，称为幅角：\\(\varphi = \mathrm{atan2}(y, x)\\)。

可以用欧拉公式证明两种形式是等价的，用矢量可以形象地表示出这一关系，便于记忆。

用复数和矢量分析交流电是很方便的。例如，在三相电源中，设A相电压\\[\dot{U}\_A = 220\phase{0^\circ}\;\mathrm{V},\\]则另外两相电压分别可以表示为
\begin{align}
\dot{U}\_B &= 220\phase{-120^\circ}\;\mathrm{V}, \\\\ 
\dot{U}\_C &= 220\phase{120^\circ}\;\mathrm{V}.
\end{align}

这种记法的物理意义很明显。并且用这种表示进行算术运算也具有明确的意义。例如，线电压
\\[\dot{U}\_{AB} = \dot{U}\_A - \dot{U}\_B = 220\sqrt3\phase{30^\circ}\;\mathrm{V}.\\]
可以直接从计算结果中读出它的幅值和相位。

上面的计算因为涉及特殊角，所以可以笔算得出结果。甚至可以画出对应矢量的图形（恰好构成等腰三角形），用图解法来做。

<img style="margin: auto;" src="/media/vector-3.png">

下面讨论一般的计算办法。

首先把\\(\dot{U}\_B = 220\phase{-120^\circ}\\)转换成直角坐标形式。先用诱导公式把三角函数的参数化归到第一象限：\\(\sin(-120^\circ)=-\sin 60^\circ\\)，\\(\cos(-120^\circ)=-\cos 60^\circ\\)。查数学用表：
\begin{array}{rrr}
&\lg 220 =& 2.3424 \\\\ 
\+& \lg\sin 60^\circ =& \overline{1}.9375 \\\\\hline
&& 2.2799\rlap{\quad=\lg190.5}
\end{array}
\begin{array}{rrr}
&\lg 220 =& 2.3424 \\\\ 
\+& \lg\cos 60^\circ =& \overline{1}.6990 \\\\\hline
&& 2.0414\rlap{\quad=\lg110.0}
\end{array}

添上符号，得\\(\dot{U}\_{B} = -110.0 - j190.5\\)。然后就可以进行减法计算
\\[\dot{U}\_{AB} = 220 - (-110 - j190.5) = 330.0 + j190.5. \\]

为了求出幅值和相位，需要再转换为极坐标形式。一般地，可以归结为一个解直角三角形的问题。

<img style="margin: auto;" src="/media/vector-2.png">

设三边按从小到大的顺序分别为\\(a,\,b,\,c\\)，边\\(a\\)所对的角为\\(\alpha\\)。则\\(\alpha=\arctan\frac{a}{b}\\)。注意我们总用小数除以大数。
\begin{array}{rrr}
&\lg 190.5 =& 2.2799 \\\\ 
-&\lg 330.0 =& 2.5185 \\\\\hline
&& \overline{1}.7614 \rlap{\quad=\lg\tan30^\circ}
\end{array}

接下来计算斜边长\\(c = \frac{a}{\sin\alpha}\\)：
\begin{array}{rrr}
&\lg 190.5 =& 2.2799 \\\\ 
-&\lg\sin 30^\circ =& \overline{1}.6990 \\\\\hline
&&2.5809 \rlap{\quad=\lg381.0}
\end{array}

根据复数所在的象限和计算得出的\\(\alpha\\)，可以确定\\(\varphi\\)。各种情况总结在下表中。实际计算的时候，画出坐标系和复数所在的直角三角形，比较容易看出\\(\alpha\\)和\\(\varphi\\)的关系。

\begin{array}{cccc}
\hline
\text{象限} & x & y & \varphi\_{|x| \ge |y|} & \varphi\_{|x| < |y|} \\\\\hline
\text{I}    & + & + & \alpha & 90^\circ - \alpha \\\\ 
\text{II}   & - & + & 180^\circ - \alpha & 90^\circ + \alpha \\\\ 
\text{III}  & - & - & -(180^\circ - \alpha) & -(90^\circ + \alpha)\\\\ 
\text{IV}   & + & - & -\alpha & -(90^\circ - \alpha) \\\\\hline
\end{array}

由上表知，计算结果位于第I象限，并且\\(|x| \ge |y|\\)，所以\\(\varphi=\alpha=30^\circ\\)。因此极坐标形式为\\(\dot{U}\_{AB} = 381.0\phase{30^\circ}\;\mathrm{V}.\\)

## 矢量计算尺

用矢量型的计算尺可以代替查表的工作。将角度化归到第一象限，以及根据象限确定\\(\varphi\\)取值的步骤和前面一样，此处省略。

当\\(r=220\\), \\(\varphi=60^\circ\\)时，计算过程如下：

1. 移动滑尺使C刻度右端线对齐D刻度2.2；
2. 将游标移至S刻度60，在D刻度读出1.905，得\\(y=190.5\\)；
3. 将游标移至S刻度红字60，在D刻度读出1.100，得\\(x=110.0\\)。

当\\(a=330.0\\), \\(b=190.5\\)时，计算过程如下：

1. 移动滑尺使C刻度右端线对齐D刻度3.3；
2. 将游标移至D刻度1.905，在T刻度读出30，得\\(\alpha=30^\circ\\)；
3. 移动滑尺使S刻度的30与游标对齐；
4. 将游标移至C尺右端线，在D尺读出3.807，得\\(c=380.7\\)。可见，有少许误差。

## 科学计算器

利用科学计算器上的`sin`、`cos`和<code>tan<sup>-1</sup></code>功能代替查表，也可以求出待求的量，而且不论是速度还是精度都更优。`sin`和`cos`可以接受任意角，因此不需要把角度化归到第一象限。但是，<code>tan<sup>-1</sup></code>给出的角度在\\((-\pi,\pi]\\)内，因此仍然需要根据象限来确定幅角的取值。

现在市面上的科学计算器大多都含有直角坐标和极坐标转换的功能，分别是`Rec(`和`Pol(`。

`Rec(r,θ)`相当于计算\\(r\cos\theta\\)和\\(r\sin\theta\\)。结果将分别被保存到计算器的变量E和F(有些计算器可能是其他变量)中。

* `Rec(50,53.1301)` → `30`
* `E` → `30`
* `F` → `40`

`Pol(x,y)`相当于计算\\(\sqrt{x^2+y^2}\\)和\\(\mathrm{atan2}(y,x)\\)。这时计算器会根据象限自动确定幅角的值，使用起来更加简便了。

* `Pol(30,40)` → `50`
* `E` → `50`
* `F` → `53.1301`

还有一些科学计算器可以直接做复数计算，它的操作也最简单。

例如，在TI-89中，将角度模式设置为DEG，复数模式设置为POLAR，那么它可以自动地将计算结果显示成类似\\(A\phase\varphi\\)的格式。

* <code>220/(30+40<b><i>i</i></b>)</code> → `(4.4 ∠-53.1301)`

有时候需要得到直角形式，可以用`⏵Rect`语法来实现。

* `(4.4 ∠-53.1301)⏵Rect` → <code>2.64-3.52<b><i>i</i></b></code>
