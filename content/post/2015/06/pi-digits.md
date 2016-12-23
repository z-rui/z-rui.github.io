---
date: 2015-06-05T19:47:00+08:00
title: "计算圆周率至任意精度"
---

下面这段Python 3代码可以产生圆周率π的任意多个有效数字。

```python
def pidigits():
    q, r, t, u, i = 1, 180, 60, 168, 2
    while True:
        y = r // t
        yield y
        q, r, t, u, i = 10*q*i * (2*i-1), 10*u * (q * (5*i-2) + r - y*t), t*u, u + 54*(i+1), i+1
```

这是一个永不终止的生成器，每次产生一个十进制的有效数字，依次是3, 1, 4, 1, 5, 9, 2, 6, ...

<!--more-->

自古以来有很多人试图计算出圆周率的准确值。古典的计算圆周率的方法是作圆的外接和内切多边形，通过计算多边形的面积，得到圆周率的近似值。南北朝的数学家祖冲之计算得到了7位有效数字：

> 古之九数，圆周率三，圆径率一，其术疏舛。自刘歆、张衡、刘徽、王蕃、皮延宗之徒各设新率，未臻折衷。宋末，南徐州从事史祖冲之更开密法。以圆径一亿为一丈。圆周盈数三丈一尺四寸一分五厘九毫二秒七忽；肭数三丈一尺四寸一分五厘九毫二秒六忽，正数在盈肭二限之间。密率：圆径一百一十三，圆周三百五十五。约率：圆径七,圆周二十二。”   
> --《隋书》

自从微积分发明以来，出现了很多新的计算圆周率的方法，广为人知的有[莱布尼茨级数]：
\\[{\pi\over4} = \sum\_{n=0}^\infty {(-1)^n \over 2n+1} = 1 - {1\over3} + {1\over5} - {1\over7} + \cdots\\]

现在，但凡学过微积分的人都可以轻松地证明它：在泰勒级数
\\[\arctan x = x - {x^3\over3} + {x^5\over5} - {x^7\over7} + \cdots\\]
取\\(x = 1\\)立即得到。（注意：还要验证所得的级数是收敛的。）莱布尼茨本人的证明见[这里][莱布尼茨证明]。

这个公式虽然很漂亮，但是计算的效率实际上很低。我头一次见到这个公式的时候，因为不懂无穷级数的理论，感到很神奇，拿了一台电子计算器就不停地输入其中的各项。算了很久，也没有得到什么准确的值，离3.14这个数值都还差得远。

有很多办法可以[加速级数收敛]。例如，做一些三角恒等变换可以得到[Machin公式]
\\[{\pi\over4} = 4\arctan{1\over5} + \arctan{1\over239}\\]
手工计算圆周率的记录是[William Shanks]，他用了大约20年的时间计算出了707位有效数字，用的就是这个公式。

我们也可以着手改进莱布尼茨级数。[欧拉变换]可以用来加速其收敛。具体形式为
\\[\sum\_{n=0}^\infty (-1)^n a\_n = \sum\_{n=0}^\infty (-1)^n {\Delta^n a\_0 \over 2^{n+1}}\\]
其中\\(\Delta\\)是所谓的[前向差分算子]
\\[\Delta^n a\_0 = \sum\_{k=0}^n (-1)^k {n\choose k}a\_{n-k}\\]

对于莱布尼茨级数而言，\\(a\_n = {1\over2n+1}\\)。利用二项式系数的复杂的计算技巧可以证明(见文末)
\\[\Delta^n a\_0 = \sum\_{k=0}^n (-1)^k {n\choose k}{1\over 2(n-k)+1} = (-1)^n{n!\,2^n\over(2n+1)!!}\\]
其中双阶乘记号\\((2n+1)!! := (2n+1)\times(2n-1)\times(2n-3)\times\cdots\times3\times1\\)。

代入欧拉变换的表达式得
\\[{\pi\over4} = \sum\_{n=0}^\infty {(-1)^n \over 2n+1} = \sum\_{n=0}^\infty {n!\over2\cdot(2n+1)!!}\\]

