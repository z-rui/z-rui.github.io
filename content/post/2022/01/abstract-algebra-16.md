---
title: 抽象代数习题(16) -- 同态基本定理
date: 2022-01-17T15:23:18-08:00
---

《抽象代数》第十六章是群论部分的最后一章。这一章讲了同态基本定理 (fundamental homomorphism theorem, FHT)。它把商群和同态两个概念联系起来：由任意正规子群 H 可以构造商群 G/H，从而构造从 G 到 G/H、以 H 为核的同态；反之亦然。

本章习题部分放进了许多补充内容，包括群论中的一些著名定理，如 Cauchy 定理、Sylow 定理等。这部分内容我没有看完，所以先只选做其中较简单的一部分习题。

<!--more-->

## B. FHT 应用于 \\(\mathscr F(\mathbb R)\\) 的例子

设 \\(\alpha: \mathscr F(\mathbb R) \to \mathbb R\\) 定义为 \\(\alpha(f) = f(1)\\)，\\(\beta: \mathscr F(\mathbb R) \to \mathbb R\\) 定义为 \\(\beta(f) = f(2)\\)。

__B.1__ 证明 \\(\alpha\\) 和 \\(\beta\\) 是 \\(\mathscr F(\mathbb R)\\) 到 \\(\mathbb R\\) 的*满*同态。

**说明** \\(\mathscr F(\mathbb R)\\) 是所有 \\(\mathbb R\to \mathbb R\\) 函数的集合。群运算为函数加法：\\((f+g)(x) = f(x) + g(x)\\)。

**证明** 只证明 \\(\alpha\\) 是满同态，\\(\beta\\) 的情况类似。

设 \\(f,g\in\mathscr F(\mathbb R)\\)，则 \\[\alpha(f+g) = (f+g)(1) = f(1) + g(1) = \alpha(f) + \alpha(g)\\] 这说明 \\(\alpha\\) 是同态。

对任意 \\(c\in \mathbb R\\)，存在常数函数 \\(f(x) = c\\) 使得 \\(\alpha(f) = c\\)。因此 \\(\alpha\\) 是到 \\(\mathbb R\\) 的满射。

---

__B.2__ 设 \\(J\\) 是所有 \\(\mathbb R\to \mathbb R\\) 的图象通过点 \\((1,0)\\) 的所有函数的集合；设 \\(K\\) 是所有 \\(\mathbb R\to \mathbb R\\) 的图象通过点 \\((2,0)\\) 的所有函数的集合。使用 FHT 证明 \\(\mathbb R \cong \mathscr F(\mathbb R)/J\\) 以及 \\(\mathbb R\cong \mathscr F(\mathbb R)/K\\)。

**证明** 只证明关于 \\(J\\) 的命题；关于 \\(K\\) 的命题同理。

依题意 \\(J = \\{f\in \mathscr F(\mathbb R) : f(1) = 0\\}\\)。这表明 \\(J\\) 是 \\(\alpha\\) 的核。又因为 \\(\alpha\\) 是 \\(\mathscr F(\mathbb R)\to \mathbb R\\) 的满同态，根据 FHT 可知 \\(\mathbb R \cong \mathscr F(\mathbb R) / J\\)。

**注** 由此可知 \\(\mathscr F(\mathbb R)/J \cong \mathscr F(\mathbb R)/K\\) （下一问的回答）。

## C. FHT 应用于 Abel 群的例子

设 \\(G\\) 为 Abel 群。设 \\(H=\\{x^2 : x\in G\\}\\), \\(K=\\{x\in G : x^2=e\\}\\)。

__C.1__ 证明 \\(f(x) = x^2\\) 是 \\(G\\) 到 \\(H\\) 的满同态。

**证明** 显然 \\(f\\) 是 \\(G\to H\\) 的映射。

设 \\(x,y\in G\\) 则 \\(f(xy) = (xy)^2 = (x^2)(y^2) = f(x)f(y)\\)。这表明 \\(f\\) 是同态。

对任意 \\(h\in H\\)，由定义可知存在 \\(x\in G\\) 使得 \\(h = x^2\\)，于是 \\(f(x) = h\\)。这表明 \\(f\\) 是满射。

__C.2__ 求 \\(f\\) 的核。

**解** 列方程 \\(f(x) = e\\)，即 \\(x^2 = e\\)。可知方程的解集为 \\(K\\)。所以 \\(K\\) 是 \\(f\\) 的核。

