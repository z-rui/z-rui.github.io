---
date: 2024-06-24T12:03:35-07:00
title: 一个初等不等式
---

我上高中的时候曾经自己总结出了一个不等式：
\\[\frac{a^{n+1}}{c^n}+\frac{b^{n+1}}{d^n} \geq \frac{(a+b)^{n+1}}{(c+d)^n}\\]
其中\\(a,b,c,d>0\\)，且\\(n\\)为正整数。取等号的充分必要条件是\\(\frac a c = \frac b d\\)。

当时发现有若干问题，采用这个不等式可以迅速得到答案，比使用导数求极值的方法少了许多计算量。我自己琢磨了一阵子，终于证明了这个不等式，并记录在了自己的笔记本上。我以为我发现了什么了不起的结论。今天才知道，其实它叫**权方和不等式**，早就有人研究过了。而我在整个高中从未听说过它。

我的笔记本也早已丢失。这两天沿着之前的思路重新证明了一遍。

<!--more-->

## 证明方法

证明这个不等式的方法是利用著名的**算术几何平均不等式**（AM--GM不等式），即
\\[\sqrt[n]{x_1x_2\cdots x_n} \leq \frac{x_1+x_2+\cdots+x_n}{n}\\]
当且仅当\\(x_1=x_2=\cdots=x_n\\)时，取等号。

证明中还用到了**二项式定理**，即
\\[(a+b)^n = \sum_{k=0}^{n} \binom{n}{k} a^{n-k} b^k\\]
其中\\(\binom n k = \frac{n!}{(n-k)!k!}\\)。

首先，将不等式变形为
\\[(c+d)^n\left[\frac{a^{n+1}}{c^n}+\frac{b^{n+1}}{d^n}\right] \geq (a+b)^{n+1}\\]
将左边的二项式展开，
\begin{align}
左边 &= \sum_{k=0}^n \binom n k c^{n-k}d^k \left[\frac{a^{n+1}}{c^n}+\frac{b^{n+1}}{d^n}\right]\\\\
&= \sum_{k=0}^n \binom n k \left(\frac{a^{n+1}d^k}{c^k} + \frac{b^{n+1}c^{n-k}}{d^{n-k}}\right)
\end{align}
定义\\(\alpha_k=\frac{a^{n+1}d^k}{c^k}\\)，\\(\beta_k=\frac{b^{n+1}c^{n-k}}{d^{n-k}}\\)。将求和式中的两项错开相加，可得
\begin{align}
左边&=\sum_{k=0}^n(\alpha_k+\beta_k)\\\\
&= \binom n 0 \alpha_0 + \binom n 1 \alpha_1 + \binom n 2 \alpha_2 + \cdots + \binom n n \alpha_n \\\\
&\phantom{{}=\binom n 0 \alpha_0} + \binom n 0 \beta_0 + \binom n 1 \beta_1 + \cdots + \binom n {n-1} \beta_{n-1} + \binom n n \beta_n \\\\
&= \alpha_0 + \sum_{k=1}^{n} \left[\binom n k\alpha_k + \binom n {k-1}\beta_{k-1}\right] + \beta_n
\end{align}
使用AM--GM不等式，
\begin{align}
\binom n k\alpha_k + \binom n {k-1}\beta_{k-1}
&\geq \left[\binom n k + \binom n {k-1}\right]\sqrt[\binom n k + \binom n {k-1}]{\alpha_k^{\binom n k}\beta_{k-1}^{\binom n {k-1}}}
\end{align}
当且仅当\\(\alpha_k=\beta_{k-1}\\)时，取等号。
根式下方
\begin{align}
\alpha_k^{\binom{n}{k}}\beta_{k-1}^{\binom n {k-1}}
&= \left(\frac{a^{n+1}d^k}{c^k}\right)^{\frac{n!}{(n-k)!k!}}
   \left(\frac{b^{n+1}c^{n-k+1}}{d^{n-k+1}}\right)^{\frac{n!}{(n-k+1)!(k-1)!}}\\\\
&= a^{\frac{(n+1)!}{(n-k)!k!}} \left(\frac{d}{c}\right)^{\frac{n!}{(n-k)!(k-1)!}}
   b^{\frac{(n+1)!}{(n-k+1)!(k-1)!}}\left(\frac{c}{d}\right)^{\frac{n!}{(n-k)!(k-1)!}}\\\\
