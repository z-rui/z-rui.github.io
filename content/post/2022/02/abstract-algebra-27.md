---
title: 抽象代数习题(27) -- 域扩张
date: 2022-02-02T20:20:29-08:00
---

《抽象代数》第二十七章讲的是域扩张 (extension of fields)。F[x] 中的多项式在 F 中不一定有根，但是，在比 F 更大的域 K 中可以有根。在初等数学中就有这样的例子：x²-2 和 x²+1 都是 &Qopf;[x] 中的多项式，但是它们的根分别在扩域 &Ropf; 和 &Copf; 中。

本章研究了代数元、多项式和域扩张之间的联系。

* 凡是能作为 F\[x\] 中某个多项式的根的元素就是 F 的**代数元 (algebraic)**；否则是**超越元 (transcendental)**。
* F 的任意代数元 a 都对应 F\[x\] 中的一个**最小多项式 (minimum polynomial)** p(x)，使得 a 是 p(x) 的根。
* 对于 F\[x\] 中的任意多项式 p(x)，总能找到适当的扩张 K &supe; F，使得 p(x) 在 K 中有根。这就是**域扩张基本定理**。

<!--more-->

## A. 识别代数元

__A.1__ 证明下列数是 \\(\newcommand\Q{\mathbb Q}\Q\\) 上的代数元：

1. \\(\sqrt{i-\sqrt2}\\)
2. （其余略）

**证明** 设 \\(a=\sqrt{i-\sqrt2}\\)，两边平方得 \\(a^2 = i-\sqrt2\\)。移项后两边再平方得
\\(a^4-2a^2\mkern1mui-1 = 2\\)。移项后两边再平方得 \\(a^8-6a^4+9 = -4a^2\\)，即
\\(a^8-2a^4+9 = 0\\)。可见 \\(a\\) 是 \\(\Q[x]\\) 中的多项式 \\(x^4-2x^2+9\\) 的根。因此 \\(a\\) 是 \\(\Q\\) 上的代数元。

---

__A.2__ 证明下列数在给定的域上是代数元：

1. \\(\sqrt\pi\\) 在 \\(\Q(\pi)\\) 上
2. \\(\sqrt\pi\\) 在 \\(\Q(\sqrt\pi)\\) 上
3. \\(\pi^2-1\\) 在 \\(\Q(\pi^3)\\) 上

**证明**

1. \\(\sqrt\pi\\) 是 \\(x^2-\pi\\) 的根。
1. \\(\sqrt\pi\\) 是 \\(x^4-\pi^2\\) 的根。
2. \\(\pi^2-1\\) 是 \\(x^3+3x^2+3x+1-\pi^6\\) 的根。

**注** 证明一个数是超越元要难得多。通过一些复杂的数学工具可以证明 &pi; 和 e 是 &Qopf; 上的超越元。

## B. 寻找最小多项式

__B.1__ 寻找下列数的在 \\(\Q\\) 上的最小多项式：

1. \\(1+\sqrt2\\)
2. \\(\sqrt{1+\sqrt2}\\)
3. （其余略）

**解**

1. \\(p(x) = x^2-2x-1\\)。可能的有理根是 \\(\pm 1\\)；代入可知均不是根。所以二次式 \\(p(x)\\) 在 \\(\Q\\) 上不可约。
2. \\(p(x) = x^4-2x^2-1\\)
   * \\(p(x+1) = x^4 + 4x^3 + 4x^2 - 2\\)。取 \\(p=2\\)，根据 Eisenstein 判据可知 \\(p(x+1)\\) 在 \\(\Q\\) 上不可约。因此 \\(p(x)\\) 在 \\(\Q\\) 上不可约。

---

__B.2__ 证明 \\(\sqrt2+i\\) 的最小多项式是

1. 在 \\(\mathbb R\\) 上：\\(x^2-2\sqrt2 x+3\\)
2. 在 \\(\Q\\) 上：\\(x^4-2x^2+9\\)
3. 在 \\(\Q(i)\\) 上：\\(x^2-2ix-3\\)

**证明** 显然 \\(\sqrt2+i\\) 都是这三个多项式的根。只须证明不可约性即可。

1. 判别式 \\(\Delta = (2\sqrt2)^2 - 4\cdot1\cdot 3 = -4 < 0\\)。由此可知二次式 \\(x^2-2\sqrt2 x+3\\) 在 \\(\mathbb R\\) 上没有根，因此不可约。
2. 可能的有理根为 \\(\pm1,\pm3\\)；代入可知均不是根。假设可以分解为 \\((x^2+ax+b)(x^2+cx+d)\\)，其中 \\(a,b,c,d\\) 是整数，那么 \\(a+c=0\\)，\\(ac+b+d=-2\\)，\\(bc+ad=0\\) 且 \\(bd=9\\)。那么 \\(a=-c\\) 且 \\(b=d=\pm3\\)。于是 \\(a^2=b+d+2 = 5 \text{ 或 }{-1}\\)，当 \\(a\\) 是整数时不可能。所以也不能分解为二次式的积。
3. 该二次式的两个根 \\(\pm\sqrt2+i\\) 都不在 \\(\Q(i)\\) 中，所以在 \\(\Q(i)\\) 上不可约。

---

__B.5__ 寻找首一多项式 \\(p(x)\\) 使得 \\(Q[x]/\langle p(x)\rangle\\) 同构于

1. \\(\Q(\sqrt2)\\)
2. \\(\Q(1+\sqrt2)\\)
3. \\(\Q\bigl(\sqrt{1+\sqrt2}\bigr)\\)

**解** 根据 \\(F(c) \cong F[x] / \langle p(x)\rangle\\) 可知，只需寻找首一多项式 \\(p(x)\\) 使得 \\(c\\) 是 \\(p(x)\\) 的根即可。