__C.3__ 根据 FHT 说明 \\(H\cong G/K\\)。

**答** 根据前两问的结论和 FHT 直接得出结论。

## D. 群 G 的内自同构群

设 \\(G\\) 是群。\\(G\\) 的 _自同构 (automorphism)_ 指的是同构 \\(f: G\to G\\)。

__D.1__ 符号 \\(\DeclareMathOperator{\Aut}{Aut}\Aut(G)\\) 表示 \\(G\\) 的所有自同构的集合。通过证明 \\(\Aut(G)\\) 是 \\(S_G\\) 的子群来证明集合 \\(\Aut(G)\\) 与复合运算 \\(\circ\\) 构成一个群。

**证明** 见第九章习题I.4。

---

__D.2__ \\(G\\) 的*内自同构 (inner automorphism)* 指的是任意如下形式的函数 \\(\phi_a(x)\\)：
\\[\forall x\in G\quad \phi_a(x) = axa^{-1}\\]
证明 \\(G\\) 的每个内自同构都是 \\(G\\) 的自同构。

**证明** 显然 \\(\phi_a\\) 是 \\(G\to G\\) 的映射，并且由于存在反函数 \\(\phi_a^{-1}(x) = a^{-1}xa\\)，因此是双射。

设 \\(x,y\in G\\)，则
\\[\phi_a(xy) = a(xy)a^{-1} = (axa^{-1})(aya^{-1}) = \phi_a(x)\phi_a(y)\\]
因此 \\(\phi_a\\) 保持群结构，所以是同构。

\\(\phi_a\\) 是 \\(G\to G\\) 的同构，根据定义它是 \\(G\\) 的自同构。

---

__D.3__ 证明：对任意 \\(a,b\in G\\)，有
\\[\phi_a \circ \phi_b = \phi_{ab}\quad\text{以及}\quad (\phi_a)^{-1} = \phi_{a^{-1}}\\]

**证明** 考虑任意 \\(x\in G\\)，
\\[(\phi_a\circ\phi_b)(x) = \phi_a(\phi_b(x)) = a(bxb^{-1})a^{-1} = abx(ab)^{-1} = \phi_{ab}(x)\\]
这说明 \\(\phi_a\circ\phi_b = \phi_{ab}\\)。
\\[(\phi_a)^{-1}(x) = a^{-1}xa = \phi_{a^{-1}}(x)\\]
这说明 \\((\phi_a)^{-1} = \phi_{a^{-1}}\\)。

---

__D.4__ 设 \\(I(G)\\) 是 \\(G\\) 的所有内自同构的集合。也就是说，\\(G=\\{\phi_a : a\in G\\}\\)。利用 D.3 的结论证明 \\(I(G)\\) 是 \\(\Aut(G)\\) 的子群。

**证明** 由 D.2 知 \\(I(G)\subseteq \Aut(G)\\)。由 D.3 知 \\(I(G)\\) 中的元素对复合运算和取逆运算均封闭。所以 \\(I(G) \le \Aut(G)\\)。

---

__D.5__ \\(G\\) 的*中心 (center)* 指的是 G 中和所有元素都满足交换律的元素的集合，即
\\[C = \\{a\in G: ax=xa\\;\forall x\in G\\}\\]
证明 \\(a\in C\\) 当且仅当 \\(axa^{-1} = x\\) 对任意 \\(x\in G\\) 都成立。

**证明** \\(a\in C \iff \forall x\in G\\;(ax = xa) \\iff \forall x\in G\\;(axa^{-1} = x)\\)。

---

__D.6__ 设 \\(h: G\to I(G)\\) 定义为 \\(h(a) = \phi_a\\)。证明 \\(h\\) 是从 \\(G\\) 到 \\(I(G)\\) 的满同态并且其核为 \\(C\\)。

**证明** 设 \\(a,b\in G\\)，则 \\(h(ab) = \phi_{ab} = \phi_a \circ \phi_b = h(a)\circ h(b)\\)。这说明 \\(h\\) 是同态。

对于任意 \\(I(G)\\) 的元素 \\(\phi_a\\)，显然 \\(h(a) = \phi_a\\)。这表明 \\(h\\) 是满射。

---

__D.7__ 根据 FHT 说明 \\(I(G)\\) 与 \\(G/C\\) 同构。

**答** 根据前一问的结论和 FHT 直接得到结论。

## E. FHT 应用于群的直积

