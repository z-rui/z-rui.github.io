---
date: 2015-05-06T13:30:00+08:00
title: "一道几何题的证明"
---

这是一道来自高中数学的几何题。值得纪念一下，因为它有好几种证明的办法。有的办法简单，有的办法麻烦。

{{<figure src="/media/geometry-1.svg">}}

问题是这样的：如图，矩形的边长\\(AB=a, BC=b\\)，而\\(P\\)是\\(CD\\)上的动点。问：\\(\angle APB\\)什么时候最大？并说明理由。

根据直觉可知，当\\(P\\)是\\(CD\\)的中点的时候，\\(\angle APB\\)应该最大。

直觉虽然有重要的提示作用，但是有时也不很可靠，所以并不能作为推理的依据。现在让我们忘记直觉，按照一般的分析办法来处理这个问题。

<!--more-->

设\\(DP=x\\;(0 \leq x \leq a)\\)，则\\(CP=a-x\\)。根据勾股定理：

\\[\eqalign{
AP^2 &= x^2 + b^2, \cr
BP^2 &= (a-x)^2 + b^2.
}\\]

根据余弦定理：
\\[\eqalign{
\cos\angle APB
&= {AP^2+BP^2-AB^2 \over 2AP\cdot BP} \cr
&= {x^2+b^2+(a-x)^2+b^2-a^2 \over 2\sqrt{x^2+b^2}\sqrt{(a-x)^2+b^2}} \cr
&= {x^2-ax+b^2 \over \sqrt{(x^2+b^2)(x^2-2ax+a^2+b^2)}}.
}\\]

因为余弦函数在\\([0, \pi]\\)内是单调递减的，所以要使\\(\angle APB\\)最大，只须\\(\cos\angle APB\\)取最小值即可。

这样，问题转化为一元函数的最值问题。高中数学提供了导数这个有用的工具，可以有效地求出各种初等函数的最值。方法如下：

设最后一个等号右边的表达式为\\(f(x)\\)，首先计算要导数。

设
\\[\eqalign{
u &= x^2-ax+b^2, \cr
v &= (x^2+b^2)(x^2-2ax+a^2+b^2) \cr
  &= x^4-2ax^3+(a^2+2b^2)x^2-2ab^2x+(a^2+b^2)b^2. \cr
}\\]
那么
\\[f'(x) = \left(u\over\sqrt{v}\right)' = {u'\sqrt{v} - u(\sqrt{v})' \over v} = {u'v - u{v'\over2} \over v^{3\over2}},\\]
其中
\\[\eqalign{
u' &= 2x-a, \cr
v' &= 4x^3-6ax^2+(2a^2+4b^2)x-2ab^2, \cr
u'v &= 2x^5 -5ax^4 +4(a^2+b^2)x^3 -a(a^2+6b^2)x^2 +2(2a^2+b^2)b^2x -ab^4 -a^3b^2, \cr
u{v'\over2} &= 2x^5 -5ax^4 +4(a^2+b^2)x^3 -a(a^2+6b^2)x^2 +2(\phantom{0}a^2+b^2)b^2x -ab^4, \cr
u'v-u{v'\over2} &= 2a^2b^2x - a^3b^2 = a^2b^2(2x-a).
}\\]

一般来说，教科书通常是轻描淡写地就把计算结果给求出来了。实际上这一步计算是很繁的，对初等代数的运算能力是一个巨大的考验。借助计算机代数系统(CAS)来求导数可能会是一个好主意。

幸运的是导函数分子中2次以上的项全都消掉了，由此得到唯一的驻点\\(x={a\over2}\\)。事实上，当\\(0 < x < {a\over2}\\)时，\\(f'(x) < 0\\)，\\(f(x)\\)单调递减；当\\({a\over2} < x < a\\)时，\\(f'(x) > 0\\)，\\(f(x)\\)单调递增。这样，我们无需计算边界处的函数值就可知，当\\(x={a\over2}\\)时，\\(f(x)\\)取得最小值。

综上所述，当\\(P\\)是\\(CD\\)中点时，\\(\angle APB\\)最小。

---

另一种做法是不用余弦定理而改用其他的三角恒等式。根据图形可知\\(\tan\angle PAD = {x\over b},\\,\tan\angle PBC = {a-x\over b}\\)。所以根据余切公式
\\[\cot\angle APB = {{1-{x\over b}{a-x\over b}} \over {x\over b}+{a-x\over b}} = {x^2-ax+b^2 \over ab}.\\]