1. \\(p(x) = x^2 - 2\\)
2. \\(p(x) = x^2-2x-1\\)
3. \\(p(x) = x^4-2x^2-1\\)

2 和 3 的不可约性见 B.1。

## C. 域 F[x]/&langle;p(x)&rangle; 的结构

设 p(x) 是 F 上的 n 次不可约多项式。设 c 是 p(x) 在 F 的某个扩张中的根（如域扩张基本定理所述）。

__C.1__ 求证：F(c) 中的每个元素都可以写成 r(c)，其中 r(x) 是 F[x] 中次数小于 n 的多项式。

**证明** 设 a &isin; F(c)。那么 a = t(c)，其中 t(x) &isin; F[x]。根据多项式带余除法，有 t(x) = q(x)p(x) + r(x)。其中 q(x),r(x) &isin; F[x] 且 deg r(x) < n。代入 x=c 得 t(c) = q(c)p(c) + r(c)。而 p(c) = 0，因此 t(c) = r(c)。

---

__C.2__ 如果 s(c) = t(c) 是 F(c) 的元素，其中 s(x) 和 t(x) 是次数小于 n 的多项式，证明 s(x) = t(x)。

**证明** 设同态 &sigma;<sub>c</sub>: F[x] &rarr; K 定义为 &sigma;<sub>c</sub>(a(x)) = a(c)。设它的核为 J<sub>c</sub>，则 J<sub>c</sub> 是 F[x] 的主理想。设 J<sub>c</sub>=&langle;m(x)&rangle;，其中 m(x) 是不可约多项式。因为 p(c) = 0，所以 p(x) &isin; J<sub>c</sub>，从而有 m(x) | p(x)。但是 p(x) 不可约，所以 m(x) 也是 n 次多项式。

设 a(x) = s(x) - t(x)，则 a(x) = 0 或 a(x) 是次数小于 n 的多项式。而 a(c) = s(c) - t(c) = 0，这说明 a(x) &isin; J<sub>c</sub>。但已经知道 J<sub>c</sub> 中最小的次数是 n，所以只可能 a(x) = 0。由此可知 s(x) - t(x) = 0 即 s(x) = t(x)。

---

__C.3__ 根据 C.1 和 C.2 做出结论：F(c) 中的每个元素都可以*唯一地*表示为 r(c)，其中 deg r(x) < n。

**证明** 存在性由 C.1 给出；唯一性由 C.2 给出。

---

__C.4__ 根据 C.3，解释为什么 &Zopf;₂[x]/&langle;x²+x+1&rangle; 中恰有 4 个元素。列出这 4 个元素，并给出它们的加法和乘法运算表。

**解** 设 p(x) = x²+x+1，并设在 &Zopf;₂ 的扩域 &Zopf;₂(c) 中有根 x=c。那么，&Zopf;₂[x]/&langle;x²+x+1&rangle; &cong; &Zopf;₂(c)。根据 C.3，任意 &Zopf;₂(c) 中的元素都可以表示为 r(c)，其中 r(x) &isin; &Zopf;₂[x] 是次数小于 2 的多项式。

由此可知 &Zopf;₂ 中的元素是：0、1、c、c+1，共有 4 个。

（运算表略）

## D. 和域扩张有关的简短问题

设 F 是任意域。证明 1--5。

__D.1__ 如果 c 是 F 上的代数元，那么 c+1 和 kc 也是（其中 k &isin; F）。

**证明** 设 p(x) &isin; F[x] 且 c 是 p(x) 的一个根。那么，使用二项式定理可知 p(x-1) &isin; F[x]。于是 c+1 是 p(x-1) 的一个根。这表明 c+1 是 F 上的代数元。

另一方面，如果 k=0，则 kc=0 显然在 F 上是代数的。否则，p(k<sup>-1</sup>x) &isin; F[x]，而 kc 是它的根。这表明 kc 是 F 上的代数元。

---

__D.2__ 如果 \\(c \ne 0\\) 且 \\(c\\) 是 \\(F\\) 上的代数元，那么 \\(1/c\\) 也是。

**证明** 设 \\(p(x) \in F[x]\\) 是 \\(c\\) 的最小多项式。设
\\[p(x) = p_0+p_1x+\cdots+p_nx^n\\]
那么 \\(p(c) = 0\\)。设
\\[q(x) = p_n + p_{n-1}x + \cdots + p_0x^n\\]
则 \\(q(x) \in F[x]\\)，而在 \\(F(c)\\) 中有
\begin{align\*}
q(1/c) &= p_n + p_{n-1}c^{-1} + \cdots + p_0c^{-n} \\\\
       &= c^{-n}\bigl(p_nc^n + p_{n-1}c^{n-1} + \cdots + p_0) \\\\
       &= c^{-n}p(c) \\\\
       &= 0
\end{align\*}
这表明 \\(1/c\\) 是 \\(q(x)\\) 的根，所以 \\(1/c\\) 是 \\(F\\) 的代数元。

---

__D.3__ 如果 \\(cd\\) 是 \\(F\\) 上的代数元，那么 \\(c\\) 是 \\(F(d)\\) 上的代数元。如果 \\(c+d\\) 是 \\(F\\) 上的代数元，那么 \\(c\\) 是 \\(F(d)\\) 上的代数元。（假定 \\(c\ne0\\) 且 \\(d\ne0\\)。）

**证明** 设 \\(p(x) \in F[x]\\) 是 \\(cd\\) 的最小多项式。设
\\[p(x) = p_0+p_1x+\cdots+p_nx^n\\]
那么 \\(p(cd) = 0\\)。设 \\(K=F(d)\\) 且
\\[q(x) = p_0 + (p_1d)x + \cdots + (p_nd^n)x^n\\]
则 \\(q(x) \in K[x]\\)，而在 \\(K(c)\\) 中有
\\[q(c) = p_0 + p_1(cd) + \cdots + p_0(cd)^n = p(cd) = 0\\]
这表明 \\(c\\) 是 \\(q(x)\\) 的根，所以 \\(c\\) 是 \\(F(d)\\) 的代数元。