设 \\(G\\) 和 \\(H\\) 是群。设 \\(J\triangleleft G\\), \\(K\triangleleft H\\)。

__E.1__ 证明函数 \\(f(x,y) = (Jx, Ky)\\) 是 \\(G\times H\to (G/J)\times(H/K)\\) 的同态。

**证明** 首先明确 \\(G/J\\) 中的运算为陪集的乘法：\\(Jx\cdot Jy = J(xy)\\)；\\(H/K\\) 中同理。根据商群的定义可知 \\(Jx\in (G/J)\\), \\(Ky\in (H/K)\\) 因此 \\(f\\) 是 \\(G\times H\to (G/J)\times(H/K)\\) 的映射。

设 \\((x_1,y_1),(x_2,y_2)\in G\times H\\)，则
\begin{align\*}
f((x_1,y_1)(x_2,y_2)) &= f(x_1x_2,y_1y_2) \\\\
&=(J(x_1x_2),K(y_1y_2)) \\\\
&=(Jx_1\cdot Jx_2),Ky_1\cdot Ky_2) \\\\
&=(Jx_1,Ky_1)\cdot(Jx_2,Ky_2) \\\\
&=f(x_1,y_1)\cdot f(x_2,y_2)
\end{align\*}
这说明 \\(f\\) 是同态。

---

__E.2__ 求 \\(f\\) 的核。

**解** 值域中的单位元为 \\((J,K)\\)。列方程 \\(f(x,y) = (J,K)\\) 得方程组
\begin{gather\*}
Jx = J\\\\
Ky = K\\\\
\end{gather\*}
由此可知解集为 \\(\\{(x,y) : x\in J, y\in K\\}\\)，即 \\(J\times K\\)。这就是 \\(f\\) 的核。

---

__E.3__ 根据 FHT 说明 \\((G\times H) / (J\times K) \cong (G/J) \times (H/K)\\)。

**答** 由前两问的结论和 FHT 直接得到结论。

## F. 第一同构定理

设 \\(G\\) 是群，\\(H\triangleleft G\\), \\(K\le G\\)。证明下列命题：

__F.1__ \\((H\cap K) \triangleleft K\\)

**证明** 不难证明 \\(H\cap K\\) 是 \\(K\\) 的子群。下面证明 \\(H\cap K\\) 是正规子群。

设 \\(a\in(H\cap K)\\)，则 \\(a\in H\\) 且 \\(a\in K\\)。对任意 \\(x\in K\le G\\)，根据群的封闭性知 \\(xax^{-1} \in K\\)。又因为 \\(H\triangleleft G\\)，所以 \\(xax^{-1} \in H\\)。因此 \\((H\cap K) \triangleleft K\\)。

---

__F.2__ 如果 \\(HK = \\{xy : x\in H \wedge y\in K\\}\\)，那么 \\(HK\le G\\)。

**证明** 显然 \\(e\in HK\\)。

