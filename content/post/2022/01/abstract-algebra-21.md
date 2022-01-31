---
title: 抽象代数习题(21--23) -- 整数和数论基础
date: 2022-01-27T22:20:39-08:00
---

《抽象代数》第二十一章到第二十三章主要研究整数的性质，因此和初等数论有比较大的重叠。我把它们的习题放在同一篇。

* 第二十一章 -- [整数]({{< ref "#整数" >}})
* 第二十二章 -- [分解质因数]({{< ref "#分解质因数" >}})
* 第二十三章 -- [数论基础]({{< ref "#数论基础" >}})

<!--more-->

# 整数

第二十一章讲的是一种最常见的整环：整数环 &Zopf;。整数环具备以下公理：

* 有序性：两个整数 a 和 b 比较大小，结果一定是 a < b, a = b, a > b 三者之一。并且不等号满足通常的传递性、同向可加性和正数可乘性。
* 良序性 (well-ordering property)：正整数集的非空子集必有最小元素。这个性质看起来是常识，并且在前面章节的许多证明中已经使用过了。由它还可以导出带余除法和数学归纳法原理。

事实上，满足这些性质的整环全都同构于 &Zopf;。可见整数环是一类非常特别的环。

## B. 有序整环的更多性质

设 A 是有序整环。证明下列命题对于任意 a, b, c &isin; A 都成立。

__B.1__ a² + b² ≥ 2ab

**证明** 有序整环中，任意元素的平方总是非负的。因此有 (a-b)² ≥ 0。

于是 a² - 2ab + b² ≥ 0。两边加上 2ab 即得要证的不等式成立。

---

__B.3__ a² + b² + c² ≥ ab + bc + ac

**引理**

1. 如果 a ≥ b 且 c ≥ d，那么 a + c ≥ b + d。可以根据习题 A.1 和 A.2 得到（略）。
2. 如果 a + a ≥ b + b，那么 a ≥ b。用反证法易证。

**证明** 仿照 B.1 可得三个不等式

1. a² + b² ≥ 2ab
2. b² + c² ≥ 2bc
3. a² + c² ≥ 2ac

根据引理 1，可以将三个不等式的两边分别相加，得 (a² + b² + c²) + (a² + b² + c²) ≥ (ab + bc + ac) + (ab + bc + ac)。
根据引理 2，得 a² + b² + c² ≥ ab + bc + ac。

---

__B.5__ 当 a,b > 1 时，a + b < ab + 1

**证明** 由已知条件得 a - 1 > 0, b - 1 > 0。因此 (a-1)(b-1) > 0。
即 ab - a - b + 1 > 0。两边加上 (a + b) 得 ab + 1 > a + b，即 a + b < ab + 1。

## E. 绝对值

对于任意有序整环，定义绝对值 \\(|a|\\) 为
\\[|a|=\cases{a & $a\ge 0$\cr -a & $a < 0$}\\]
使用这个定义证明下列命题。

__E.5__ \\(|a+b| \le |a| + |b|\\)

**证明一** 不失一般性，设 \\(|a|\le|b|\\)。分类讨论：

1. \\(a>0,b>0\\)，则 \\(a=|a|,b=|b|,a+b>0\\)。
2. \\(a>0,b<0\\)，则 \\(a=|a|,b=-|b|,a+b<0\\)。
3. \\(a<0,b>0\\)，则 \\(a=-|a|,b=|b|,a+b>0\\)。
4. \\(a<0,b<0\\)，则 \\(a=-|a|,b=-|b|,a+b<0\\)。

因为 \\(-|a|\le|a|\\\)，两边加上 \\(|b|\\) 得 \\(|b|-|a|\le |a|+|b|\\)。

* 对于 1、4，有\\(|a+b|=|a|+|b|\\)；
* 对于 2、3，有 \\(|a+b|=|b|-|a| \le |a|+|b|\\)。

综上所述，不论哪种情况总有 \\(|a+b|\le |a|+|b|\\)。

**证明二** 当 \\(b\ge0\\) 时，\\(|a|\le b\\) 当且仅当 \\(-b\le a\le b\\)（习题 E.4，证略）。

在上式中用做代换 \\(a\mapsto a+b\\) 和 \\(b\mapsto |a|+|b|\\) 得
\\[|a+b|\le |a|+|b| \iff -(|a|+|b|)\le a+b \le |a|+|b|\\]
右边的不等式的成立的，因为 \\(-|a|\le a\le|a|\\) 且 \\(-|b|\le b\le|b|\\)，根据同向不等式可以相加即得。

因此，要证明的不等式成立。

---

__E.8__ \\(|a|-|b|\le |a-b|\\)

**证明一** 如果 \\(|a|\le|b|\\)，则 \\(|a|-|b|\le0\le|a-b|\\)。

下面只考虑 \\(|a| > |b|\\) 的情况。分类讨论：

1. \\(a>0,b>0\\)，则 \\(a=|a|,b=|b|,a-b>0\\)。
2. \\(a>0,b<0\\)，则 \\(a=|a|,b=-|b|,a-b>0\\)。
3. \\(a<0,b>0\\)，则 \\(a=-|a|,b=|b|,a-b<0\\)。
4. \\(a<0,b<0\\)，则 \\(a=-|a|,b=-|b|,a-b<0\\)。

* 对于 1、3，有\\(|a-b|=|a|+|b|\ge|a|-|b|\\)；
* 对于 2、4，有 \\(|a-b|=|a|-|b|\\)。

综上所述，不论哪种情况总有 \\(|a-b|\ge |a|-|b|\\)，所以要证的不等式成立。