另一个证明类似，设 \\(q(x) = p(x+d)\\) 即可。

---

__D.4__ 如果 a 在 F 上的最小多项式的次数是 1，那么 a &isin; F，反之亦然。

**证明** 设 a 的最小多项式是 p(x)，那么只可能是 p(x) = x - a。因为 p(x) &isin; F[x]，所以 -a &isin; F，所以 a &isin; F。

反过来，如果 a &isin; F，则可以构造 p(x) = x - a，使得 a 是 p(x) 的根。这里 p(x) 是首一式，并且是次数最小的根为 a 的多项式，所以 p(x) 是 a 在 F 上的最小多项式，并且次数是 1。

---

__D.5__ 设 F &sube; K 且 a &isin; K。如果 p(x) 是 F[x] 中的首一不可约多项式，并且 p(a) = 0，那么 p(x) 是 a 在 F 上的最小多项式。

**证明** 考虑同态 &sigma;<sub>a</sub>: F[x] &rarr; K 定义为 &sigma;<sub>a</sub>(&alpha;(x)) = &alpha;(a)。则 ker &sigma;<sub>a</sub> = J<sub>a</sub> = &langle;m(x)&rangle;，其中 m(x) 如果取为首一多项式，则它就是 a 在 F 上的最小多项式（根据第二十五章习题 D.2 可知，m(x) 是唯一的）。

J<sub>a</sub> 中的任何其他多项式都可以写成 p(x)m(x)，因此或者可约，或者不是首一式。这表明 J<sub>a</sub> 中的首一不可约多项式只有 m(x)。而 p(x) &isin; J<sub>a</sub> 是首一不可约多项式，这表明 p(x) = m(x)。由此可知 p(x) 就是 a 在 F 上的最小多项式。

---

__D.6__ 指出一个包含多项式 x⁵+2x³+4x²+6 的根的（除 &Ropf;、&Qopf; 外的）域。

**答** 根据 Eisenstein 判据（取 p = 2）可知多项式 p(x) = x⁵+2x³+4x²+6 不可约。那么，根据域扩张基本定理可知 &Qopf;[x]/&langle;p(x)&rangle; 满足要求。

---

__D.7__ 求证：\\(\Q(1+i) \cong \Q(1-i)\\)。但是，\\(\Q(\sqrt2) \ncong \Q(\sqrt3)\\)。

**说明** 原书中这里有排版错误，第二个式子应为不同构符号。

**证明** 设 \\(p(x)=x^2-2x+2\\) 是不可约多项式，那么 \\(1+i\\) 和 \\(1-i\\) 是它的根。因此 \\(\Q(1+i) \cong \Q[x]/\langle p(x)\rangle \cong \Q(1-i)\\)。

假设存在同构 \\(h\colon \Q(\sqrt2) \to \Q(\sqrt3)\\)，那么 \\(h(1) = 1\\)。于是
\begin{gather\*}
h(2) = h(1) + h(1) = 1+1 = 2\\\\
h(2) = h(\sqrt2 \cdot \sqrt2) = h(\sqrt2)h(\sqrt2)
\end{gather\*}
记 \\(x=h(\sqrt2)\\)，则 \\(x^2 = 2\\)。因为 \\(\Q(\sqrt3)\\) 中的元素总是 \\(a+b\sqrt3\\) 的形式，因此设
\\[x=a+b\sqrt3\qquad(a,b\in\Q)\\]
那么 \\(a^2+2\sqrt3+3b^2=2\\)，解得 \\[\sqrt3 = \frac{2-a^2-3b^2}{2}\\]
这是不可能的，因为等式左边是无理数，右边是有理数。因此假设不成立，所以 \\(\Q(\sqrt2) \ncong \Q(\sqrt3)\\)。

---

__D.8__ 如果二次式 p(x) 不可约，证明 F[x]/&langle;p(x)&rangle; 包含 p(x) 的*两个*根。

**证明** 设 c 是 p(x) 的一个根，并设 K=F(c)。那么 K &cong; F[x]/&langle;p(x)&rangle;。在 K[x] 中因为 (x-c) | p(x)，所以 p(x) = q(x)(x-c)，其中 q(x) &isin; K[x]。易知 q(x) 是一次式，不妨设 q(x) = x-d。那么 p(x) 在 K[x] 中可以分解为 (x-c)(x-d)。这表明 p(x) 在 K 中有两个根：c 和 d。这就是所要证明的。

## E. 单扩张

回顾 F(a) 的定义。它是满足如下条件的域：(i) F &sube; F(a); (ii) a &isin; F(a); (iii) 任意包含 F 和 a 的域都包含 F(a)。

用这个定义证明 1--5，其中 F &sube; K, c &isin; F, 并且 a &isin; K。

__E.1__ F(a) = F(a+c) 且 F(a) = F(ca)。（假定 c &ne; 0。）

**证明** 设 E = F(a+c)。

1. 由 (i) 得 F &sube; E。
2. 由 (ii) 得 a+c &isin; E。而 c &isin; F &sube; E，根据域对减法的封闭性可知 c = (a+c)-c &isin; E。
3. 设 F &sube; E' 并且 a &isin; E'。那么有 c &isin; E &sube; E'，根据域多加法的封闭性可知 a+c &isin; K。由 (iii) 可知 E &sube; E'。

根据定义可知 E = F(a)，即 F(a+c) = F(a)。