余切函数在\\((0, \pi)\\)上也是单调递减的。所以问题转化为求上述函数（记为\\(f(x)\\)）的最小值。计算导数
\\[f'(x) = {2x-a \over ab}\\]

得唯一驻点\\(x={a\over2}\\)。通过类似的讨论可知这是最小值点。所以当\\(P\\)是\\(CD\\)中点时，\\(\angle APB\\)最小。

不计算导数也无所谓。因为余切函数是关于\\(x\\)的二次函数，且开口向上，自然在对称轴处取得最小值。

由此可见计算切函数要比计算余弦函数在计算上来得方便地多。

如果知道反正切函数，那么
\\[\angle APB = \arctan{x\over b} + \arctan{a-x\over b}.\\]

根据反正切函数的凸凹性，得
\\[{\arctan{x_1} + \arctan{x_2}\over 2} \leq \arctan{x_1+x_2\over 2} \quad \forall x_1,x_2 \in (0,+\infty). \\]
于是
\\[\angle APB \leq 2\arctan{a \over 2b},\\]
当且仅当\\(x={a\over2}\\)等号成立，此时\\(\angle APB\\)取得最大值。这种方法更简单了，只不过超出了高中数学的范围。

---

除了微积分以外，还可以使用算术-几何平均不等式来求解最小值问题。

算术-几何平均不等式说的是正数的几何平均数不大于算术平均数，即\\[\sqrt{uv} \leq {u+v \over 2},\\]当且仅当\\(u=v\\)时，取等号。这个不等式在高中数学课本中被称为“基本不等式”，其重要性可见一斑。

用不等式求最值的原理是：如果不等式的一边是定值，并且等号能够成立，那么这个定值一定就是另一边的最值。

对于刚才求出来的余切函数
\\[\cot\angle APB = {b^2 - x(a-x)\over ab}.\\]

使用基本不等式很容易得到：\\(x(a-x) \leq \left(a-x\over2\right)^2\\)。所以取\\(x=a-x\\)也就是\\(x={a\over2}\\)时，余切取得最小值。

如果是余弦函数，情况会复杂一些，但是仍然可以使用基本不等式来求出最小值。

将基本不等式两边平方并移项化简，可得\\(uv \leq {u^2+v^2 \over 2}\\)。两边同加上\\(u^2+v^2 \over 2\\)，则得
\\[u^2+v^2 \geq {(u+v)^2 \over 2}.\\]
这个不等式也将在求最小值的过程中起到作用。

刚才已经得到\\[ \cos\angle APB = {x^2+(a-x)^2+2b^2-a^2 \over 2\sqrt{x^2+b^2}\sqrt{(a-x)^2+b^2}}. \\]

根据上面提到的两个不等式，有
\\[\eqalign{
2\sqrt{x^2+b^2}\sqrt{(a-x)^2+b^2} &\leq x^2+(a-x)^2+2b^2 \cr
x^2+(a-x)^2 &\geq {[x+(a-x)]^2 \over 2} = {a^2\over2}.
}\\]

所以
\begin{align}
\cos\angle APB &\geq {x^2+(a-x)^2+2b^2-a^2 \over x^2+(a-x)^2+2b^2} \\\\ 
&= 1 - {a^2 \over x^2+(a-x)^2+2b^2} \\\\ 
&\geq 1-{a^2 \over {a^2\over2}+2b^2} \\\\ 
&= {4b^2-a^2 \over 4b^2+a^2}.
\end{align}

当且仅当\\(x=a-x\\)即\\(x={a\over2}\\)时，两个不等式同时取等号，\\(\cos\angle APB\\)取得最小值。

使用不等式求最值的好处是，相比求导而言，计算更容易些，并且最值点和最值同时求得，无需多余的代入计算。但是不等式的应用范围不如导数广：只有一些特殊的表达式可以使用不等式求出最值，而且这些特殊的形式也不一定一眼能够看得出来，有时候需要一定的技巧。

---

最后，抛弃代数，用纯几何的办法，也可以证明这个结论。

{{<figure src="/media/geometry-2.svg">}}

如图，取\\(CD\\)的中点\\(E\\)，作圆弧\\(\overset{\large\frown}{AEB}\\)。假设\\(P\\)不是\\(CD\\)的中点，那么设\\(\overset{\large\frown}{AEB}\\)和\\(AP\\)的交点是\\(Q\\)。连结\\(BQ\\)。根据圆周角定理，\\(\angle AQB = \angle AEB.\\)另一方面，因为\\(\angle AQB\\)是\\(\triangle PQB\\)的外角，有\\(\angle AQB > \angle APB.\\)所以\\[\angle APB < \angle AEB.\\]所以只有\\(P\\)是\\(CD\\)中点的时候，\\(\angle APB\\)才最大。

纯几何的办法，实质上没有任何的计算，所以可以算是最简单的一种做法。这题能用几何做法很可能是凑巧（实际上这是一道经典的最值问题，几何做法也是经典解法）。另外，还需要解题者事先知道\\(P\\)在\\(CD\\)中点这样的结论，这需要靠直觉。只有经验丰富的人才能识别出这个问题，然后给出这样一种巧妙的解法。