由此我们得到了一个新的无穷级数，它收敛到同一个值，但是收敛速度远远地提高了。把上式两边乘以4，就得到π的计算公式
\\[\pi = \sum\_{n=0}^\infty 2\cdot {n!\over(2n+1)!!} = 2\times{1\over3} + 2\times{1\times2\over3\times5} + 2\times{1\times2\times3\over3\times5\times7} + \cdots \\]

提取其中的公因式，就可以得到一个重要的公式\\[\pi = 2 + {1\over3} \times \left(2 + {2\over5} \times \left(2 + {3\over7} \times \left(\cdots 2+{i\over 2i+1} \times \cdots \right) \right) \right) \tag{1}\\]

这个公式有特别的意义。为说明这一点，我们观察在十进制小数的表示方法中，圆周率表示成
\\[\pi = 3 + {1\over10} \times \left(1 + {1\over10} \times \left(4 + {1\over10} \times \left(1 + {1\over10} \times \cdots \right) \right) \right) \\]

比较一下两个式子，可以发现，其实第一个式子可以看作圆周率在一个特殊的进位制\\({\cal B} = \left({1\over3}, {2\over5}, {3\over7}, \dots\right)\\)中的表示。[Spigot算法]就是由此而来。Spigot算法首先在\\(\cal B\\)进制下把圆周率表示成足够多的位数（每一位都是2而已），然后通过进制转换的方式，逐渐地将它转换成十进制数。尽管这个进制比较特殊，转换方法也稍有奥妙，但基本原理和普通的进制转换是一样的：每次提取出整数部分，然后只留下小数部分，再把这个数乘以10。具体请参考相关文献。

Spigot算法非常有趣，它在计算每一位数字的时候事实上都没有用到之前算出来的任何位数字（但是它仍然需要存储其他的信息，只靠有限的存储空间是无法生成不循环的数字序列的）。它还能被编码成十分晦涩的形式，下述代码来自Dik Winter and Achim Flammenkamp：

```c
a[52514],b,c=52514,d,e,f=1e4,g,h;
main(){for(;b=c-=14;h=printf("%04d", e+d/f))
for(e=d%=f;g=--b*2;d/=g)d=d*b+f*(h?a[b]:f/5),a[b]=d%--g;
```

它真的是可以直接编译的C语言程序！运行的结果是输出了圆周率的前15000个有效数字。

使用spigot算法计算π在十进制下的\\(n\\)个有效数字，只需要取\\(\cal B\\)进制下\\(\left\lfloor10n\over3\right\rfloor+1\\)位数字就足够了。所以，如果现在的你想像祖冲之那样把圆周率计算到7位有效数字，也只需要取\\(\cal B\\)进制下的25位数。在计算的过程中，只有整数的四则运算，而没有什么复杂的开方之类的计算。虽然我没有试过，但是我估计，即便完全手工计算，用一天的时间大概是足够了。

Spigot算法有一个本质的问题是你必须预先指定将要计算多少个有效数字。这主要是因为在做进制转换的时候该算法是从最低位向最高位操作的。Gibbons\[[1][Gibbons]\]给出了一种称为Streaming的算法，它无需预先知道所需要计算的位数，所以原则上可以计算任意多个有效数字。

Gibbons提出的算法非常具有启发性：他把上述计算公式\\((1)\\)表示成如下的形式：
\\[\pi = \left(2 + {1\over3} \times\right) \left(2 + {2\over5} \times\right) \left(2+{3\over7} \times\right) \cdots \left(2 + {i\over 2i+1} \times\right) \cdots \\]

其中的每一项\\(T\_i = \left(2 + {i\over 2i+1} \times\right)\\)都是一个分式线性映射\\[T\_i:\;x\to{q\_ix + r\_i \over s\_ix + t\_i}\\]其中\\(q\_i = i, r\_i = 2(2i+1), s\_i = 0, t\_i = 2i+1\\)，当然，你给它们同时乘上或除以某一个常数也是无所谓的。

分式线性映射可以用矩阵表示为\\[T\_i=\pmatrix{q\_i&r\_i\cr s\_i&t\_i}\\]不难证明，分式线性映射的复合可以用矩阵的乘法来表示。