F(a) = F(ca) 同理，使用域对乘法和除法封闭的性质即可。

---

__E.2__ F(a²) &sube; F(a) 并且 F(a+b) &sube; F(a, b)。[F(a, b) 是包含 F, a, b 的域并且被任意包含 F, a, b 的域包含。] 为什么反向包含不成立？

**证明**

* 因为 F &sube; F(a) 并且 a² &isin; F(a)，根据 (iii) 可知 F(a²) &sube; F(a)。
   * 反过来不成立，例如 &Qopf;(2) &ne; &Qopf;(&radic;2)。
* 因为 F &sube; F(a, b) 并且 a+b &isin; F(a, b)，根据 (iii) 可知 F(a+b) &sube; F(a, b)。
   * 反过来不成立，例如 &Qopf;(0) &ne; &Qopf;(-&radic;2,&radic;2)。

---

__E.3__ a+c 是 p(x) 的根当且仅当 a 是 p(x+c) 的根；ca 是 p(x) 的根当且仅当 a 是 p(cx) 的根。

**证明**

* a+c 是 p(x) 的根 &hArr; p(a+c) = 0 &hArr; a 是 p(x+c) 的根。
* ca 是 p(x) 的根 &hArr; p(ca) = 0 &hArr; a 是 p(cx) 的根。

---

__E.4__ 设 p(x) 不可约，并设 a 是 p(x+c) 的根。那么 F[x]/&langle;p(x+c)&rangle; &cong; F(a) 并且 F[x]/&langle;p(x)&rangle; &cong; F(a+c)。做出结论：F[x]/&langle;p(x+c)&rangle; &cong; F[x]/&langle;p(x)&rangle;。

**证明** 由 E.3 可知 F[x]/&langle;p(x+c)&rangle; &cong; F(a) 并且 F[x]/&langle;p(x)&rangle; &cong; F(a+c)。由 E.1 可知 F(a) &cong; F(a+c)。所以结论成立。

---

__E.5__ 设 p(x) 不可约，并设 a 是 p(cx) 的根。那么 F[x]/&langle;p(cx)&rangle; &cong; F(a) 并且 F[x]/&langle;p(x)&rangle; &cong; F(ca)。做出结论：F[x]/&langle;p(cx)&rangle; &cong; F[x]/&langle;p(x)&rangle;。

**证明** 仿照 E.4 即可。

---

__E.6__ 使用 E.4 和 E.5 证明下列命题：

1. &Zopf;<sub>11</sub>\[x\]/&langle;x^2+1&rangle; &cong; &Zopf;<sub>11</sub>\[x\]/&langle;x^2+x+4&rangle; 。
2. 如果 a 是 x²-2 的根且 b 是 x²-4x+2 的根，那么 &Qopf;(a) &cong; &Qopf;(b)。
3. 如果 a 是 x²-2 的根且 b 是 x²-1/2 的根，那么 &Qopf;(a) &cong; &Qopf;(b)。

**证明** 只要证明一个多项式通过 E.4 或 E.3 中的换元可以变成另一个多项式即可。

1. 在 &Zopf;<sub>11</sub>\[x\] 中有 (x+6)²+1 = x²+x+4。
2. (x-2)²-2 = x²-4x+2。
3. (2x)²-2 = 4(x²-1/2)。

## F. 二次扩张

如果 \\(a\\) 在 \\(F\\) 上的最小多项式的次数是 2，称 \\(F(a)\\) 是 \\(F\\) 的二次扩张 (quadratic extension)。

__F.1__ 求证：如果 \\(F\\) 的特征不为 \\(2\\)，那么 \\(F\\) 的任意二次扩张都是 \\(F(\sqrt a)\\) 的形式，其中 \\(a\in F\\)。

**证明** 因为有限域的特征不为 \\(2\\)，因此 \\(2\ne 0\\) 并且 \\(4\ne 0\\)。设 \\[p(x) = x^2+ux+v = \left(x+\frac12u\right)^2 - \left(\frac14u^2-v\right)\qquad (u,v\in F)\\]
做代换 \\(a = \frac14u^2-v\\) 和 \\(b = -\frac12u\\)，则 \\(p(x) = (x-b)^2-a\\) 且 \\(a,b\in F\\)。
设 \\(p(x)\\) 在扩域 \\(K=F[x]/\langle p(x)\rangle\\) 中的一个根 \\(c = b+\sqrt a\\)，那么 \\(K\cong F(c) = F(b+\sqrt a)\\)。根据 E.4 可知 \\(F(b+\sqrt a)\cong F(\sqrt a)\\)，因此 \\(F[x]/\langle p(x)\rangle \cong F(\sqrt a)\\)。

---

设 \\(F\\) 为有限域，并且 \\(F^\ast\\) 是其非零元素构成的乘法群。显然 \\(H=\\{x^2:x\in F^\ast\\}\\) 是 \\(F^\ast\\) 的一个子群；因为每个 \\(x^2\in F^\ast\\) 都只能是两个元素（即 \\(\pm x\\)）的平方，所以 \\(F^\ast\\) 中恰好有一半的元素在 \\(H\\) 中。因此，\\(H\\) 恰有两个陪集：它本身，包含了所有的平方元素，以及 \\(aH\\) （其中 \\(a\notin H\\)），包含所有的非平方元素。如果 \\(a\\) 和 \\(b\\) 是非平方元素，那么根据第十五章定理 5(i) 有
\\[ab^{-1} = \frac ab \in H\\]
于是：如果 \\(a\\) 和 \\(b\\) 是非平方元素，那么 \\(a/b\\) 是平方元素。使用这些注解解答下列问题。