设 \\(a,b\in HK\\)，那么存在 \\(x_1,x_2\in H\\) 和 \\(y_1y_2\in K\\) 使得 \\(a=x_1y_1\\), \\(b=x_2y_2\\)。那么 \\(ab = x_1y_1x_2y_2\\)。
因为 \\(H\\) 是正规子群，有 \\(x_2' = y_1x_2y_1^{-1} \in H\\)。于是 \\(ab = x_1x_2'y_1y_2\\)。而 \\(x_1x_2' \in H\\), \\(y_1y_2\in K\\)，所以 \\(ab\in HK\\)。

设 \\(a\in HK\\)，那么存在 \\(x\in H\\), \\(y\in K\\) 使得 \\(a=xy\\)。那么 \\(a^{-1} = (xy)^{-1} = y^{-1}x^{-1}\\)。因为 \\(H\\) 是正规子群，有 \\(x' = y^{-1}x^{-1}(y^{-1})^{-1} \in H\\)。于是 \\(a^{-1} = x'y^{-1}\\)。而 \\(x'\in H\\), \\(y^{-1}\in K\\)，所以 \\(a^{-1}\in HK\\)。

综上所述，\\(HK\le G\\)。

---

__F.3__ \\(H\triangleleft HK\\)

**证明** 显然 \\(H\subseteq HK\\)，又已知 \\(H\\) 是群，所以 \\(H\le HK\\)。另一方面，对于任意 \\(a\in H\\) 和 \\(x\in HK\subseteq G\\)，都有 \\(xax^{-1} \in H\\)。这表明 \\(H\\) 是正规子群。

---

__F.4__ 商群 \\(HK/H\\) 中的每个元素都可以写成 \\(Hk\\) 的形式，其中 \\(k\in K\\)。

**证明** 考虑 \\(HK/H\\) 中的元素 \\(Ha\\)，其中 \\(a=hk\\), \\(h\in H\\), \\(k\in K\\)。因为 \\(ak^{-1} = h\in H\\)，根据第十五章定理 5，有 \\(Ha = Hk\\)。

---

__F.5__ 函数 \\(f(k) = Hk\\) 是从 \\(K\\) 到 \\(HK/H\\) 的满同态，并且它的核为 \\(H\cap K\\)。

**证明** 显然 \\(K\subseteq HK\\)。设 \\(x\in K\subseteq HK\\)，则 \\(Hk\in (HK/H)\\)。所以 \\(f\\) 是 \\(K\to (HK/H)\\) 的映射。

考虑任意 \\(Ha\in (HK/H)\\)，由前一问结论知存在 \\(k\in K\\) 使得 \\(Ha = Hk\\)，于是 \\(f(k) = Ha\\)。这表明 \\(f\\) 是满射。

设 \\(x,y \in K\\)，则 \\(f(xy) = H(xy) = Hx\cdot Hy = f(x)\cdot f(y)\\)。这表明 \\(f\\) 是同态。

为了求 \\(f\\) 的核，设 \\(k\in K\\) 并令 \\(f(k) = H\\)，即 \\(Hk = H\\)。根据第十五章定理 5，当且仅当 \\(k\in H\\) 时该等式成立。又 \\(k\in K\\)，所以解集是 \\(H\cap K\\)。

综上所述，\\(f\\) 是 \\(K\to (HK/H)\\) 的满同态，且核为 \\(H\cap K\\)。

---

__F.6__ 根据 FHT 说明 \\(K/(H\cap K) \cong HK/H\\)。（这被称作*第一同构定理*。）

**答** 根据前一问结论和 FHT 直接得出结论。

## G. 更强的 Cayley 定理

设 \\(H\\) 是 \\(G\\) 的子群。设 \\(X\\) 表示 G 中所有 H 的左陪集。对于任意 \\(a\in G\\)，定义 \\(\rho_a:X\to X\\) 为：\\[\rho_a(xH) = (ax)H\\]

__G.1__ 证明每个 \\(\rho_a\\) 是 \\(X\\) 的一个置换。

**引理** \\(aH = bH \iff a^{-1}b \in H\\)

类似于第十五章定理 5，只不过这里是左陪集，逆元的位置也不一样。证明略。

**证明** 只须证明 \\(\rho_a\\) 是双射即可。

（单射）设 \\(x,y\in G\\) 且 \\((ax)H = (ay)H\\)。根据引理得 \\((ax)^{-1}(ay) \in H\\)，即 \\(x^{-1}y \in H\\)。于是根据引理得 \\(xH = yH\\)。这说明 \\((ax)H = (ay)H \Rightarrow xH=yH\\)。因此 \\(\rho_a\\) 是单射。

（满射）考虑 \\(X\\) 中任意元素 \\(xH\\)，其中 \\(x\in G\\)。令 \\(y = a^{-1}x \in G\\)，则 \\(f(yH) = (ay)H = xH\\)。因此 \\(\rho_a\\) 是满射。

---

__G.2__ 证明 \\(h: G\to S_X\\) 定义为 \\(h(a) = \rho_a\\) 是同态。

**证明** 考虑 \\(X\\) 中的任意元素 \\(xH\\)，其中 \\(x\in G\\)。则
\\[
(\rho_a \circ \rho_b)(xH) = \rho_a(\rho_b(xH))
= (a(bx))H = ((ab)x)H = \rho_{ab}(xH)
\\]
恒成立。所以 \\(\rho_a \circ \rho_b = \rho_{ab}\\)。

这意味着 \\(h(ab) = h(a)h(b)\\)，因此 \\(h\\) 是同态。

---

__G.3__ 证明集合 \\(\\{a\in H : xax^{-1} \in H\\;\forall x\in G\\}\\) （即 \\(H\\) 中所有共轭都在 \\(H\\) 内的元素构成的集合），是 \\(h\\) 的核。

**证明** 值域中的单位元是恒等变换 \\(\epsilon = \rho_e\\)，其中 \\(e\\) 是 \\(G\\) 的单位元。设 \\(a\in G\\)，令 \\(h(a) = \epsilon\\)，得方程 \\(\rho_a = \rho_e\\)。该方程成立的条件是 \\((ax)H = xH\\) 对所有 \\(x\in G\\) 成立。

根据 G.1 的引理可知等式成立的充分必要条件是 \\((ax)^{-1}x = x^{-1}ax \in H\\)。因此解集是 \\(\\{a\in H : xax^{-1} \in H\\;\forall x\in G\\}\\)，这也就是 \\(h\\) 的核。

---

__G.4__ 证明如果 \\(H\\) 不包含 \\(G\\) 的除 \\(\\{e\\}\\) 外的正规子群，则 \\(G\\) 和 \\(S_X\\) 的某个子群同构。

**证明** 前一问证明了
\\[\DeclareMathOperator{\ker}{ker}
\ker h = \\{a\in H : xax^{-1} \in H\\;\forall x\in G\\}\\]
注意到核总是正规子群，所以如果 \\(H\\) 不包含任何除 \\(\\{e\\}\\) 外的任何正规子群，则只能是 \\(\ker h = \\{e\\}\\)。

根据 FHT，有 \\(G/\\{e\\} \cong \mathop{\rm ran} h \subseteq S_X\\)。

另一方面，\\(G/\\{e\\} = \\{\\{x\\} : x\in G\\}\\) 显然和 \\(G\\) 同构：\\(x \leftrightarrow \\{x\\}\\)。

综上所述，\\(G\\) 同构于 \\(S_X\\) 的某个子集。

## H. 同构于圆群的商群

__H.1__ 对任意 \\(x\in\mathbb R\\)，约定 \\(\DeclareMathOperator{\cis}{cis}\cis x = \cos x + i\sin x\\)。证明 \\(\cis (x+y) = (\cis x)(\cis y)\\)。

**证明** 使用欧拉公式或者使用复数的运算法则结合三角恒等式（略）。

---

__H.2__ 设 \\(\mathbb T\\) 表示集合 \\(\\{\cis x: x\in R\\}\\)（即所有位于单位元上的复数）和复数乘法构成的群。证明 \\(\mathbb T\\) 是群。（称为*圆群*）。

**证明**

1. 单位元是 \\(\cis 0 = 1\\)。
2. \\(\cis x\\) 的逆元是 \\(\cis (-x)\\)。
3. 结合律：\begin{align\*}(\cis x \cis y) \cis z &= \cis (x+y) \cis z \\\\ &= \cis(x+y+z) \\\\ &= \cis x \cis (y+z) \\\\ &= \cis x (\cis y \cis z)\end{align\*}

---

__H.3__ 证明 \\(f(x) = \cis x\\) 是 \\(\mathbb R \to \mathbb T\\) 的满同态。

**证明** 显然 \\(f\\) 是 \\(\mathbb R \to \mathbb T\\) 的满射。

设 \\(x,y\in R\\)，则由 H.1 知 \\(\cis(x+y) = (\cis x)(\cis y)\\)。所以 \\(f\\) 是满同态。

---

__H.4__ 证明 \\(\ker f = \\{2n\pi : n\in\mathbb Z\\} = \langle 2\pi \rangle\\)。

**证明** 设 \\(x\in \mathbb R\\)，令 \\(f(x) = 1\\) 得
\\[\cos x + i\sin x = 1 + 0i\\]
根据复数相等的规则，方程等价于方程组
\begin{gather\*}
\cos x = 1 \\\\
\sin x = 0
\end{gather\*}
根据三角函数知识，可知解集为 \\(\\{2n\pi  : n\in \mathbb Z\\}\\)，这也就是 \\(f\\) 的核。

---

__H.5__ 根据 FHT 说明 \\(\mathbb T \cong \mathbb R / \langle 2\pi \rangle\\)。

**答** 根据 H.3、H.4 的结论和 FHT 立即得到结论。

---

__H.6__ 证明 \\(f(x) = \cis 2\pi x\\) 是 \\(\mathbb R \to \mathbb T\\) 的满同态，且核为 \\(\mathbb Z\\)。

**证明** 参考 H.3、H.4 证明。

---

__H.7__ 说明 \\(\mathbb T \cong \mathbb R / \mathbb Z\\)。

**答** 根据 H.6 和 FHT 立即得到结论。

## I. 第二同构定理

设 \\(H\\) 和 \\(K\\) 是群 \\(G\\) 的正规子群，并且 \\(H\subseteq K\\)。
定义 \\(\phi: G/H \to G/K\\) 为 \\(\phi(Ha) = Ka\\)。

证明 1--4。

__I.1__ \\(\phi\\) 是良定义的函数。[即如果 \\(Ha = Hb\\)，则 \\(\phi(Ha)=\phi(Hb)\\)。]

**证明** 设 \\(Ha=Hb\\)，则 \\(ab^{-1} \in H \subseteq K\\)。于是 \\(Ka = Kb\\) 即 \\(\phi(Ha) = \phi(Hb)\\)。

---

__I.2__ \\(\phi\\) 是同态。

**证明** 考虑 \\(G/H\\) 中的任意元素 \\(Hx,Hy\\)，其中 \\(x,y\in G\\)。
\\[\phi(Hx\cdot Hy) = \phi(H(xy)) = K(xy) = Kx \cdot Ky = \phi(x)\cdot\phi(y)\\]
所以 \\(\phi\\) 是同态。

---

__I.3__ \\(\phi\\) 是满射。

**证明** 考虑 \\(G/K\\) 中的任意元素 \\(Ka\\)，其中 \\(a\in G\\)。因为有 \\(Ha \in G/H\\) 且 \\(\phi(Ha) = Ka\\)，所以 \\(\phi\\) 是满射。

---

__I.4__ \\(\ker \phi = K/H\\)

**证明** 值域中的单位元是 \\(K\\)。设 \\(Hx \in G/H\\)，其中 \\(x\in G\\)，列方程 \\(\phi(Hx) = K\\)，即
\\(Kx = K\\)。等式成立的充要条件是 \\(x \in K\\)，所以方程（关于未知数 \\(Hx\\)）的解集是 \\(\\{Hx : x \in K\\}\\)，这也就是 \\(\phi\\) 的核。
另一方面，根据商群的定义有 \\(K/H = \\{Hx : x\in K\\}\\)。所以 \\(\ker \phi = K/H\\)。

---

__I.5__ （使用 FHT）说明 \\((G/H) / (K/H) \cong G/K\\)。

**答** 根据 I.2--I.4 和 FHT 立即得到结论。

## J. 对应定理 (Correspondence Theorem)

设 \\(f\\) 为 \\(G\\) 到 \\(H\\) 上的满同态，且核为 \\(K\\)：
\\[f:G \xrightarrow[\mkern-8mu ^K\mkern40mu]{}\mkern-12mu\to H\\]
如果 \\(S\\) 是 \\(H\\) 的子群，设 \\(S^\* = \\{x\in G : f(x) \in S\\}\\)。求证：

__J.1__ \\(S^\*\\) 是 \\(G\\) 的子群。

**证明** 显然 \\(e \in S^\* \subseteq G\\)。

设 \\(x,y\in S^\*\\)，则有 \\(f(x),f(y)\in S\\)。因为 \\(f\\) 是同态而且 \\(S\\) 是群，所以
\\(f(xy) = f(x)f(y) \in S\\)。于是 \\(xy\in S^\*\\)。

设 \\(x\in S^\*\\)。则 \\(f(x^{-1}) = [f(x)]^{-1} \in S\\)，所以 \\(x^{-1}\in S^\*\\)。

综上所述， \\(S^\*\\) 是 \\(G\\) 的非空子集，且对乘法和逆元均封闭，所以是 \\(G\\) 的子群。

---

__J.2__ \\(K\subseteq S^\*\\)

**证明** \\(x\in K \Longrightarrow f(x) = e \in S \Longrightarrow x\in S^\*\\)

---

__J.3__ 设 \\(g\\) 是 \\(f\\) 把定义域限定在 \\(S^\*\\) 上的函数，则 \\(g\\) 是 \\(S^\*\\) 到 \\(S\\) 的满同态，并且
\\(K = \ker g\\)。

**证明** 根据定义易知 \\(g\\) 是满射。根据 \\(f\\) 的同态性质可知 \\(g\\) 是同态。

设 \\(x\in S^\*\\)，列方程 \\(g(x) = e\\)。注意到 \\(f(x) = e\\) 的解集为 \\(K\\)，又由前一问知 \\(K\subseteq S^\*\\)，所以 \\(\ker g = K\\)。

---

__J.4__ \\(S \cong S^\* / K\\)。

**证明** 根据 J.3 的结论和 FHT 立即得到。