**证明二**

在 E.5 的不等式中做代换 \\(a\mapsto a-b\\) 得
\\[|a|\le|a-b|+|b|\\]
两边加上 \\(-|b|\\) 得
\\[|a|-|b|\le|a-b|\\]
证明完毕。

## F. 带余除法问题

证明 1--3，其中 k, m, n, q, r 表示整数。

__F.2__ 设 n > 0 且 k > 0。如果 q 是 m 除以 n 的商，且 q₁ 是 q 除以 k 的商，那么 q₁ 是 m 除以 nk 的商。

**证明** 根据已知条件有

1. m = nq + r
2. q = q₁k + r₁

其中的量都是整数，且 0 ≤ r < n，0 ≤ r₁ < k。消去 q 得

* m = n(q₁k + r₁) + r = q₁(nk) + (nr₁+r)

因为 r₁ < k，所以 k - r₁ > 0。又因为整数集中，大于 0 的最小元素是 1，所以 k - r₁ ≥ 1。于是有 r₁ ≤ k-1。
又 n > 0，所以 nr₁ ≤ nk(k-1)。两边加上 r 得 nr₁+r ≤ nk - (n-r)。因为 r < n，所以 -(n-r) < 0，因此 nr₁+r ≤ nk - (n-r) < nk。显然又有 nr₁+r ≥ 0，所以 nr₁+r 是 m 除以 nk 得余数。根据带余除法结果的唯一性可知 q₁ 是 m 除以 nk 的商。

# 分解质因数

第二十二章讲的是整数的分解质因数。这一章的主要内容是初等数论的基础知识，包括：

* 数的整除、因子、互质；最大公约数；质数和合数
* Bézout 引理、欧几里得引理[^1]、算术基本定理

## B. 最大公约数的性质

证明下列命题对任意整数 a, b, c 都成立。

__B.1__ 如果 a > 0 且 a | b，那么 gcd(a, b) = a。

**证明**

1. a | a 且 a | b。
2. 设 u 是整数且 u | a 且 u | b，那么 u | a。

以上两条符合最大公约数的定义，且 a > 0，因此 gcd(a, b) = a。

---

__B.6__ 如果 gcd(ab, c) = 1，则 gcd(a, c) = 1 且 gcd(b, c) = 1。

**证明** 设 gcd(a, c) = g。根据最大公约数的定义可知 g | a。从而 g | ab。

对于 gcd(ab, c) = 1，根据最大公约数的定义可知：

* 设 u 是整数且 u | ab 且 u | c，那么 u | 1。

取 u = g，则因为 g | ab，所以 g | 1。能整除 1 的正整数只有 1，所以 g = 1。

同理可证 gcd(b, c) = 1。

## C. 互质整数的性质

证明下列各命题对任意整数 a, b, c, d, r, s 都成立。

__C.1__ 如果存在整数 r, s 使得 ra + sb = 1，则 a 和 b 互质。

**证明** 根据 Bézout 引理，a 和 b 的线性组合都是 gcd(a, b) 的倍数。根据已知条件，gcd(a, b) | 1 ，所以只可能是 gcd(a, b) = 1。因此 a 和 b 互质。

---

__C.2__ 如果 gcd(a, c) = 1 且 c | ab，那么 c | b。

**证明** 根据 Bézout 引理，存在整数 r, s 使得方程 ra + sc = 1 成立。两边乘以 b 得 rab + scb = b。因为 c | ab，所以存在整数 m 使得 ab = mc，于是 rmc + scb = b，即 (rm + sb)c = b。这说明 c | b。

---

__C.3__ 如果 a | d 且 c | d，并且 gcd(a, c) = 1，那么 ac | d。

**证明** 根据 Bézout 引理，存在整数 r, s 使得方程 ra + sc = 1 成立。两边乘以 d 得 rad + scd = d。设 d = ma= nc，其中 m, n 是整数。代入得 ranc + scma = d，即 (rn + sm)ac = d。这说明 ac | d。

## D. 最大公约数和互质整数的更多性质

证明下列命题对任意整数 a, b, c, d, r, s 都成立。

__D.3__ 设 d = gcd(a,b)。对任意整数 x，d | x 当且仅当 x 是 a 和 b 的线性组合。

**证明**

（必要性）设 x = nd，其中 n 是整数。根据 Bézout 引理，存在整数 r, s 使得方程 ra + sb = d 成立。两边乘以 n 得 (rn)a + (sn)b = dn = x。这表明 x 是 a 和 b 的线性组合。

（充分性）根据 Bézout 引理，任何 a 和 b 的线性组合都是 d 的倍数。

## E. 最大公约数的一个性质

设 a, b 是整数。证明 1--2。

__E.1__ 设 a 是偶数，b 是奇数，或者反过来。那么 gcd(a, b) = gcd(a+b, a-b)。

**证明** 设 gcd(a, b) = g，则 g | a 且 g | b。于是 g | (a+b) 且 g | (a-b)。

设 gcd(a+b, a-b) = g'，则 g | g'。
此外，根据 Bézout 引理，存在整数 r, s 使得方程 ra + sb = g 成立。
那么 (r+s)(a+b) + (r-s)(a-b) = 2g。但是 a+b 和 a-b 都是奇数，所以一定有 r+s 和 r-s 都是偶数。

设 r' = (r+s)/2, s' = (r-s)/2，则有 r'(a+b) + s'(a-b) = g。
根据 D.3 知 g' | g。于是只可能是 g' = g。

---