__F.2__ 设 \\(F\\) 为有限域。如果 \\(a,b\in F\\)，设 \\(p(x) = x^2 - a\\) 且 \\(q(x) = x^2 - b\\) 是 \\(F[x]\\) 中的不可约多项式，并设 \\(\sqrt a\\) 和 \\(\sqrt b\\) 分别表示 \\(p(x)\\) 和 \\(q(x)\\) 在 \\(F\\) 的某个扩域中的根。解释为什么 \\(a/b\\) 是平方元素，即 \\(a/b=c^2\\)，其中 \\(c\in F\\)。证明 \\(\sqrt b\\) 是 \\(p(cx)\\) 的根。

**证明** 首先，\\(a\\) 和 \\(b\\) 都不是 \\(F\\) 中的平方数；否则 \\(p(x)\\) 或 \\(q(x)\\) 可约。根据上面提供的信息可知 \\(a/b\\) 是平方数。设 \\(a/b = c^2\\)，其中 \\(c \in F\\)，那么 \\(a = c^2b\\)。

由此可知 \\(\sqrt a = c \sqrt b\\)（注：在 \\(a/b=c^2\\) 中，把 \\(c\\) 替换成 \\(-c\\) 也成立，所以我们总是可以选择一个恰当的 \\(c\\) 使得这个式子右边不会出现负号）。因为 \\(\sqrt a\\) 是 \\(p(x)\\) 的根，所以 \\(p(\sqrt a) = p(c\sqrt b) = 0\\)，即 \\(c\sqrt b\\) 是 \\(p(x)\\) 的根。根据 E.3 可知 \\(\sqrt b\\) 是 \\(p(cx)\\) 的根。

---

__F.3__ 使用 F.2 证明 \\(F[x]/\langle p(cx)\rangle \cong F(\sqrt b)\\)；然后使用 E.5 做出结论：\\(F(\sqrt a) \cong F(\sqrt b)\\)。

**证明** 根据 F.2 可知 \\(\sqrt b\\) 是 \\(p(cx)\\) 的根，并且 \\(p(x)\\) 不可约，从而有 \\(p(cx)\\) 不可约。所以 \\(F[x]/\langle p(cx)\rangle \cong F(\sqrt b)\\)。

另一方面，根据 E.5 可知 \\(F[x]/\langle p(cx)\rangle \cong F[x]/\langle p(x)\rangle\\)。又因为 \\(\sqrt a\\) 是 \\(p(x)\\) 的根，所以 \\(F[x]/\langle p(x)\rangle \cong F(\sqrt a)\\)。这表明 \\(F(\sqrt a) \cong F(\sqrt b)\\)。

---

__F.4__ 根据 F.3 证明：有限域的任意两个二次扩张都同构。

**说明** 这里讨论的应当是真扩张，即严格包含 \\(F\\) 的扩张。

**证明** 根据 F.1 可知特征不为 \\(2\\) 的有限域 \\(F\\) 的二次真扩张都是 \\(F(\sqrt a)\\) 的形式。设任意两个二次扩张分别是 \\(F(\sqrt a)\\) 和 \\(F(\sqrt b)\\)，那么根据 F.3 可知它们同构。

如果 \\(F\\) 的特征为 \\(2\\)，那么它不存在二次不可约多项式。

这就证明了结论。

---

__F.5__ 如果 \\(a\\) 和 \\(b\\) 是 \\(\mathbb R\\) 中的非平方元素，那么 \\(a/b\\) 是平方元素（为什么？）。使用和 F.4 同样的论证来证明 \\(\mathbb R\\) 的任意两个单扩张都同构（从而都同构于 \\(\mathbb C\\)）。

**说明** 这里讨论的应当是真扩张，即严格包含 \\(\mathbb R\\) 的扩张。

**证明** 在 \\(\mathbb R\\) 中，当 \\(x\ge0\\) 时，它有平方根 \\(\sqrt x\\)，因而 \\(x\\) 是平方元素。

设 \\(a\\) 和 \\(b\\) 都是非平方元素，那么 \\(a,b<0\\)。于是 \\(a/b > 0\\)，这说明 \\(a/b\\) 是平方元素。

根据代数基本定理，\\(\mathbb R[x]\\) 中的不可约多项式只有一次和二次式，所以 \\(\mathbb R\\) 只有一次和二次扩张。因为域的一次扩张等于它本身，所以我们只考虑二次扩张。

仿照 F.1--F.4 可以证明，\\(\mathbb R\\) 的任意两个二次扩张都同构。另外，\\(\mathbb C = \mathbb R(i) \cong \mathbb R[x] / \langle x^2+1\rangle\\) 是二次扩张，所以 \\(\mathbb R\\) 的任意单扩张都和 \\(\mathbb C\\) 同构。

## G. 和超越元有关的问题

设 F 是域，并且 c 是 F 上的超越元。证明下列命题。

__G.1__ {a(c) : a(x) &isin; F[x]} 是和 F[x] 同构的整环。

**证明** 设 A = {a(c) : a(x) &isin; F[x]}。设同态 &sigma;<sub>c</sub>: F[x] &rarr; A 定义为 &sigma;<sub>c</sub>(a(x)) = a(c)。那么根据定义可知 &sigma;<sub>c</sub> 是满射。

设 a(c)=b(c) &isin; A，其中 a(x) 和 b(x) 是 F[x] 中的非零多项式。构造多项式 p(x) = a(x) - b(x)，那么 p(x) &isin; F[x]。而 p(c) = a(c) - b(c) = 0，这表明 c 是 p(x) 的根。但 c 是 F 上的超越元，所以 p(x) 不可能是非零多项式。于是 p(x) = 0，从而 a(x) = b(x)。这表明 &sigma;<sub>c</sub> 是单射。

综上所述，&sigma;<sub>c</sub> 是 F[x] 到 A 的同构，从而有 A &cong; F[x]，是一个整环。

