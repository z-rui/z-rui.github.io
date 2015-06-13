---
date: 2015-06-08T23:59:00+08:00
title: "高考数学填空题"
---

又是一年高考日。今年江苏省的高考数学卷中，有一道填空题是这样的：

设向量\\(\vec{a\_k} = \left(\cos{k\pi\over6}, \cos{k\pi\over6} + \sin{k\pi\over6}\right)\\)，则\\[\sum\_{k=0}^{11} \vec{a\_k}\cdot\vec{a\_{k+1}}={?}\\]

这样的题似乎只需要你足够地胆大心细总是可以计算出来。

<!--more-->

## 算术法

\begin{array}{r|rrr}
   k & \vec{a\_k} & \vec{a\_k}\cdot\vec{a_{k+1}} & \hbox{部分和} \cr
   0 & \left(              1,                 1\right) &   {1\over2} + \sqrt3 & {1\over2} +           \sqrt3 \cr
   1 & \left( {\sqrt3\over2},  {1+\sqrt3\over2}\right) &  1 + {3\sqrt3\over4} & {3\over2} + { 7\sqrt3\over4} \cr
   2 & \left(      {1\over2},  {1+\sqrt3\over2}\right) &     {1+\sqrt3\over2} &         2 + { 9\sqrt3\over4} \cr
   3 & \left(              0,                 1\right) &    {-1+\sqrt3\over2} & {3\over2} + {11\sqrt3\over4} \cr
   4 & \left(     -{1\over2}, {-1+\sqrt3\over2}\right) & -1 + {3\sqrt3\over4} & {1\over2} + {14\sqrt3\over4} \cr
   5 & \left(-{\sqrt3\over2},  {1-\sqrt3\over2}\right) &  -{1\over2} + \sqrt3 &             {18\sqrt3\over4} \cr
   6 & \left(             -1,                -1\right) &   {1\over2} + \sqrt3 & {1\over2} + {22\sqrt3\over4} \cr
   7 & \left(-{\sqrt3\over2}, {-1-\sqrt3\over2}\right) &  1 + {3\sqrt3\over4} & {3\over2} + {25\sqrt3\over4} \cr
   8 & \left(     -{1\over2}, {-1-\sqrt3\over2}\right) &     {1+\sqrt3\over2} &         2 + {27\sqrt3\over4} \cr
   9 & \left(              0,                -1\right) &    {-1+\sqrt3\over2} & {3\over2} + {29\sqrt3\over4} \cr
  10 & \left(      {1\over2},  {1-\sqrt3\over2}\right) & -1 + {3\sqrt3\over4} & {1\over2} + {32\sqrt3\over4} \cr
  11 & \left( {\sqrt3\over2}, {-1+\sqrt3\over2}\right) &  -{1\over2} + \sqrt3 &             {36\sqrt3\over4} \cr
\end{array}

如果时间充裕，像上边这样列个表，一步一步计算，只要不算错，也用不了多少时间。这些算术运算，叫初中生也可以做，无非是加减乘除加上平方根罢了。

## 代数法

既然是一道高考题，我最好还是用高中的代数知识来解决它。

首先根据**三角恒等变换**的知识
\\[\eqalign{
\cos x \cos y &= {1\over2}\left[\cos(x+y) + \cos(x-y)\right] \cr
(\sin x + \cos x)(\sin y + \cos y)
&= (\sin x \sin y + \cos x \cos y) + (\sin x \cos y + \cos x \sin y) \cr
&= \cos(x-y) + \sin(x+y)
}\\]
不难得到
\\[\eqalign{
\vec{a\_k}\cdot\vec{a\_{k+1}}
&= \cos{k\pi\over6}\cos{(k+1)\pi\over6} + \left(\sin{k\pi\over6}+\cos{k\pi\over6}\right)\left[\sin{(k+1)\pi\over6}+\cos{(k+1)\pi\over6}\right] \cr
&= {1\over2}\cos{(2k+1)\pi\over6} + {1\over2}\cos{-\pi\over6} + \cos{-\pi\over6} + \sin{(2k+1)\pi\over6} \cr
&= {1\over2}\cos{(2k+1)\pi\over6} + \sin{(2k+1)\pi\over6} + {3\sqrt3\over4} \cr
}\\]

算到这里，聪明的人已经一眼能够看出来，正弦和余弦函数因为其参数的取值依次是\\({\pi\over6}, {3\pi\over6}, {5\pi\over6}, \dots, {23\pi\over6}\\)，在单位圆上恰好是对称出现的，所以求和以后一定相消。

如果看不出来，就要考虑：高中数学中，求数列的前\\(n\\)项和有多种方法，例如倒序相加法和错位相减法。倒序相加法的本质是重排求和项的顺序，让第一项和最后一项配对，第二项和倒数第二项配对……以便利用求和项的对称性。事实上，如果运用倒序相加法，则可以消去式中的正弦函数，但并不能消去余弦函数。三角函数除了具有对称性以外，还具有周期性，特别是\\(\sin(x+\pi) = -\sin x\\), \\(\cos(x+\pi)=-\cos x\\)这样的规律非常值得利用。所以可以考虑另外一种配对的方式。

如果设\\(b\_k = \vec{a\_k}\cdot\vec{a\_{k+1}}\\)，运用上述性质，不难证明\\[ b\_k + b\_{k+3} = {3\sqrt3\over2} \\]
那么所求的和
\\[\eqalign{
\sum\_{k=0}^{11} b\_k
&= \sum\_{k=0}^2 b\_k + \sum\_{k=3}^5 b\_k + \sum\_{k=6}^8 b\_k + \sum\_{k=9}^{11} b\_k \cr
&= \sum\_{k=0}^2 (b\_k + b\_{k+3}) + \sum\_{k=6}^8 (b\_k + b\_{k+3}) \cr
&= 3\times{3\sqrt3\over2} + 3\times{3\sqrt3\over2} \cr
&= 9\sqrt3
}\\]

## 评论

不知道考生做这题是什么感觉。这个题目我暂时想不到更容易的办法。就我知道的两个做法而言，其实没什么意思，只不过是一些纯粹的计算而已。在计算机如此普及的时代，这样的计算工作不应该再让人手工完成。使用计算机代数系统(CAS)可以在眨眼之间求出结果，并不需要任何技巧。

[Wolfram|Alpha的计算结果](http://www.wolframalpha.com/share/clip?f=d41d8cd98f00b204e9800998ecf8427efh6p47n86i)

{{% figure caption="TI-89 Titanium的计算结果" src="/media/ti89-1.jpg" %}}