__E.2__ 设 a 和 b 都是奇数。那么 2gcd(a, b) = gcd(a+b, a-b)。

**证明** 类似 E.1 的推理。但此时 a+b 和 a-b 都是偶数。

设 a' = (a+b)/2, b' = (a-b)/2，则有 (r+s)a' + (r-s)b' = g。根据 D.3 知 gcd(a', b') | g。
从而有 g' = gcd(2a', 2b') = 2gcd(a', b') | 2g。

因为 a 和 b 都是奇数，所以 g 是奇数。但 g' 是偶数，所以 g' 不可能等于 g。因此只可能 g' = 2g。

---

__E.3__ 如果 a 和 b 都是偶数，解释为什么上述两种结论都有可能。

**答** 推理过程类似 E.2，但是 g 和 g' 都是偶数，所以不能断言 g' ≠ g。因此 g' = g 和 g' = 2g 都有可能。

## G. &Zopf; 中的理想

证明下列命题。

**说明** 此题似应排除平凡理想（对应 n = 0, &pm;1）的情况。按照[参考资料](https://en.wikipedia.org/wiki/Prime_ideal)的定义，&langle;0&rangle; 是素理想，但 &langle;1&rangle; 不是素理想。

__G.1__ &langle;n&rangle; 是素理想当且仅当 n 是素数。

**证明** 只讨论 n ≥ 2 的情况。

（必要性）假设 n 是合数，那么存在 1 < p < n 和 1 < q < n 使得 n = pq。因为 pq = n &isin; &langle;n&rangle;，根据素理想的定义有 p &isin; &langle;n&rangle; 或 q &isin; &langle;n&rangle;。这和 n 是 &langle;n&rangle; 中的最小正元矛盾。所以假设不成立，n 是素数。

（充分性）设 a, b &isin; &Zopf; 且 ab &isin; &langle;n&rangle;。那么存在 k &isin; &Zopf; 使得 ab = kn。即 n | ab。根据欧几里得引理，n | a 或者 n | b。于是 a &isin; &langle;n&rangle; 或者 b &isin; &langle;n&rangle;。 

---

__G.2__ 任意素理想都是极大理想。

**证明** 设素理想 I = &langle;p&rangle;。根据 G.1 可知 p 是素数。

设 J 是 &Zopf; 的理想，且 I &subne; J。那么，存在 a &isin; J 但 a &notin; I。

a &notin; I 意味着 p &nmid; a。因为 p 的约数只有 1 和 p，所以 gcd(a, p) = 1。

根据 Bézout 引理，方程 ra + sp = 1 有整数解。因为 a &isin; J 且 p &isin; I &sube; J，所以 ra &isin; J 且 sp &isin; J。由此可知 1 &isin; J。

于是对于任意的 x &isin; &Zopf; 有 x = 1x &isin; J。这表明 J = A。因此 I 是极大理想。

---

__G.3__ 对于任意素数 p，&Zopf;<sub>p</sub> 是域。

**证明** 由 G.1 知 &langle;p&rangle; 是素理想。由 G.2 知 &langle;p&rangle; 是极大理想。根据第十九章证明的命题“如果 J 是极大理想则 A/J 是域”，&Zopf;<sub>p</sub> = &Zopf;/&langle;p&rangle; 是域。

---

__G.5__ 每个 &Zopf; 的同态像都同构于某个 &Zopf;<sub>n</sub>。

**证明** 设 f: &Zopf; &rarr; A，且它的核是 K。则 K 是 &Zopf; 的理想。
而 &Zopf; 的理想都是主理想，即 K = &langle;n&rangle;，其中 n 是整数。

根据同态基本定理，有 f(&Zopf;) &cong; &Zopf;/K = &Zopf;<sub>n</sub>。这就是所要证明的。

---

__G.6__ 设 G 是群且 a,b &isin; G。则 S = { n &isin; Z : ab<sup>n</sup> = b<sup>n</sup>a } 是 &Zopf; 的理想。

**证明** 显然 ab⁰ = b⁰a，所以 0 &isin; S。

设 x, y &isin; S。那么 ab<sup>x</sup> = b<sup>x</sup>a，即 b<sup>x</sup> = a<sup>-1</sup>b<sup>x</sup>a。
同理有 b<sup>y</sup> = a<sup>-1</sup>b<sup>y</sup>a，求逆得 b<sup>-y</sup> = a<sup>-1</sup>b<sup>-y</sup>a。
于是 b<sup>x-y</sup> = a<sup>-1</sup>b<sup>x-y</sup>a，即 ab<sup>x-y</sup> = b<sup>x-y</sup>a。这表明 (x-y) &isin; S，即 S 对减法封闭。

设 x &isin; S，则 b<sup>x</sup> = a<sup>-1</sup>b<sup>x</sup>a。对于任意 k &isin; &Zopf;，有 b<sup>kx</sup> = (b<sup>x</sup>)<sup>k</sup> = a<sup>-1</sup>b<sup>kx</sup>a，即 ab<sup>kx</sup> = </sup>b<sup>kx</sup>a。这表明 xk = kx &isin; S，即 S 在 &Zopf; 中吸收乘法。

综上所述，S 是 &Zopf; 的理想。

# 数论基础

第二十三章是可选章节，包含了初等数论的基础内容：

* 同余
* Fermat 定理、Euler 定理（它们是 Lagrange 定理在模 n 整数环中的特例）
* 线性同余方程（组）
* 中国剩余定理

习题部分额外引入了如下内容： Wilson 定理、二次剩余和原根。

## A. 解同余方程

__A.4__ 求解下列二次同余方程。（如果没有解，写“无解”。）

(f) \\(3x^2-6x+6 \equiv 0\pmod{15}\\)

**解**
\begin{gather\*}
3x^2-6x+6 \equiv 0\pmod{15}\\\\
x^2-2x+2 \equiv 0\pmod{5}\\\\
x^2-2x-3 \equiv 0\pmod{5}\\\\
(x-3)(x+1) \equiv 0\pmod{5}\\\\
x \equiv 3 \text{ 或 } x \equiv -1 \pmod{5}\\\\
\end{gather\*}

---

__A.6__ 解下列丢番图[^1]方程。（如果没有解，写“无解”。）

(d) \\(30x^2+24y=18\\)。

**解** 原方程等价于 \\(5x^2+4x=3\\)。以 \\(4\\) 为模（注意 \\(5\equiv1\\)）得同余方程
\\[x^2 \equiv 3\pmod4\\]
而在 \\(\mathbb Z_4\\) 中，平方等于 \\(3\\) 的数是不存在的，所以方程无解。

## D. 同余的更多性质

证明下列命题对任意整数 a, b, c 和正整数 m, n 成立。

__D.6__ 如果 ab &equiv; 1 (mod c)，ac &equiv; 1 (mod b)，且 bc &equiv; 1 (mod a) ，那么 ab + bc + ac &equiv; 1 (mod abc)。

根据已知条件可知存在整数 x, y, z 使得

* ab - 1 = xc
* bc - 1 = ya
* ac - 1 = zb

三式相乘得 (ab-1)(bc-1)(ac-1) = (xyz)(abc)。

两边对 abc 取模。注意到左边的展开式中，任意两个带字母的项相乘总是含有因子 abc，所以和 0 同余。于是得
ab + bc + ac - 1 &equiv; 0 (mod abc)。两边加 1 就得到要证的同余式。

## E. Fermat 定理的结论

证明 2--6。

__E.4__ 设 p 和 q 是不同的质数，则 p<sup>q-1</sup> + q<sup>p-1</sup>&equiv; 1 (mod pq)。

**证明** 根据 Fermat 定理，当 p &ne; q 时有 p<sup>q-1</sup> &equiv; 1 (mod q)。所以存在整数 x 使得 p<sup>q-1</sup> - 1 = xq。同理可证存在整数 y 使得 q<sup>p-1</sup> - 1 = yp。

两式相乘得 (p<sup>q-1</sup>-1)(q<sup>p-1</sup>-1) = xypq。两边对 pq 取模，得 -p<sup>q-1</sup> -q<sup>p-1</sup> + 1 &equiv; 0 (mod pq)。移项就得到要证的同余式。

__E.6__ 设 p 和 q 是不同的质数。

1. 如果 (p-1) | m 且 (q-1) | m，那么 a<sup>m</sup> &equiv; 1 (mod pq) 对任意满足 p&nmid;a 且 q&nmid;a 的整数 a 成立。
2. 如果 (p-1) | m 且 (q-1) | m，那么 a<sup>m+1</sup> &equiv; a (mod pq) 对任意整数 a 都成立。

**证明(1)** 当 p&nmid;a 时，p 和 a 互质。根据 Fermat 定理有 a<sup>m</sup> &equiv; 1 (mod p)。于是存在整数 x 使得 a<sup>m</sup>-1 = xp。同理可证存在整数 y 使得 a<sup>m</sup>-1 = yq。

由 xp = yq 知 p | yq。但 p&nmid;q，根据欧几里得引理知 p | y，即存在整数 z 使得 y = zp。

于是 a<sup>m</sup>-1 = zpq。两边对 pq 取模得 a<sup>m</sup>-1 &equiv; 0 (mod pq)。移项就得到要证的同余式。

**证明(2)** 分类讨论：

* 如果 p 和 q 均不整除 a，由 (1) 得 a<sup>m</sup> &equiv; 1 (mod pq)。两边乘 a 得 a<sup>m+1</sup> &equiv; a (mod pq)。
* 如果 p 和 q 中有且只有一个整除 a，不失一般性，设 p | a 且 q&nmid;a。那么 a = kp，其中 k 是整数。根据 Fermat 定理有 a<sup>m</sup> &equiv; 1 (mod q)。因此存在整数 t 使得 a<sup>m</sup> - 1 = tq。和 a = kp 相乘得 a<sup>m+1</sup> - a = ktpq。两边对 pq 取模，得 a<sup>m+1</sup> - a &equiv; 0 (mod pq)。移项得 a<sup>m+1</sup> &equiv; a (mod pq)。
* 如果 p 和 q 均整除 a，那么 pq | a。所以 a<sup>m+1</sup> &equiv; 0 &equiv; a (mod pq)。

综上所述，要证的同余式总是成立。

## F. Euler 定理的结论

__F.8__ 如果 \\(\gcd(m,n)=1\\)，证明 \\(n^{\phi(m)}+m^{\phi(n)}&equiv;1\pmod{mn}\\)。

**证明** 同 E.4。将其中的 Fermat 定理改为 Euler 定理即可。

## G. Wilson 定理和相关结论

在任意整环中，如果 \\(x^2 = 1\\)，那么 \\(x^2-1 = (x+1)(x-1) = 0\\)；于是 \\(x = \pm1\\)。
因此，任意元素 \\(x \ne \pm1\\) 都不可能是自己的乘法逆元。
由此得到一个结论：在 \\(\mathbb Z_p\\) 中，整数 \\(\bar2, \bar3, ..., \overline{p-2}\\) 可以两两配对，每个元素都和自己的乘法逆元组成一对。

证明下列命题。

__G.1__ 在 \\(\mathbb Z_p\\) 中，\\(\bar2\cdot\bar3\cdots\overline{p-2} = 1\\)。

**证明** 可以把因子两两配对，每对由互逆的两个元素构成，从而乘积等于 1。因此所有因子的乘积等于 1。

---

__G.2__ \\((p-2)! \equiv 1 \pmod p\\) 对任意质数 \\(p\\) 都成立。

**证明** 容易验证当 \\(p=2,3\\) 时该同余式成立。当 \\(p>3\\) 时，由 G.1 立即得到。

---

__G.3__ \\((p-1)!+1&equiv;0\pmod p\\) 对任意质数 \\(p\\) 都成立。这被称为 *Wilson 定理*。

**证明** 将 G.2 的同余式和 \\(p-1 \equiv -1 \pmod p\\) 相乘再移项即可得到。

---

__G.4__ 对于任意合数 \\(n\ne4\\)，有 \\((n-1)! \equiv 0 \pmod n\\)。

**证明** 要证明该同余式，只需证明 \\(n\mid (n-1)!\\) 即可。

因为 \\(n\\) 是合数，所以 \\(n=ab\\)，其中 \\(1 < a\le b < n\\)。

* 如果 \\(a < b\\)，那么 \\((n-1)! = (n-1)(n-2)\cdots b\cdots a\cdots1\\) 能被 \\(ab\\) 整除。
* 如果 \\(a = b\\)，那么 \\(n = a^2\\)。根据合数 \\(n\ne 4\\) 可知 \\(a > 2\\)。因此 \\(a < 2a < a^2=n\\)。于是 \\((n-1)! = (n-1)(n-2)\cdots (2a)\cdots a\cdots1\\) 能被 \\(a^2\\) 整除。

综上所述，\\((n-1)!\\) 总能被 \\(n\\) 整除，因此命题得证。

---

__G.9__ 对于任意质数 \\(p>2\\)，方程 \\(x^2\equiv -1\pmod p\\) 有解当且仅当 \\(p\not\equiv3\pmod 4\\)。

**证明** 大于 2 的质数都是奇数。所以除以 4 的余数只可能是 1 或 3。分两个方面证明。

1. 当 \\(p\equiv1\pmod4\\) 时方程有解。

因为
\begin{align\*}
p-1 &\equiv -1\\\\
p-2 &\equiv -2\\\\
&\vdots\\\\
\frac{p+2}{2} &\equiv -\frac{p-1}{2} \pmod p
\end{align\*}
全部相乘，并考虑此时 \\((p-1)/2\\) 是偶数，得
\begin{align\*}
(p-1)(p-2)\cdots\frac{p+2}{2} &\equiv (-1)^{p-1\over2}\frac{p-1}{2}\cdots2\cdot1 \\\\
&= \Bigl(\frac{p-1}{2}\Bigr)! \pmod p
\end{align\*}
于是
\\[(p-1)! \equiv \Bigl[\Bigl(\frac{p-1}{2}\Bigr)!\Bigr]^2 \pmod p\\]
由 Wilson 定理知 \\((p-1)! \equiv -1 \pmod p\\)，因此
\\[\Bigl[\Bigl(\frac{p-1}{2}\Bigr)!\Bigr]^2 \equiv -1 \pmod p\\]
这表明 \\(x = \bigl(\frac{p-1}{2}\bigr)!\\) 是方程 \\(x^2 \equiv -1\\) 的一个解。

2. 当 \\(p\equiv3\pmod4\\) 时方程无解。

这时 \\((p-1)/2\\) 是奇数。将方程两边求 \\((p-1)/2\\) 次幂，得 \\(x^{p-1} \equiv -1 \pmod p\\)。

* 如果 \\(p\mid x\\)，显然有 \\(x^{p-1} \equiv 0 \pmod p\\)；矛盾。
* 如果 \\(p\nmid x\\)，根据 Fermat 定理，应当有 \\(x^{p-1} \equiv 1\pmod p\\)；矛盾。

无论如何都得到矛盾，因此方程不可能有解。

## H. 二次剩余

对于整数 \\(a\\)，如果存在整数 \\(x\\) 使得 \\(x^2\equiv a\pmod m\\)，则称为模 \\(n\\) 的*二次剩余 (quadratic residue)*。这等于说 \\(\bar a\\) 在 \\(\mathbb Z_m\\) 中是一个平方元素。如果 \\(a\\) 不是模 \\(m\\) 的二次剩余，则称 \\(a\\) 为模 \\(m\\) 的*二次非剩余  (quadratic nonresidue)*。二次剩余对于求解二次同余方程、研究平方数的和，等等，有重要作用。这里，我们考察模任意素数 \\(p > 2\\) 的二次剩余。

设 \\(\newcommand{\Zp}{\mathbb Z_p}h\colon \Zp^\ast \to \Zp^\ast\\) 定义为 \\(h(\bar a) = \bar a^2\\)。

[说明：\\(\Zp^\ast = \\{\bar1,\bar2,\ldots,\overline{p-1}\\}\\) 是 \\(\Zp\\) 中所有非零元素和乘法运算构成的群。]

__H.1__ 证明 \\(h\\) 是同态且核为 \\(\\{\pm1\\}\\)。

**证明** 显然 \\(\Zp^\ast\\) 是 Abel 群。设 \\(x,y\in\Zp^\ast\\)，则
\\[h(xy) = (xy)^2 = x^2y^2 = h(x)h(y)\\]
因此 \\(h\\) 是同态。令 \\([h(x)]^2 = 1\\)。因为在 \\(\Zp\\) 中满足 \\(x^2=1\\) 的只有 \\(x=\pm 1\\)，它们都在 \\(\Zp^\ast\\) 中，所以方程的解集是 \\(\\{\pm1\\}\\)，这也就是 \\(h\\) 的核。

---

__H.2__ \\(h\\) 的值域含有 \\((p-1)/2\\) 个元素。证明：值域 \\(R\\) 是 \\(\Zp^\ast\\) 的子群且含有两个陪集。一个陪集包含所有剩余，另一个包含所有非剩余。

**证明** 显然 \\(R\\) 是 \\(\Zp^\ast\\) 的非空子集。

设 \\(y_1,y_2\in R\\)，则存在 \\(x_1,x_2\in \Zp^\ast\\) 使得 \\(y_1=x_1^2,y_2=x_2^2\\)。
因此 \\(y_1y_2 = (x_1x_2)^2 = h(x_1x_2) \in R\\)。这说明 \\(R\\) 对乘法封闭。

又 \\(R\\) 是有限集，所以根据非空和对乘法封闭就可以判定（第五章习题 D.5）它是 \\(\Zp^\ast\\) 的子群。因为 \\(\Zp^\ast\\) 是 Abel 群，因此 \\(R\\) 是正规子群。

因为 \\(R=R1\\) 是一个陪集，而 \\(|R|=(p-1)/2\\), \\(|\Zp^\ast| = p-1\\)，所以陪集的个数为 \\((\Zp^\ast : R)=\frac{p-1}{(p-1)/2} = 2\\)。根据定义，\\(R\\) 中有且只有表示为 \\(x^2\\)（其中 \\(x\in\Zp^\ast\\)）的元素，即所有的二次剩余。所以另一个陪集中有且只有二次非剩余。

---

*Legendre 符号*定义如下：
\\[\newcommand{\legendre}[2]{\genfrac{(}{)}{}{}{#1}{#2}}
\legendre{a}{p} = \cases{
+1&如果 $p\not\mid a$ 且 $a$ 是模 $p$ 的剩余\cr
-1&如果 $p\not\mid a$ 且 $a$ 是模 $p$ 的非剩余\cr
 0&如果 $p\mid a$\cr
}\\]

__H.3__ 参考 H.2，把 \\(R\\) 的两个陪集命名为 \\(1\\) 和 \\(-1\\)。那么 \\(\Zp^\ast / R = \\{1,-1\\}\\)。解释为什么对每个不是 \\(p\\) 的倍数的整数 \\(a\\) 都有
\\[\legendre{a}{p} = R\bar a\\]

**说明** 原书中似有排版错误（它将 \\(R\bar a\\) 写成 \\(h(\bar a)\\)；那样的话不成立）。

**答** 根据 H.2，包含所有剩余的陪集是 \\(R\\)，将它命名为 \\(1\\)；设包含所有非剩余的陪集是 \\(R\bar n\\)，其中 \\(\bar n\\) 是某个非剩余，将它命名为 \\(-1\\)。那么
\\[R\bar a = \cases{1& $\bar a$ 是剩余\cr -1&$\bar a$ 是非剩余}\\]
当 \\(p\nmid a\\) 时，\\(\bar a\in\Zp^\ast\\)。对照 Legendre 符号的定义可知 \\(\legendre a p = R\bar a\\)。

另外说明，陪集之间的乘法满足如下运算规则：
\begin{gather\*}
1\cdot 1 = 1 \\\\
(-1)\cdot 1 = 1 \cdot (-1) = R(1\cdot\bar n) = -1\\\\
(-1)\cdot (-1) = R(\bar n^2) = 1\\\\
\end{gather\*}

---

__H.4__ 求 \\(\legendre{17}{23}\\); \\(\legendre{3}{29}\\); \\(\legendre{5}{11}\\); \\(\legendre{8}{13}\\); \\(\legendre{2}{23}\\)。

**解** 计算得
\\[\legendre{17}{23} =-1, \legendre{3}{29} = -1, \legendre{5}{11} = 1, \legendre{8}{13} = -1, \legendre{2}{23} = 1\\]

---

__H.5__ 求证：如果 \\(a\equiv b\pmod p\\)，则 \\(\legendre a p = \legendre b p\\)。特别地，\\(\legendre {a+kp}{p} = \legendre a p\\)。

**证明** 在 \\(\Zp^\ast\\) 中，\\(\bar a = \bar b\\)。于是 \\(R\bar a = R\bar b\\)。根据 H.3 可知 \\(\legendre a p = \legendre b p\\)。代入 \\(b = a+kp\\) 即得特例。

---

__H.6__ 求证：
\begin{gather\*}
\legendre a p \legendre b p = \legendre {ab}{p}\tag a\\\\
\text{当 $p\nmid a$ 时，}\legendre {a^2}{p} = 1\tag b
\end{gather\*}

**说明** 原书中有排版错误，第二个等式右边的 1 是我凭猜测补上的。

**证明** 在 \\(\Zp^\ast\\) 中有 \\(\overline{ab} = \bar a \bar b\\)。因为 \\(R\bar a\cdot R\bar b=R(\bar a \bar b)=R\overline{ab}\\)，根据 H.3 可知 \\(\legendre a p \legendre b p = \legendre {ab}{p}\\)。
当 \\(p\nmid a\\) 时，\\(\legendre a p = \pm 1\\)。于是 \\(\legendre {a^2}{p} = {\legendre a p}^2 = 1\\)。

---

__H.7__ 求证：\\[\legendre {-1}{p} = \begin{cases}+1& p\equiv1\pmod 4\\\\-1 &p\equiv3\pmod 4\end{cases}\\]

**证明** 参见 G.9。结合二次剩余的定义即可得到结论。

---

用于计算 \\(\legendre a p\\) 的最重要的规则是*二次互反律 (law of quadratic reciprocity)*，它断言对于任意不相等的素数 \\(p,q > 2\\)，有
\\[\legendre p q = \cases{-\legendre q p& 如果 $p\equiv q\equiv 3\pmod 4$\cr \phantom-\legendre q p& 不然}\\]
（其证明可以在任何一本数论教科书中找到，例如 W. J. LeVeque 的 *Fundamentals of Number Theory*。）

__H.8__ 用 H.5--H.7 以及二次互反律计算：
\\[\legendre{30}{101}\quad\legendre{10}{151}\quad\legendre{15}{41}\quad\legendre{14}{59}\quad\legendre{379}{401}\\]
14 是不是模 59 的二次剩余？

**注** 不能对含 2 的 Legendre 符号套用二次互反律的公式，否则计算结果不一定正确。可以使用 H.5 做加减法来避开因子 2。事实上，如果使用和一个 Legendre 符号有关的定理（二次互反律的第二补充）：
\\[\legendre{2}{p} = \cases{+1& 如果 $p\equiv\pm1 \pmod 8$\cr-1& 不然}\\]
则可以使计算更简便。对于本题，仅对第一个计算式使用 H.5 方法避开因子 2 作为示范，其余的使用以上补充条件。

**解**

1. \\(\legendre{30}{101} = \legendre{2}{101} \legendre{3}{101} \legendre{5}{101}=1\\)，其中
   * \\(\legendre{2}{101}=\legendre{-99}{101}=\legendre{-1}{101}\legendre{3^2}{101}\legendre{11}{101}=-1\\)，其中
      * \\(\legendre{-1}{101} = 1\\)（使用 H.7）
      * \\(\legendre{3^2}{101} = 1\\)（使用 H.6(b)）
      * \\(\legendre{101}{11} = \legendre{2}{11} = \legendre{-9}{11} = \legendre{-1}{11}\legendre{3^2}{11}=-1\\)（使用 H.7、H.6(b)）
   * \\(\legendre{3}{101}=\legendre{101}{3}=\legendre{2}{3}=-1\\)
   * \\(\legendre{5}{101}=\legendre{101}{5}=\legendre{1}{5}=1\\)
2. \\(\legendre{10}{151} = \legendre{2}{151} \legendre{5}{151} = 1\\)，其中
   * \\(\legendre{2}{151} = 1\\)（第二补充）
   * \\(\legendre{5}{151} = \legendre{151}{5} = \legendre{1}{5} = 1\\)
3. \\(\legendre{15}{41} = \legendre{3}{41} \legendre{5}{41} = -1\\)，其中
   * \\(\legendre{3}{41} = \legendre{41}{3} = \legendre{2}{3} = -1\\)
   * \\(\legendre{5}{41} = \legendre{41}{5} = \legendre{1}{5} = 1\\)
4. \\(\legendre{14}{59} = \legendre{2}{59} \legendre{7}{59} = -1\\)，其中
   * \\(\legendre{2}{59} = -1\\)（第二补充）
   * \\(\legendre{7}{59} = -\legendre{59}{7} = -\legendre{3}{7} = \legendre{7}{3} = \legendre{1}{3} = 1\\)
5. \\(\legendre{379}{401} = \legendre{401}{379} = \legendre{22}{379} = \legendre{2}{379} \legendre{11}{379} = 1\\)，其中
   * \\(\legendre{2}{379} = -1\\)（第二补充）
   * \\(\legendre{11}{379} = -\legendre{379}{11} = -\legendre{5}{11} = -\legendre{11}{5} = -\legendre{1}{5} = -1\\)

根据 (4) 可知 14 不是模 59 的二次剩余。

## I. 原根

回忆 \\(V_n\\) 表示 \\(\mathbb Z_n\\) 中所有可逆元素构成的乘法群[^2]。
如果 \\(V_n\\) 恰好是循环群，例如 \\(V_n=\langle m\rangle\\)，那么任意整数 \\(a \equiv m\\) 称为 \\(n\\) 的*原根 (primitive root)*。

__I.1__ 证明 \\(a\\) 是 \\(n\\) 的原根当且仅当 \\(\bar a\\) 在 \\(V_n\\) 中的阶是 \\(\phi(n)\\)。

**证明** 乘法群 \\(V_n\\) 的阶是 \\(\phi(n)\\)。\\(\bar a\\) 是 \\(V_n\\) 的生成元的充分必要条件是 \\(\mathop{\rm ord}(\bar a) = |V_n| = \phi(n)\\)（第十一章习题 C.5）。

---

__I.2__ 证明每个素数 p 都有原根。

**证明** 因为 &Zopf;<sub>p</sub> 是域。根据第三十三章定理 1，域的所有非零元素和乘法运算构成的群是循环群。那么循环群 &Zopf;<sub>p</sub><sup>\*</sup> 的生成元就是原根。

**注** 这题用到了后面章节的定理；我自己并不能想出“域的所有非零元素和乘法运算构成循环群”的证明。

---

__I.3__ 寻找下列整数的原根（如果不存在，请指出）： 6, 10, 12, 14, 15。

**解**

* &Zopf;<sub>6</sub><sup>\*</sup> = {1, 5} = &langle;5&rangle;。所以 5 是 6 的原根。
* &Zopf;<sub>10</sub><sup>\*</sup> = {1, 3, 7, 9} = &langle;3&rangle; = &langle;7&rangle;。所以 3 和 7 是 10 的原根。
* &Zopf;<sub>12</sub><sup>\*</sup> = {1, 5, 7, 11}。所有元素的阶均不超过 2，所以不存在生成元。因此 12 没有原根。
* &Zopf;<sub>14</sub><sup>\*</sup> = {1, 3, 5, 9, 11, 13} = &langle;3&rangle; = &langle;5&rangle;。所以 3 和 5 是 14 的原根。
* &Zopf;<sub>15</sub><sup>\*</sup> = {1, 2, 4, 7, 8, 11, 13, 14}。所有元素的阶均不超过 4，所以不存在生成元。因此 15 没有原根。

---

__I.4__ 假设 \\(a\\) 是 \\(m\\) 的原根。求证：如果 \\(b\\) 是和 \\(m\\) 互质的整数，那么 \\(b\equiv a^k\pmod m\\) 对某个 \\(k\ge 1\\) 成立。

**证明** \\(a\\) 是 \\(m\\) 的原根意味着 \\(V_m = \langle \bar a\rangle\\)。当 \\(b\\) 和 \\(m\\) 互质时有 \\(\bar b \in V_m\\)。因为 \\(\bar a\\) 是生成元，所以存在正整数 \\(k\\) 使得 \\(\bar b = \bar a^k = \overline{a^k}\\)。这意味着 \\(b\equiv a^k \pmod m\\)。

---

__I.5__ 设 \\(m\\) 有原根，且 \\(n\\) 和 \\(\phi(m)\\) 互质（假定 \\(n>0\\)）。证明：如果 \\(a\\) 和 \\(m\\) 互质，则 \\(x^n \equiv a \pmod m\\) 有解。

**证明** 设原根为 \\(g\\)，则 \\(V_m = \langle \bar g\rangle\\)。因为 \\(a\\) 和 \\(m\\) 互质，所以 \\(\bar a \in V_m\\)。根据生成元定义，存在正整数 \\(k\\) 使得 \\(\bar a=\bar g^k\\)。

因为 \\(\gcd(n,\phi(m)) = 1\\)，根据 Bézout 引理，方程 \\(rn + s\phi(m) = k\\) 有整数解。

于是有 \\(\bar a = \bar g^{rn+s\phi(m)} = (\bar g^r)^n \bigl(\bar g^{\phi(m)}\bigr)^s = (\bar g^r)^n\\)，因为 \\(\mathop{\rm ord}(g) = \phi(m)\\)。设 \\(\bar x = \bar g^r\\)，则有 \\(\bar a = \bar x^n\\)。这表明存在整数 \\(x\\) 是 \\(x^n \equiv a \pmod m\\) 的解。

---

__I.6__ 设 \\(p>2\\) 是质数。证明：\\(p\\) 的每个原根都是模 \\(p\\) 的二次非剩余。

**证明** 用反证法。设 \\(g\\) 是 \\(p\\) 的原根。假设 \\(g\\) 是模 \\(p\\) 的二次剩余，那么 \\(\legendre g p = 1\\)，于是 \\(\legendre {g^n} p = \legendre g p ^n = 1\\)。这意味着 \\(\Zp^\ast\\) 中的所有元素都是二次剩余。这是不可能的，因为我们已经知道 \\(\Zp^\ast\\) 中恰有一半的元素是二次剩余（习题 H.2）。所以假设不成立，原命题成立。

---

__I.7__ 形如 \\(p=2^m+1\\) 的质数 \\(p\\) 称为 *Fermat 质数*。设 \\(p\\) 是一个 Fermat 质数。证明每个模 \\(p\\) 的二次非剩余都是 \\(p\\) 的原根。

**证明** 根据 I.2 知 \\(\Zp^\ast = \\{\bar 1,\bar 2,\ldots,\overline{2^m}\\} = \langle \bar g\rangle\\) 是循环群，其中 \\(\bar g\\) 是某个生成元。那么，\\(\bar g\\) 的阶是 \\(2^m\\)。

设 \\(a\\) 是模 \\(p\\) 的二次非剩余。则 \\(\bar a \in\Zp^\ast\\) 是二次非剩余。根据生成元定义，存在正整数 \\(r\\) 使得 \\(\bar a = \bar g^r\\)。这里 \\(r\\) 只能是奇数，否则 \\(\bar a = (\bar g^{r/2})^2\\) 就是二次剩余。

设 \\(\bar a\\) 的阶为 \\(n\\)，根据 Lagrange 定理，有 \\(n \mid 2^m\\)。根据阶的定义，有 \\(\bar a^n = \bar g^{nr} = \bar 1\\)。于是 \\(nr\\) 一定是 \\(g\\) 的阶的倍数，也即 \\(2^m \mid nr\\)。但是 \\(r\\) 是奇数，所以只可能是 \\(2^m \mid n\\)。这意味着 \\(n = 2^m\\)，即 \\(\bar a\\) 的阶等于 \\(\Zp^\ast\\) 的阶，所以 \\(\bar a\\) 是生成元。

因此，二次非剩余 \\(a\\) 是模 \\(p\\) 的原根。

[^1]: 欧几里得、丢番图是约定俗成的译名。它们的希腊文名字是 Ευκλειδης 和 Διόφαντος。
[^2]: 另一种常见的记法是 \\((\mathbb Z/n\mathbb Z)^\times\\)。