---

__G.2__ \\(F(c)\\) 是 \\(\\{a(c) : a(x) \in F[x]\\}\\) 的分式域，并且同构于 \\(F(x)\\)，即 \\(F[x]\\) 的分式域。

**说明** 关于分式域，可以参考第二十四章的习题 I 和第二十章的选读部分。

**证明** 设 \\(A=\\{a(c) : a(x) \in F[x]\\}\\) 的分式域为 \\(Q\\)。根据定义可知 \\(Q\\) 中任意元素可以表示为
\\[\frac{\alpha(c)}{\beta(c)}\qquad\text{其中 }\alpha(x),\beta(x) \in F[x]\text{ 且 }\beta(x)\ne0\\]
因为 \\(F(c)\\) 是包含 \\(F\\) 和 \\(c\\) 的域，根据域对加法和乘法的封闭性可知 \\(\alpha(c),\beta(c)\in F(c)\\)；再根据域对除法的封闭性可知 \\(\frac{\alpha(c)}{\beta(c)}\in F(c)\\)。这说明 \\(Q\subseteq F(c)\\)。

另一方面，因为 \\(F\subseteq Q\\)，并且 \\(c\in Q\\)，所以 \\(F(c) \subseteq Q\\)。这就证明了 \\(F(c)=Q\\) 是 \\(A\\) 的分式域。

设映射 \\(h\colon F(x) \to F(c)\\) 定义为 \\(h\bigl({\alpha(x)\over \beta(x)}\bigr) = {\alpha(c)\over \beta(c)}\\)。那么，对于任意 \\({\alpha(c)\over\beta(c)}\in F(c)\\)，其中 \\(\alpha(x),\beta(x)\in F[x]\\) 且 \\(\beta(x)\ne0\\)，存在分式 \\({\alpha(x)\over\beta(x)}\in F(x)\\) 使得 \\(h\bigl({\alpha(x)\over\beta(x)}\bigr) = {\alpha(c)\over\beta(c)}\\)。这说明 \\(h\\) 是满射。

另一方面，设 \\(h\bigl(\frac{\alpha(x)}{\beta(x)}\bigr)=h\bigl(\frac{\gamma(x)}{\delta(x)}\bigr)\\)，其中 \\(\alpha(x),\beta(x),\gamma(x),\delta(x)\in F[x]\\) 且 \\(\beta(x),\delta(x) \ne 0\\)。那么有
\\[\frac{\alpha(c)}{\beta(c)}=\frac{\gamma(c)}{\delta(c)} \Longrightarrow \alpha(c)\delta(c)=\beta(c)\gamma(c)\\]
构造多项式 \\(p(x) = \alpha(x)\delta(x)-\beta(x)\gamma(x)\\)，则 \\(p(x)\in F[x]\\)。而 \\(p(c) = 0\\)，这表明 \\(c\\) 是 \\(p(x)\\) 的根。但 \\(c\\) 是 \\(F\\) 上的超越元，所以 \\(p(x)\\) 只可能是零多项式，即 \\(p(x)=0\\)。于是
\\[\alpha(x)\delta(x)=\beta(x)\gamma(x) \Longrightarrow \frac{\alpha(x)}{\beta(x)}=\frac{\gamma(x)}{\delta(x)}\\]
这就证明了 \\(h\\) 是单射。

综上所述，\\(h\\) 是 \\(F(x)\\) 到 \\(F(c)\\) 的同构。因此 \\(F(x)\cong F(c)\\)。

---

__G.3__ 如果 c 是 F 上的超越元，那么 c+1, kc（其中 k &isin; F 且 k &ne; 0）和 c² 也是超越元。

**证明** 只须证明逆否命题即可。

* c+1 是 p(x) 的根 &rArr; c 是 p(x+1) 的根
* kc 是 p(x) 的根 &rArr; c 是 p(kx) 的根
* c² 是 p(x) 的根 &rArr; c 是 p(x²) 的根

---

__G.4__ 如果 \\(c\\) 是 \\(F\\) 上的超越元，那么 \\(F(c)\\) 中任意不在 \\(F\\) 中的元素都是在 \\(F\\) 上的超越元。

**证明** 考虑 \\(F(c)\\) 中的任意元素 \\(a={\alpha(c)\over\beta(c)}\\)，其中 \\(\alpha(x),\beta(x)\in F[x]\\) 且 \\(\beta(x)\ne0\\)。只须考虑 \\(\alpha(x),\beta(x)\\) 没有公因式的情况即可。

显然代数元 \\(0\in F\\)。假设 \\(a\ne0\\) 是 \\(F\\) 上的代数元，那么 \\(\alpha(x) \ne 0\\)，且存在多项式 \\(p(x)\in F[x]\\) 使得 \\(p(a) = 0\\)，即
\\[p_0+p_1\frac{\alpha(c)}{\beta(c)}+\cdots+p_n\biggl(\frac{\alpha(c)}{\beta(c)}\biggr)^n=0\\]
两边乘 \\(\beta(c)^n\\) 得
\\[p_0\beta(c)^n+p_1\alpha(c)\beta(c)^{n-1}+\cdots+p_n\alpha(c)^n=0\\]
构造 \\(q(x) = p_0\beta(x)^n+p_1\alpha(x)\beta(x)^{n-1}+\cdots+p_n\alpha(x)^n\\)，则 \\(q(x)\in F[x]\\)。
因为 \\(q(c) = 0\\)，而 \\(c\\) 是 \\(F\\) 上的超越元，所以 \\(q(x)\\) 只可能是零多项式。即
\\[p_0\beta(x)^n+p_1\alpha(x)\beta(x)^{n-1}+\cdots+p_n\alpha(x)^n=0\\]
于是
\\[p_0\beta(x)^n=-\alpha(x)\bigl(p_1\beta(x)^{n-1}+\cdots+p_n\alpha(x)^{n-1}\bigr)\\]
这表明 \\(\alpha(x)\mid\beta(x)^n\\)。但 \\(\alpha(x),\beta(x)\\) 互质，所以只可能是 \\(\alpha(x) \mid 1\\)，即 \\(\alpha(x)\\) 是零次多项式。同理可证 \\(\beta(x)\\) 是零次多项式。因为常数项一定是 \\(F\\) 中的元素，所以 \\(\alpha(c),\beta(c)\in F\backslash\\{0\\}\\)。根据域对除法封闭可知 \\(\frac{\alpha(c)}{\beta(c)}\in F\\)。