Gibbons说明了这些映射的复合在\\([3,4]\\)上收敛。所以取其中的任意一个数代入即可求出π。当然，我们没法求出无穷多个映射的复合。只需要求出所谓“部分复合映射”（类比于无穷级数的部分和）\\[T=\pmatrix{q&r\cr s&t}=\prod_{i=1}^{n}\pmatrix{q\_i&r\_i\cr s\_i&t\_i}\\]代入一个数值，则它至少应该具备一定的精度。如果精度不够，则我们再多复合几个映射，如此即可。

怎样检查精度是否足够？我们只需计算出部分复合映射的整数部分，即\\[n=\left\lfloor qx+r\over sx+t \right\rfloor\\]分别代入\\(x=3\\)和\\(x=4\\)，如果得到的整数部分是一样的话，因为映射是单调的，可知整数部分必然就是这个数字不会再有其他变化，因此我们可以输出这一位数。

为了得到小数点后面的那一位数字，我们把这个数减去\\(n\\)，再乘以10，这样我们又只需要求出整数部分就可以了。要达到这一目的，只要左乘矩阵\\(\left({10\atop0}{-10n\atop1}\right)\\)就可以了，因为它是\\(\left(n+{1\over10}\times\right)\\)的反变换。然后继续检查这时候的整数部分，精度是否足够？这就和刚才一样了。如此循环就可以不断地求出π的有效数字。

这个算法的精妙之处在于它还能运用于圆周率的其他[计算公式][圆周率公式]上，例如Lambert连分式
\\[\pi = {4\over1+{1^2\over3+{2^2\over5+{3^2\over7+\cdots}}}}\\]
和Gosper序列
\\[\pi = 3 + {1\times1\over3\times4\times5} \times \left(8 + {2\times3\over3\times7\times8} \times \left(\cdots 5i-2 + {i(2i-1) \over 3(3i+1)(3i+2)}\times\cdots\right)\right)\\]

因为它们都可以表示成无穷个分式线性映射的复合。当然，不同的计算公式的收敛速度不一样。Gibbons指出，Gosper序列是这三者中收敛速度最快的。快到什么地步呢？每复合一个映射，就可以保证当前的整数部分一定是准确的，所以我们无需代入区间的两个端点去检查精确度了。

不过，Gibbons表示这一点实质上未能得到证明，因此只是一个猜想，还有待于勤恳的读者加以证明。我虽然没有本事证明这个猜想，但是我提出了另一个猜想，那就是Gosper序列对于\\(x=0\\)也是收敛的。如果这个猜想成立，那么计算部分复合映射的整数部分就简化为\\[n=\left\lfloor r\over t\right\rfloor\\]
在使用Gosper序列的算法中，\\(q\\)是一个挺大的数，如果取\\(x=0\\)的话，可以省去不少的计算量。