&= a^{(n+1-k)\binom{n+1}{k}} b^{k\binom{n+1}{k}}
\end{align}
再根据二项式系数的恒等式\\(\binom{n}{k} + \binom{n}{k-1} = \binom{n+1}{k}\\)得
\\[
\binom n k\alpha_k + \binom n {k-1}\beta_{k-1}
\geq \binom{n+1}k \sqrt[\binom {n+1}{k}]{\alpha_k^{\binom n k}\beta_{k-1}^{\binom n {k-1}}}
=\binom{n+1}k a^{n+1-k}b^k
\\]
在要证明的不等式中，我们得到
\begin{align}
左边 &= \alpha_0 + \sum_{k=1}^{n} \left[\binom n k\alpha_k + \binom n {k-1}\beta_{k-1}\right] + \beta_n\\\\
&\geq a^{n+1} + \sum_{k=1}^n \binom{n+1}{k} a^{n+1-k}b^k + b^{n+1}\\\\
&= \sum_{k=0}^{n+1} \binom{n+1}{k} a^{n+1-k}b^k\\\\
&= (a+b)^{n+1}\\\\
&= 右边
\end{align}
取等号的充分必要条件是\\(\alpha_k=\beta_{k-1}\\)，\\(k=1,2,\ldots,n\\)，即
\\[\frac{a^{n+1}d^k}{c^k} = \frac{b^{n+1}c^{n-k+1}}{d^{n-k+1}}\\]
该式成立的充分必要条件是\\(\frac a c = \frac b d\\)。

## 点评

这个证明方法我当时是凑出来的。将求和式中的项错开一位相加，再利用AM--GM不等式，得到的恰好是不等式的另一边，我也不知道为什么会有这样的巧合。

用数学归纳法可以将该不等式扩展到任意多项，即
\\[\frac{x_1^{n+1}}{y_1^n} + \cdots + \frac{x_m^{n+1}}{y_m^n} \geq \frac{(x_1 + \cdots + x_m)^{n+1}}{(y_1+\cdots+y_m)^n}\\]
其中\\(x_1,\ldots,x_m,y_1,\ldots,y_m>0\\)，\\(n\\)为正整数。取等号的充分必要条件是\\(\frac{x_1}{y_1} = \cdots = \frac{x_m}{y_m}\\)。

证明完了之后查了一下，\\(n=2\\)的情况可以用Cauchy不等式的证明。我这里给出的初等证明适用于任意正整数\\(n\\)。有其他的证明可以处理\\(n\\)为实数。可以想象\\(n=\frac12\\)也是比较有意思的情况。

## 应用实例

例1a. 已知 \\(x,y>0\\) 且 \\(\frac{x^2}{4} + \frac{y^2}{9} = 1\\)，求 \\(2x+y\\) 的最大值。

解：
\\[1=\frac{x^2}{4} + \frac{y^2}{9} = \frac{(2x)^2}{16} + \frac{y^2}{9}\geq \frac{(2x+y)^2}{25} \\]
因此\\(2x+y \leq \sqrt{25} = 5\\)。

说明：此题往往采用三角换元的方式求解，也可以用Cauchy不等式。

例1b. 已知 \\(x,y>0\\) 且 \\(\frac{2}{x^2} + \frac{1}{y^2} = 1\\)，求 \\(2x+y\\) 的最小值。

解：
\\[1 = \frac{2}{x^2} + \frac{1}{y^2} = \frac{2^3}{(2x)^2} + \frac{1}{y^2} \geq \frac{(2+1)^3}{(2x+y)^2}\\]
因此\\(2x+y\geq\sqrt{27}=3\sqrt3\\)。

例2. 在\\(x=0\\)处有一点电荷，带正电，电荷量为\\(+q\\)；在\\(x=1\\) 处有一点电荷，带负电，电荷量为\\(-q\\)。问在 \\(0\<x\<1\\) 中，哪一点的电场强度最小？最小是多少？

解：根据库仑定律，电场强度为
\\[E=\frac{kq}{x^2} + \frac{kq}{(1-x)^2} \geq \frac{kq(1+1)^3}{(x+1-x)^2} = 8kq\\]
取等号的充分必要条件是\\(\frac{1}{x} = \frac{1}{1-x}\\)，即\\(x=\frac12\\)。

例3. 已知\\(a,b,c>0\\) 且\\(a+b+c = 1\\)，求\\(\frac 1{a+b} + \frac 1{b+c} + \frac 1{c+a}\\) 的最小值。

解：
\\[\frac 1{a+b} + \frac 1{b+c} + \frac 1{c+a} \geq \frac{(1+1+1)^2}{2(a+b+c)} = \frac{9}{2}\\]

## 附：AM--GM不等式的证明

高中数学讲授\\(n=2\\)时的特殊情况。证明的办法很简单，
根据\\[(\sqrt a - \sqrt b)^2 \geq 0\\]得\\[a - 2\sqrt{ab} + b \geq 0\\]
移项后两边除以\\(2\\)即得\\[\frac{a+b}{2} \geq \sqrt{ab}\\]
称为**基本不等式**。

一般的情况需要一些巧思。下面附一个利用数学归纳法证明AM--GM不等式的办法。这个办法是我在网上找到的。