这就证明了 \\(F(c)\\) 的任意代数元都在 \\(F\\) 中。因此，\\(F(c)\\) 中任意不在 \\(F\\) 中的元素都是 \\(F\\) 的超越元。

## H. 两个多项式的公因式：在 F 上和在 F 的扩域上

设 F 为域，并设 a(x),b(x) &isin; F[x]。证明下列命题。

__H.1__ 如果 a(x) 和 b(x) 在 F 的某个扩域中有共同的根 c，那么它们在 F[x] 中有正次数的公因式。

**解** 易知 c 是 F 上的代数元。设 &sigma;<sub>c</sub>: F[x]&rarr;K 定义为 &sigma;<sub>c</sub>(a(x)) = a(c)，并设它的理想为 J<sub>c</sub>。那么 J<sub>c</sub>=&langle;m(x)&rangle;，其中 m(x) 是不可约多项式（因此具有正的次数）。因为 a(c)=b(c)=0，所以 a(x),b(x) &isin; J<sub>c</sub>。这表明 m(x) | a(x) 且 m(x) | b(x)，所以 m(x) 是 a(x) 和 b(x) 的公因式。

---

__H.2__ 如果 a(x) 和 b(x) 在 F[x] 中互质，则它们在 K[x] 中也互质，其中 K 是 F 的任意扩域。反过来，如果 a(x) 和 b(x) 在 K[x] 中互质，则它们在 F[x] 中也互质。

**证明** 如果 a(x) 和 b(x) 在 K[x] 中有不互质，即它们有正次数的公因式 d(x) &isin; K[x]。根据域扩张基本定理，d(x) 在 K 的某个扩域 L 中有根。因为 F &sube; K &sube; L，所以 L 也是 F 的扩域。根据 H.1 可知 a(x) 和 b(x) 在 F[x] 中有正次数的公因式，即它们不互质。

反过来，如果 a(x) 和 b(x) 在 F[x] 中不互质，则它们有正次数的公因式 d(x) &isin; F[x] &sube; K[x]。因此 a(x) 和 b(x) 在 K[x] 中也有同样的公因式 d(x)，所以在 K[x] 中也不互质。

## I. 导数和它们的性质