同样，我并没有本事证明这个猜想，但是我做了个实验，在运用这个猜想时求了π的100000位有效数字，和[MPFR高精度运算函数库](http://www.mpfr.org/)所得的结果是一样的。文章开头给出的Python代码，大约就是基于这样的原理。那个代码还针对矩阵运算做了其他的优化：利用\\(s\equiv0\\)这个条件，可以简化一些表达式。

Gibbon还指出，这个算法还有值得优化的地方。例如可以在恰当的时候约去\\((q, r, t)\\)的公约数，以减少它们的大小，提高运算速度。不过，求大整数的公约数也不是一件省时省力的事情。为了保持代码的简单，我就没有写在其中。在一个使用了[GMP高精度运算函数库](http://www.gmplib.org)的C版本中，我加上了这一项优化，确实能提高运算速度。

如Gibbons所言，这样的算法并非是最快的。这主要是因为所计算出来的分式线性映射的系数会迅速地变成特别大的整数。即便是我用C配合GMP编写的程序，最终也没有MPFR自带的求π的函数的速度快。但是这个算法很具有启发性，让我以一种全新的视角看待无穷级数、连分数这样的构造。把它们转换成分式线性映射、分式线性映射又用矩阵表示、计算有效数字转化为提取整数部分……这些转化的思想都妙不可言。

话说回来，MPFR用的是什么算法呢？经过查看它的[文档](http://www.mpfr.org/algo.html)可知，求π用的是[高斯-勒让德算法]。是的，听着名字就是这么霸气。它的原理和这里所说的算法都不同，并不是基于某个无穷级数，而是基于算术-几何平均数的一些性质。此外，它是一种迭代算法，每次迭代都需要用到前面的计算结果，而不是每一位有效数字都是单独得到的。

## 前向差分算子等式的证明

文中提及一个等式
\\[\sum\_{k=0}^n (-1)^k {n\choose k}{1\over 2(n-k)+1} = (-1)^n{2^n n!\over (2n+1)!!}\\]
这里我给予证明。正如文中所说，需要用到和二项式系数相关的运算技巧，所以我先给出二项式系数的定义：设\\(n, k\\)为自然数，那么\\[{n\choose k} = {n!\over k!\,(n-k)!}\\]

使用定义不难证明下面的等式成立：
\\[{n\over n-k}{n-1\choose k} = {n\choose k}\\]

此外还有二项式定理
\\[(a+b)^n = \sum\_{k=0}^n {n\choose k} a^{n-k} b^k\\]
对于任意的自然数\\(n\\)都成立。可以用数学归纳法证明。下面的证明中我们需要用到一种特殊情况。取\\(a=1, b=-1\\)，即可证明\\[\sum\_{k=0}^n (-1)^k {n\choose k} = 0\\]

考察一种一般的情况
\\[F(m) := \sum\_{k=0}^{n-m} (-1)^k {n-m\choose k}{1\over 2(n-k)+1} = {?}\\]
如果求出了\\(F(m)\\)的一般表达式，那么只要取\\(m=0\\)即可。

接下来是一些枯燥的代数运算
\\\[\eqalign{
F(m)
&= \sum\_{k=0}^{n-m} (-1)^k {n-m\choose k}{1\over 2(n-k)+1} \cr
&= {(-1)^{n-m}\over2m+1} + \sum\_{k=0}^{n-m-1} (-1)^k {n-m-1\choose k}{n-m\over n-m-k}{1\over 2(n-k)+1} \cr
&= {(-1)^{n-m}\over2m+1} + {1\over2m+1}\sum\_{k=0}^{n-m-1} (-1)^k {n-m-1\choose k}\left[{n-m\over n-m-k}-{2(n-m)\over 2(n-k)+1}\right] \cr
&= {1\over2m+1}\left[(-1)^{n-m} + \sum\_{k=0}^{n-m-1} (-1)^k {n-m\choose k} - 2(n-m)\sum\_{k=0}^{n-m-1} (-1)^k {n-m-1\choose k}{1\over 2(n-k)+1}\right] \cr
&= {1\over2m+1}\left[\sum\_{k=0}^{n-m} (-1)^k {n-m\choose k} - 2(n-m)F(m+1) \right] \cr
&= -{2(n-m)\over 2m+1} F(m+1)
}\\\]

这是一个递推的表达式。反复运用得
\\[\eqalign{
F(0)
&= {-2n\over1}\cdot{-2(n-1)\over3}\cdots{-2(n-n+1)\over2(n-1)+1}F(n)\cr
&= {-2n\over1}\cdot{-2(n-1)\over3}\cdots{-2(n-n+1)\over2(n-1)+1}\cdot{1\over2n+1}\cr
&= (-1)^n{n!\,2^n\over(2n+1)!!}
}\\]

证明完毕。

[莱布尼茨级数]:		http://en.wikipedia.org/wiki/Leibniz_formula_for_%CF%80
[莱布尼茨证明]:		https://proofwiki.org/wiki/Leibniz's_Formula_for_Pi
[加速级数收敛]:		http://en.wikipedia.org/wiki/Series_acceleration
[Machin公式]:		http://mathworld.wolfram.com/MachinsFormula.html
[William Shanks]:	http://en.wikipedia.org/wiki/William_Shanks
[欧拉变换]:		http://mathworld.wolfram.com/EulerTransform.html
[前向差分算子]:		http://en.wikipedia.org/wiki/Forward_difference_operator
[Spigot算法]:		http://www.mathpropress.com/stan/bibliography/spigot.pdf
[Gibbons]:		http://web.comlab.ox.ac.uk/oucl/work/jeremy.gibbons/publications/spigot.pdf
[圆周率公式]:		http://mathworld.wolfram.com/PiFormulas.html
[高斯-勒让德算法]:	http://en.wikipedia.org/wiki/Gauss%E2%80%93Legendre_algorithm