定义
\begin{align}
A_n &= \frac{x_1+\cdots+x_n}{n}\\\\
G_n &= \sqrt[n]{x_1\cdots x_n}
\end{align}
以下两个递推式在证明中将用到
\begin{align}
(n+1)A_n &= nA_n + x_{n+1}\\\\
G_{n+1}^{n+1} &= G_n^nx_{n+1}
\end{align}

要证明的命题是“\\(A_n\geq G_n\\)对任意\\(x_1,\ldots,x_n>0\\)都成立，并且取等号的充分必要条件是\\(x_1=\cdots=x_n\\)”，对正整数\\(n\\)进行归纳。

当\\(n=1\\)时，\\(A_1=x_1=G_1\\)，命题显然成立。
假设当\\(n=k\\)时，命题成立，下面将要证明不等式\\(A_{k+1}\geq G_{k+1}\\)。

只须证明\\[(k+1)A_{k+1} \geq (k+1)G_{k+1}\\]
只须证明\\[kA_k + x_{k+1} \geq (k+1)G_{k+1}\\]
只须证明\\[kA_k + x_{k+1} + (k-1)G_{k+1} \geq 2kG_{k+1}\\]

利用归纳假设，有\\(A_k\geq G_k\\)。这允许我们使用\\(k\\)个数的AM--GM不等式：
\begin{align}
x_{k+1} + (k-1)G_{k+1}
&= x_{k+1} + \underbrace{G_{k+1} + \cdots + G_{k+1}}\_{k-1} \\\\
&\geq \sqrt[k]{x_{k+1}G_{k+1}^{k-1}}
\end{align}
于是有：
\begin{align}
kA_k + x_{k+1} + (k-1)G_{k+1}
&\geq kG_k + \sqrt[k]{x_{k+1}G_{k+1}^{k-1}} \\\\
&\geq 2k\sqrt[2k]{G_k^{k}x_{k+1}G_{k+1}^{k-1}}\\\\
&= 2kG_{k+1}
\end{align}
第二个不等号使用了基本不等式（即\\(n=2\\)的情况，前面已经证明）。
取等号的必要条件是 \\(x_1=\cdots=x_k\\) 和 \\(x_{k+1}=G_{k+1}\\)。这已经要求\\(x_1=\cdots=x_k=x_{k+1}\\)。代入不等式可知这个条件也是充分的。
这说明\\(A_{k+1}\geq G_{k+1}\\)，当且仅当\\(x_1=\cdots=x_{k+1}\\)时取等号。根据数学归纳法，要证明的命题对一切正整数\\(n\\)都成立。

## 附：推广到实数指数的情况

我简单思考了一下，AM--GM不等式似乎并不能用于处理\\(n\\)为实数的情况，因为广义二项式系数可能是负的，那么证明中的关键步骤就不能成立。幸运的是，使用微积分工具可以不需要太多技巧就能证明这个不等式，而且可以轻松地推广到实数指数的情况。

我们只需要证明
\\[1+\frac{y^{n+1}}{x^n} \geq \frac{(1+y)^{n+1}}{(1+x)^n}\\]
其中\\(x,y,n>0\\)。在上式中代换\\(y=\frac ba\\)和\\(x=\frac dc\\)就能得到原来的不等式。
设\\[f(x)=(1+x)^n\left(1+\frac{y^{n+1}}{x^n}\right)=(1+x)^n + \left(\frac{1}{x}+1\right)^n y^{n+1}\\]
对\\(x\\)求导，得
\begin{align}
f'(x)
&= n(1+x)^{n-1}+n\left(\frac{1}{x}+1\right)^{n-1}(-\frac{1}{x^2})y^{n+1} \\\\
&= n(1+x)^{n-1}\left[1-\left(\frac{x}{y}\right)^{n+1}\right]
\end{align}
注意条件\\(n>0\\)，当\\(x\<y\\)时，\\(\left(\frac xy\right)^{n+1} < 1\\)，因此\\(f'(x)<0\\)。类似地，当\\(x\>y\\)时，\\(f'(x)>0\\)。由此可知，\\(f(x)\\)在\\(x=y\\)处取得最小值，最小值为
\\[f_\min = f(y) = (1+y)^{n+1}\\]
由此可知，对任意\\(x>0\\)都有
\\[f(x)=(1+x)^n\left(1+\frac{y^{n+1}}{x^n}\right) \geq (1+y)^{n+1}\\]
当且仅当\\(x=y\\)时，取等号。
我们还可以进一步推广到\\(-1\<n\<0\\)的情况，此时不等号反向。\\(n < -1\\)的情况可以转换为\\(n>0\\)的情况，不再探讨。

除此以外还有使用其他著名公式的证明。这些公式我并没有系统学过，用起来也需要一些技巧。好在微积分能够普适地、机械化的处理问题。这就是它的优势所在。