设 \\(a(x) = a_0+a_1x+\cdots+a_nx^n \in F[x]\\)。\\(a(x)\\) 的*导数 (derivative)* 由下面的多项式 \\(a'(x) \in F[x]\\) 定义：
\\[a'(x) = a_1+2a_2x+\cdots+na_nx^{n-1}\\]
（这和微积分中多项式的导数是一样的。）我们现在证明微分的形式法则，和微积分中所熟知的一样。

设 \\(a(x),b(x)\in F[x]\\)，并设 \\(k\in F\\)。

证明 1--4。

__I.1__ \\([a(x)+b(x)]' = a'(x) + b'(x)\\)

**证明** 比较两边 \\(k\\) 次项系数，它们都等于 \\(k(a_k+b_k)\\)。

---

__I.2__ \\([a(x)b(x)]' = a'(x)b(x) + a(x)b'(x)\\)

**证明** 设 \\(c(x) = a(x)b(x)\\)，则
\\[c_k=\sum_{i+j=k}a_ib_j\\]
于是
\begin{align\*}
c_k'&=k\sum_{i+j=k}a_ib_j\\\\
    &=\sum_{i+j=k}(i+j)a_ib_j\\\\
    &=\sum_{i+j=k}(ia_i)b_j + \sum_{i+j=k}a_i(jb_j)\\\\
    &=\sum_{i+j=k}(ia_i)b_j + \sum_{i+j=k}a_i(jb_j)\\\\
\end{align\*}
等式右边两项分别是 \\(a'(x)b(x)\\) 和 \\(a(x)b'(x)\\) 的 \\(k\\) 次项系数。

---

__I.3__ \\([ka(x)]' = ka'(x)\\)

**证明** 比较两边 \\(n\\) 次项系数，它们都等于 \\(nka_n\\)。

---

__I.4__ 如果 F 的特征 p &ne; 0 并且 a'(x) = 0，那么 a(x) 是常数多项式。为什么这个结论对特征 p &ne; 0 的域 F 不一定成立？

**证明** 零多项式是常数多项式。下面只考虑非零多项式。设 a(x) 的次数是 n，则 a<sub>n</sub> &ne; 0。假设 n > 0，那么在 a'(x) 中，第 n-1 次项的系数是 na<sub>n</sub> &ne; 0，因为 F 的特征等于 0。于是 a'(x) 不是零多项式，和已知条件矛盾！因此假设不成立，只可能是 n = 0。这意味着 a(x) 是常数多项式。

如果特征 p &ne; 0，那么取 a(x) = x<sup>p</sup>，则 a'(x) = px<sup>p-1</sup> = 0。这就构成了一个反例。

---

__I.5__ 求下列 &Zopf;₅[x] 中的多项式的导数：

1. x⁶+2x³+x+1
2. x⁵+3x²+1
3. x<sup>15</sup>+3x<sup>10</sup>+4x⁵+1

**解**

1. [x⁶+2x³+x+1]' = x⁵+x²+1
2. [x⁵+3x²+1]' = x
3. [x<sup>15</sup>+3x<sup>10</sup>+4x⁵+1]' = 0

---

__I.6__ 如果 F 的特征 p &ne; 0，并且 a'(x) = 0，证明 a(x) 中的非零项都具备 a<sub>mp</sub>x<sup>mp</sup> 的形式，其中 m 为某个自然数。[也就是说，a(x) 是 a<sup>p</sup> 的幂构成的多项式。]

**证明** 设 a(x) 中的各项的形式为 a<sub>n</sub>x<sup>n</sup>。那么，根据整数带余除法可设 n=pq+r，其中 0&le;q&lt;r。那么，这一项在 a'(x) 中对应的项为 na<sub>n</sub>x<sup>n-1</sup>。因为 a'(x) = 0，所以 na<sub>n</sub> = ra<sub>n</sub> = 0。当 r &ne; 0 时，有 a<sub>n</sub>=0。这说明所有 x 的不是 p 的整数倍次幂的项的系数都为零。

## J. 重根

设 a(x) &isin; F[x]，且 K 是 F 的扩域。元素 c &isin; K 称为 a(x) 的重根，如果存在 m>1 使得 (x-c)<sup>m</sup> | a(x)。了解多项式的所有根是否是不同的经常是很重要的。现在考察一种确定任意多项式 a(x) &isin; F[x] 在 F 的任意扩域中是否有根的方法。

设 K 是包含 a(x) 的所有根的域。假设 a(x) 有一个重根 c。

__J.1__ 证明 a(x) = (x-c)²q(x) &isin; K[x]。

**证明** 根据定义，存在 m>1 使得 (x-c)<sup>m</sup> | a(x)。于是存在 p(x) &isin; K[x] 使得 a(x) = (x-c)<sup>m</sup>p(x)。令 q(x) = (x-c)<sup>m-2</sup>p(x)，则 a(x) = (x-c)²q(x)。

---

__J.2__ 使用 J.1 的结论求 a'(x)。

**解** a'(x) = [(x-c)²]\'q(x) + (x-c)²q\'(x) = (x-c)[2q(x) + (x-c)q'(x)]

---

__J.3__ 证明 x-c 是 a(x) 和 a'(x) 的公因式。使用 H.1 作出结论：a(x) 和 a'(x) 在 F[x] 中有正次数的公因式。

**说明** 原书此处似有误，“正次数”原文为“次数大于 1”，但这个结论应该不成立。后续的问题中有类似的问题。

**证明** 由 J.1 知 (x-c) | a(x)；由 J.2 知 (x-c) | a'(x)。所以 x-c 是 a(x) 和 a'(x) 的公因式。根据 H.1 可知，a(x) 和 a'(x) 在 F[x] 中有正次数的公因式。

---

于是，如果 a(x) 有重根，则 a(x) 和 a'(x) 在 F[x] 中有公因式。要证明逆命题，假设 a(x) *没有*重根。那么 a(x) 可以分解为 a(x) = (x-c₁)&middot;&middot;&middot;(x-c<sub>n</sub>)，其中 c₁,...,c<sub>n</sub> 互不相同。

__J.4__ 说明为什么 a'(x) 的每一项的形式都是 (x-c₁)&middot;&middot;&middot;(x-c<sub>i-1</sub>)(x-c<sub>i+1</sub>)&middot;&middot;&middot;(x-c<sub>n</sub>) 的形式。

**证明** 根据导数的乘法法则可证。过程可使用数学归纳法（略）。

---

__J.5__ 使用 J.4，说明为什么 a(x) 的所有根 c₁,...,c<sub>n</sub> 都不是 a'(x) 的根。

**证明** 对于任意 c<sub>i</sub>，(x-c<sub>i</sub>) 整除 a'(x) 中除 (x-c₁)&middot;&middot;&middot;(x-c<sub>i-1</sub>)(x-c<sub>i+1</sub>)&middot;&middot;&middot;(x-c<sub>n</sub>) 以外的所有项。因此 (x-c<sub>i</sub>)&nmid;a'(x)，于是 c<sub>i</sub> 不是 a'(x) 的根。

---

__J.6__ 作出结论：a(x) 和 a'(x) 没有正次数的公因式。

**证明** 根据 J.5 可知，a(x) 的任意正次数因式都不能整除 a'(x)，所以 a(x) 和 a'(x) 没有公因式。

---

这个重要的结果叙述如下：*F[x] 中的多项式 a(x) 有重根当且仅当 a(x) 和 a'(x) 有正次数的公因式。*

__J.7__ 证明下列多项式在它的系数域的任意扩域中没有重根。

1. x³-7x²+8 &isin; &Qopf;\[x\]
2. x²+x+1 &isin; &Zopf;₅\[x\]
3. x<sup>100</sup>-1 &isin; &Zopf;₇\[x\]

**证明**

1. 设 a(x) = x³-7x²+8，则 a'(x) = 3x²-14x = (3x-14)x。a'(x) 的所有根为：14/3 和 0；代入知它们都不是 a(x) 的根。所以 a(x) 和 a'(x) 没有正次数的公因式，于是 a(x) 没有重根。
2. 设 a(x) = x²+x+1，则 a'(x) = 2x+1。一次式 a'(x) 只有一个根，在 &Zopf;₅ 中为 2；代入可知它不是 a(x) 的根。所以 a(x) 和 a'(x) 没有正次数的公因式，于是 a(x) 没有重根。
3. 设 a(x) = x<sup>100</sup>-1，则 a'(x) = 0。显然 a(x) 和 a'(x) 没有正次数的公因式，于是 a(x) 没有重根。
