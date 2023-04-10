---
title: 抽象代数习题(32) -- Galois 理论II
date: 2022-02-18T20:36:51-08:00
---

《抽象代数》第三十二章讲的是 Galois 理论的核心内容。设 K 是 F 的分裂域，**Galois 群**是所有 K→K 且固定 F 的同构构成的群（群的运算为映射的复合），记作 Gal(K:F)。Galois 理论的关键内容是，K 和 F 的所有中间域 I (即 K&sube;I&sube;F) 和 Gal(K:F) 中的子群有着一一对应的关系。

<!--more-->

## A. 计算 Galois 群

__A.2__ 求 \\(\newcommand\Q{\mathbb Q}\Q(i,\sqrt2)\\) 在 \\(\Q\\) 上的扩张次数。

**解** 因为 \\(\Q(\sqrt2)\cong\Q/\langle x^2-2\rangle\\)，所以 \\([\Q(\sqrt2):\Q] = 2\\)。多项式 \\(x^2+1\\) 的根 \\(\pm i\notin \Q(\sqrt2)\\)，因此在 \\(\Q(\sqrt2)\\) 上不可约，从而是 \\(i\\) 的最小多项式。于是 \\([\Q(i,\sqrt2) : \Q(\sqrt2)] = 2\\)。于是
\\[[\Q(i,\sqrt2) : \Q] = [\Q(i,\sqrt2) : \Q(\sqrt2)]\[\Q(\sqrt2) : \Q] = 4\\]

---

__A.3__ 列出 \\(\DeclareMathOperator\Gal{Gal}\Gal(K:F)\\) 的元素及其运算表。

**解** 根据 A.2 和定理 1 可知 \\(\Gal(K:F)\\) 共有 4 个元素。事实上，四个元素如下：
\begin{gather\*}
\epsilon = {i\to i\atopwithdelims\lbrace\rbrace\sqrt2\to\sqrt2},\quad
\alpha = {i\to -i\atopwithdelims\lbrace\rbrace\sqrt2\to\sqrt2},\\\\
\beta = {i\to i\atopwithdelims\lbrace\rbrace\sqrt2\to-\sqrt2},\quad
\gamma = {i\to -i\atopwithdelims\lbrace\rbrace\sqrt2\to-\sqrt2}.\\\\
\end{gather\*}
它们的运算表为
\begin{array}{c|cccc}
\circ & \epsilon & \alpha & \beta & \gamma \\\\\hline
\epsilon & \epsilon & \alpha & \beta & \gamma \\\\
\alpha & \alpha & \epsilon & \gamma & \beta \\\\
\beta & \beta & \gamma & \epsilon & \alpha \\\\
\gamma & \gamma & \beta & \alpha & \epsilon \\\\
\end{array}

---

__A.4__ 写出 \\(\Gal(\Q(i,\sqrt2):\Q)\\) 的子群的包含图，以及 \\(\Q\\) 和 \\(\Q(i,\sqrt2)\\) 的中间域的包含图。指出 Galois 对应。

**解** 如下图所示。图中实线箭头表示包含关系，虚线箭头表示 Galois 对应
{{<figure src="/post/2022/02/cd.svg" width="640" >}}

```latex
\documentclass[crop,tikz]{standalone}
\usepackage{amsfonts}
\usepackage{tikz-cd}
\begin{document}
\newcommand\Q{\mathbb Q}
\begin{tikzcd}[row sep=1cm]
                   & \Q(i,\sqrt2) \ar[ld] \ar[d] \ar[rd] & & &                             & \{\epsilon,\alpha,\beta,\gamma\} \ar[ld] \ar[d] \ar[rd] \\
\Q(\sqrt2) \ar[rd] & \Q(i) \ar[d] & \Q(i\sqrt2) \ar[ld]    & & \{\epsilon,\gamma\} \ar[rd] & \{\epsilon,\beta\} \ar[d]        & \{\epsilon,\alpha\} \ar[ld] \\
                   & \Q           & & &                                                    & \{\epsilon\}
\ar[dashed, from=1-2, to=3-6]
\ar[dashed, from=3-2, to=1-6]
\ar[dashed, from=2-3, to=2-5]
\ar[dashed, bend right=8, from=2-1, to=2-7]
\ar[dashed, bend right=6, from=2-2, to=2-6]
\end{tikzcd}
\end{document}
```

## C. 等于 S₃ 的 Galois 群

__C.1__ 证明 \\(\Q(\sqrt[3]2,i\sqrt3)\\) 是 \\(x^3-2\\) 在 \\(\Q\\) 上的分裂域，其中 \\(\sqrt[3]2\\) 表示 \\(2\\) 的*实*立方根。

**证明** \\(x^3-2\\) 的根为 \\(\sqrt[3]2,\sqrt[3]2\bigl(\frac12\pm\frac{\sqrt3}{2}i\bigr)\\)。所以分裂域为 \\(\Q(\sqrt[3]2,i\sqrt3)\\)。

---

__C.2__ 证明 \\([\Q(\sqrt[3]2):\Q]=3\\)。

**证明** 因为 \\(\sqrt[3]2\\) 在 \\(\Q\\) 上的最小多项式为 \\(x^3-2\\)，所以是三次扩张。

---

__C.3__ 说明为什么 \\(x^2+3\\) 在 \\(\Q(\sqrt[3]2)\\) 上不可约，然后说明 \\([\Q(\sqrt[3]2,i\sqrt3):\Q(\sqrt[3]2)]=2\\)。做出结论 \\([\Q(\sqrt[3]2,i\sqrt3):\Q]=6\\)。

**证明** 因为 \\(x^2+3\\) 的根为 \\(\pm i\sqrt3\\)。假设 \\(\pm i\sqrt3\in \Q(\sqrt[3]2)\\)，那么
\\[\pm i\sqrt3 = a+b2^{1/3}+c2^{2/3}\qquad(a,b,c\in\Q)\\]
两边平方得
\\[-3 = a^2+bc + (2ab+3c^2)2^{1/3} + (2ac+b^2)2^{2/3}\\]
比较系数得
\begin{align\*}
a^2+bc &= -3\tag1\\\\
2ab+3c^2 &= 0\tag2\\\\
2ac+b^2 &= 0\tag3
\end{align\*}
由 \\((2)(3)\\) 解得 \\(2abc=-3c^3=-b^3\\)，因此 \\(\frac{b}{c} = \sqrt[3]3\\)。这是不可能的，因为等式左边是有理数，右边是无理数。因此假设不成立，\\(\pm i\sqrt3\notin \Q(\sqrt[3]2)\\)。所以 \\([\Q(\sqrt[3]2,i\sqrt3):\Q(\sqrt[3]2)]=2\\)。

---

__C.4__ 使用 C.3 说明为什么 \\(\mathbf G = \Gal(\Q(\sqrt[3]2,i\sqrt3):\Q)\\) 有 6 个元素。然后使用 323 页的规则 (ii) 说明为什么 \\(\mathbf G\\) 的每个元素都可以用 2 的三个立方根的置换来表示。

**证明** 根据本章定理 1 以及 C.3 可知 \\(|\mathbf G| = [\Q(\sqrt[3]2,i\sqrt3):\Q] = 6\\)。因为分裂域的自同构总是把多项式的根映射为根，所以 \\(\mathbf G\\) 中的每个元素可以表示为多项式 \\(x^3-2\\) 的三个根的置换。

---

__C.5__ 使用 C.4 证明 \\(\mathbf G \cong S_3\\)。

**证明** 根据 C.4 可知 \\(\mathbf G\\) 有 6 个元素，每个元素都是 2 的三个立方根的置换。而 3 个元素的置换总共只有 6 种（\\(|S_3|=6\\)），所以 \\(\mathbf G\\) 包含了所有的置换，因此 \\(\mathbf G \cong S_3\\)。

## D. 等于 D₄ 的 Galois 群

设 \\(\alpha=\sqrt[4]2\\) 是 2 的一个实四次方根，那么 2 的四个四次方根分别为 \\(\pm\alpha\\) 和 \\(\pm i\alpha\\)。简要但仔细地解释 1--6：

__D.1__ \\(\Q(\alpha,i)\\) 是 \\(x^4-2\\) 在 \\(\Q\\) 上的分裂域。

**证明** 分裂域必须包括 \\(\alpha\\) 和 \\(i\\)，因此不可能比 \\(\Q(\alpha,i)\\) 更小。根据域对四则运算的封闭性可知 \\(\Q(\alpha,i)\\) 包含了 \\(x^4-2\\) 所有的根。

---

__D.2__ \\([\Q(\alpha,i):\Q]=4\\)。

**证明** 因为 \\(\alpha\\) 在 \\(\Q\\) 上的多项式是 \\(x^4-2\\)，次数是 4。

---

__D.3__ \\(i\notin \Q(\alpha)\\)；因此 \\([\Q(\alpha,i):\Q(\alpha)]=2\\)。

**证明** 因为 \\(i\\) 不是实数，而 \\(\Q(\alpha)\\) 中的元素都是实数。所以 \\(i\notin\Q(\alpha)\\)。
最小多项式为 \\(x^2+1\\)，因此 \\([\Q(\alpha,i):\Q(\alpha)]=2\\)。

---

__D.4__ \\([\Q(\alpha,i):\Q]=8\\)

**证明** \\[[\Q(\alpha,i):\Q]=[\Q(\alpha,i):\Q(\alpha)]\[\Q(\alpha,i):\Q] = 8\\]

---

__D.5__ \\(\\{1,\alpha,\alpha^2,\alpha^3,i,i\alpha,i\alpha^2,i\alpha^3\\}\\) 是 \\(\Q(\alpha,i)\\) 在 \\(\Q\\) 上的一组基。

**证明** 因为 \\(\\{1,\alpha,\alpha^2,\alpha^3\\}\\) 是 \\(\Q(\alpha)\\) 在 \\(\Q\\) 上的一组基，并且 \\(\\{1,i\\}\\) 是 \\(\Q(\alpha,i)\\) 在 \\(\Q(\alpha)\\) 上的一组基。根据第二十九章定理 2 前的引理可得结论。

---

__D.6__ \\(\Q(\alpha,i)\\) 的任意固定 \\(\Q\\) 的自同构 \\(h\\) 由它对这组基中的元素的作用所完全确定。而它们进一步又由 \\(h(\alpha)\\) 和 \\(h(i)\\) 完全确定。

**证明** 根据 D.5 可知任意 \\(x\in \Q(\alpha,i)\\) 都可以表示为
\\[x = \sum_{k=1}^8 a_k\boldsymbol e_k\\]
其中 \\(a_k\in\Q\\)，\\(\boldsymbol e_k\\) 是 D.5 中的某个基元素。于是
\\[h(x) = \sum_{k=1}^8 a_kh(\boldsymbol e_k)\\]
这是因为 \\(h\\) 固定 \\(\Q\\)。这表明 \\(h\\) 完全由 \\(h(\boldsymbol e_k)\\) 确定。

根据 D.5 可知，\\(\boldsymbol e_k = \alpha^{k_1}i^{k_2}\\)，其中 \\(k_1=0,1,2,3\\)，\\(k_2=0,1\\)。于是
\\[h(\boldsymbol e_k) = h(\alpha)^{k_1}h(i)^{k_2}\\]
这表明 \\(h(\boldsymbol e_k)\\) 完全由 \\(h(\alpha)\\) 和 \\(h(i)\\) 确定。

---

__D.7__ 说明：\\(h(\alpha)\\) 必须是 2 的四次方根，且 \\(h(i)\\) 必须是 \\(\pm i\\)。结合 \\(h(\alpha)\\) 的四种可能性和 \\(h(i)\\) 的两种可能性，得到了八个可能的自同构。以如下形式将它们列出：
\\[\newcommand\aut[2]{\displaystyle{\alpha\to#1\alpha \atopwithdelims\\{\\} i\to #2i}}\aut{}{},\quad \aut{-}{},\ldots\\]

**证明** 因为 \\(\alpha^4=2\\)，两边应用 \\(h\\) 得 \\(h(\alpha)^4 = 2\\)。因此 \\(h(\alpha)\\) 是 2 的四次方根。同理可得 \\(h(i)^2 = -1\\)，所以 \\(h(i)=\pm i\\)。

八个可能的自同构如下：
\begin{array}{cccc}
\aut{}{},&\aut{i}{},&\aut{-}{},&\aut{-i}{}\\\\
\aut{}{-},&\aut{i}{-},&\aut{-}{-},&\aut{-i}{-}\\\\
\end{array}

---

__D.8__ 计算 \\(\Gal(\Q(\alpha,i):\Q)\\) 的运算表，并证明它同构于 \\(D_4\\)，即正方形的对称变换群。

**解**

设自同构
\\[h_{jk} = \aut{i^j}{(-1)^k}\\]
不难得到
\\[h_{jk}\circ h_{j'k'} = \aut{i^{j+j'}}{(-1)^{k+k'}}\\]

那么运算表为
\begin{array}{c|ccccccc}
\circ &h_{00}&h_{10}&h_{20}&h_{30}&h_{01}&h_{11}&h_{21}&h_{31}\\\\\hline
h_{00}&h_{00}&h_{10}&h_{20}&h_{30}&h_{01}&h_{11}&h_{21}&h_{31}\\\\
h_{10}&h_{10}&h_{20}&h_{30}&h_{00}&h_{11}&h_{21}&h_{31}&h_{01}\\\\
h_{20}&h_{20}&h_{30}&h_{00}&h_{10}&h_{21}&h_{31}&h_{01}&h_{11}\\\\
h_{30}&h_{30}&h_{00}&h_{10}&h_{20}&h_{31}&h_{01}&h_{11}&h_{21}\\\\
h_{01}&h_{01}&h_{11}&h_{21}&h_{31}&h_{00}&h_{10}&h_{20}&h_{30}\\\\
h_{11}&h_{11}&h_{21}&h_{31}&h_{01}&h_{10}&h_{20}&h_{30}&h_{00}\\\\
h_{21}&h_{21}&h_{31}&h_{01}&h_{11}&h_{20}&h_{30}&h_{00}&h_{10}\\\\
h_{31}&h_{31}&h_{01}&h_{11}&h_{21}&h_{30}&h_{00}&h_{10}&h_{20}\\\\
\end{array}

根据运算表知它同构于 \\(D_4\\)。

## E. 循环 Galois 群

__E.1__ 描述 \\(x^7-1\\) 在 \\(\Q\\) 上的分裂域 \\(K\\)。说明为什么 \\([K:\Q]=6\\)。

**答** \\(K=\Q(\alpha)\\)，其中 \\(\\alpha=\cos(2\pi/7)+i\sin(2\pi/7)\\) 是本原七次单位根。
因为 \\(\\alpha\\) 是不可约多项式 \\(x^6+x^5+\cdots+x+1 = \frac{x^7-1}{x-1}\\) 的根，所以 \\([K:\Q]=6\\)。

---

__E.2__ 说明：如果 \\(\alpha\\) 是七次本原单位根，那么任意 \\(h\in\Gal(K:\Q)\\) 一定将 \\(\alpha\\) 映射为七次单位根。事实上，\\(h\\) 由 \\(h(\alpha)\\) 确定。

**证明** \\(h\colon K\to K\\) 是固定 \\(\Q\\) 的自同构。因为 \\(\alpha^7=1\\)，两边应用 \\(h\\) 得 \\(h(\alpha)^7=1\\)。可见 \\(h(\alpha)\\) 是七次单位根。因为任意 \\(x\in K=\Q(\alpha)\\) 都可以表示为 \\(x=\sum_{i=0}^n k_i\alpha^i\\)，其中 \\(k_i\in\Q\\)，所以 \\(h(x)=\sum_{i=0}^n k_i h(\alpha)^i\\)。可见 \\(h\\) 完全由 \\(h(\alpha)\\) 确定。

---

__E.3__ 使用 E.2 的结论，显式地列出 \\(\Gal(K:\Q)\\) 的所有元素。然后写出运算表，并证明它是循环群。

**答** 所有元素分别由
\\[h_i(\alpha) = \alpha^i \qquad(1\le i \le 6)\\]
确定。（因为 \\(h(1)=1\\)，\\(h\\) 是同构，所以 \\(h(\alpha)\ne1\\)。）

不难得到 \\(h_i\circ h_j = h_{ij\bmod 7}\\)。运算表如下：
\begin{array}{c|ccccc}
\circ &h_1&h_2&h_3&h_4&h_5&h_6\\\\\hline
h_1   &h_1&h_2&h_3&h_4&h_5&h_6\\\\
h_2   &h_2&h_4&h_6&h_1&h_3&h_5\\\\
h_3   &h_3&h_6&h_2&h_5&h_1&h_4\\\\
h_4   &h_4&h_1&h_5&h_2&h_6&h_3\\\\
h_5   &h_5&h_3&h_1&h_6&h_4&h_2\\\\
h_6   &h_6&h_5&h_4&h_4&h_2&h_1\\\\
\end{array}
可见它同构于循环群 \\(\mathbb Z_7^\ast\\)。

---

__E.4__ 列出 \\(\Gal(K:\Q)\\) 的所有子群以及对应的固定域。指出 Galois 对应。

**答** 根据 E.3 知 \\(h_{2,4}\\) 轮换 \\((\alpha,\alpha^2,\alpha^4)\\) 和 \\((\alpha^3,\alpha^5,\alpha^6)\\)；\\(h_6\\) 对换 \\((\alpha,\alpha^6)\\), \\((\alpha^2,\alpha^5)\\) 和 \\((\alpha^3,\alpha^4)\\)。所以设
\begin{align\*}
K_1&=\bigl\\{a\bigl(\alpha+\alpha^2+\alpha^4\bigr)+b\bigl(\alpha^3+\alpha^5+\alpha^6\bigr)\bigr\\},\\\\
K_2&=\bigl\\{c\bigl(\alpha+\alpha^6\bigr)+d\bigl(\alpha^2+\alpha^5\bigr)+e\bigl(\alpha^3+\alpha^4\bigr)\bigr\\},
\end{align\*}
其中 \\(a,b,c,d,e\in\Q\\)。易知 \\(K_1,K_2\subseteq K\\)。

那么 \\(\Gal(K:\Q)\\) 的子群如下：

* \\(\Gal(K:\Q)=\\{h_1,h_2,h_3,h_4,h_5,h_6\\}\\)，固定域为 \\(\Q\\)。
* \\(\\{h_1,h_2,h_4\\}\\)，固定域为 \\(K_1\\)。
* \\(\\{h_1,h_6\\}\\)，固定域为 \\(K_2\\)。
* \\(\\{h_1\\}\\)，固定域为 \\(K=\Q(\alpha)\\)。

---

__E.5__ 描述 \\(x^6-1\\) 在 \\(\Q\\) 上的分裂域 \\(L\\)，并证明 \\([L:\Q]=2\\)。说明为什么 \\(\Q\\) 和 \\(L\\) 之间没有中间域（不含 \\(\Q\\) 和 \\(L\\) 本身）。

**证明** 因为 \\(x^6-1\\) 的根为 \\(\zeta^i\\)，其中 \\(\zeta=\cos\frac{2\pi}{6}+i\sin\frac{2\pi}{6} = \frac{1}{2}+\frac{\sqrt3}{2}i\\) 且 \\(i=0,1,2,3,4,5\\)，所以分裂域为 \\(L=\Q(\zeta) = \Q(\frac{1}{2}+\frac{\sqrt3}{2}i) = \Q(i\sqrt3)\\)。

因为 \\(i\sqrt3\\) 在 \\(\Q\\) 上的最小多项式为 \\(x^2+3\\)，所以 \\([L:\Q]=2\\)。

设 \\(\Q\subseteq I \subseteq L\\)，那么 \\([L:\Q] = [L:I]\[I:\Q] = 2\\)。于是或者 \\([L:I]=1\\) 或者 \\([I:\Q]=1\\)。这表明或者 \\(L=I\\) 或者 \\(I=\Q\\)。因此不存在严格介于两者之间的域。（同第二十九章习题 D.2。）

---

__E.6__ 设 \\(L\\) 为 \\(x^6-2\\) 在 \\(\Q\\) 上的分裂域。列出 \\(\Gal(L:\Q)\\) 的所有元素并写出运算表。

**证明** \\(x^6-2\\) 的所有根为 \\(\alpha\zeta^i\\)，其中 \\(\alpha=\sqrt[6]2\\) 是 \\(2\\) 的实六次方根，\\(\zeta^i\\) 与 E.5 中相同。因此 \\(L=\Q(\alpha,\zeta)=\Q(\sqrt[6]2,i\sqrt3)\\)。

因为 \\(L\\) 是分裂域，它的自同构 \\(h\colon L\to L\\) 把 \\(x^6-2\\) 的根映射为 \\(x^6-2\\) 的根。考虑 \\(h(\alpha)\\)，它有 6 种可能性： \\(h(\alpha)=\alpha\zeta^i\\)，其中 \\(i=0,1,2,3,4,5\\)。

同理，考虑多项式 \\(x^2+3\\) 得 \\(h(i\sqrt3)=\pm i\sqrt3\\)。一旦确定了 \\(h(i\sqrt3)\\)，那么 \\(h(\zeta)\\) 随之确定，于是 \\(x^6-2\\) 的所有根的映射就都确定了。

综上所述，\\(\Gal(L:\Q)\\) 中的所有元素为
\\[h_{ij} = \begin{Bmatrix}\alpha\to\alpha\zeta^i\\\\i\sqrt3\to(-1)^j i\sqrt3\end{Bmatrix}\qquad(i=0,1,2,3,4,5;j=0,1)\\]

（运算表略。）

**注** 这题很容易算错。按照类似的推理，\\(x^n-a\\) 的根如果都不在 \\(\Q\\) 中，那么它的分裂域的 Galois 群将同构于 \\(D_n\\)。这是一个很有趣的现象。

## F. 同构于 S₅ 的 Galois 群

设 \\(a(x)=x^5-4x^4+2x+2\in\Q[x]\\)，且 \\(r_1,\ldots,r_5\\) 是 \\(a(x)\\) 在 \\(\mathbb C\\) 中的根。设 \\(K=\Q(r_1,\ldots,r_5)\\) 为 \\(a(x)\\) 在 \\(\Q\\) 上的分裂域。

证明 1--3。

__F.1__ \\(a(x)\\) 在 \\(\Q[x]\\) 中不可约。

**证明** 使用 Eisenstein 判据，取 \\(p=2\\)。

---

__F.2__ \\(a(x)\\) 有三个实根和两个复根。

**证明** 使用微积分的知识：设实变函数 \\(y=a(x)\\)，那么 \\(y'=5x^4-16x^3+2\\)，\\(y''=20x^3-48x^2=4x^2(5x-12)\\)。
可见 \\(y'\\) 在 \\((-\infty,5/12)\\) 上单调递减，在 \\((5/12,+\infty)\\) 上单调递增。因为一个单调区间内至多只有一个零点，所以 \\(y'\\) 最多有 2 个零点。因为 \\(x\to\pm\infty\\) 时 \\(y'\to+\infty\\)，所以如果 \\(y\\) 能取到一个负值，则在两侧分别存在一个零点；的确如此： \\(y'(1)=-9<0\\)。由此可知 \\(y'\\) 恰有 2 个零点。因此，\\(y\\) 先单调递增，再单调递减，再单调递增；因此最多有 3 个零点。而 \\(y(0) = 2>0\\)，\\(y(2)=-26<0\\)，所以 \\(y\\) 确有 3 个零点。这就证明了 \\(a(x)\\) 有三个实根。根据代数基本定理，\\(a(x)\\) 还有两个（非实数的）复根。

**注** 因为多项式次数较高且不可约，手工计算难以得到精确的零点/极值点，所以需要进行适当的估算，比较繁琐；可以使用图形计算器[直接观察函数图象](https://www.wolframalpha.com/input?i=x%5E5-4x%5E4%2B2x%2B2)。

---

__F.3__ 如果 \\(r_1\\) 表示 \\(a(x)\\) 的一个实根，那么 \\([\Q(r_1):\Q]=5\\)。使用这个结论证明 \\([K:\Q]\\) 是 5 的倍数。

**证明** 因为 \\(a(x)\\) 不可约且是首一多项式，所以它是 \\(r_1\\) 的最小多项式。因此 \\([\Q(r_1):\Q]=\deg a(x)=5\\)。而 \\([K:\Q]=[K:\Q(r_1)]\[\Q(r_1):\Q]\\)，所以 \\([K:\Q]\\) 是 \\(5\\) 的倍数。

---

__F.4__ 使用 F.3 和 Cauchy 定理（第十三章习题 E）证明 \\(\Gal(K:\Q)\\) 中存在阶为 \\(5\\) 的元素 \\(\alpha\\)。因为 \\(\alpha\\) 可以表示为 \\(\\{r_1,\ldots,r_5\\}\\) 的置换，说明为什么它一定是长度为 \\(5\\) 的轮换。

**证明** 由 F.3 知 \\(5\mid [K:\Q]\\)。根据本章定理 1 可知 \\(|\Gal(K:\Q)|=[K:\Q]\\)，因此它也是 \\(5\\) 的倍数。根据 Cauchy 定理可知其中存在阶为 \\(5\\) 的元素，设它是 \\(\sigma \in S_5\\)。考虑 \\(S_5\\) 中的任意不是长度为 5 的轮换的元素 \\(\sigma'\\)，根据第八章内容可知它可以分解为长度小于 5 的不相交的轮换的复合。设 \\(\sigma'=\tau_1\cdots\tau_n\\)。那么 \\(\mathord{\rm ord}(\sigma')=\mathord{\rm lcm}(\mathord{\rm ord}(\tau_1),\ldots,\mathord{\rm ord}(\tau_n))\\)。因为小于 5 的正整数的最大公约数不可能等于 5，所以 \\(\sigma'\\) 的阶不可能是 5。这就说明了 \\(\sigma\\) 一定是长度为 5 的轮换。

---

__F.5__ 说明为什么 \\(\Gal(K:\Q)\\) 中存在对换。

**证明** 因为 \\(a(x)\\) 是实系数多项式，因此它的两个复数根互为共轭。容易验证 \\(h(a+b\mkern1mui) = a-b\mkern1mui\\) 是 \\(K\to K\\) 的同构，其中 \\(a,b\\) 分别为实部和虚部。可见，\\(h\\) 对换 \\(a(x)\\) 的两个复数根而固定其余实数根。这表明 \\(\Gal(K:\Q)\\) 中存在对换。

---

__F.6__ 求证：\\(S_5\\) 的任意子群，如果包含了一个长度为 5 的轮换和一个对换，那么它一定包含 \\(S_5\\) 中的所有对换，于是包含 \\(S_5\\) 的全部元素。由此可知，\\(\Gal(K:\Q)=S_5\\)。

**证明** 设轮换为 \\(\sigma=(a_1a_2a_3a_4a_5)\\) 而对换为 \\(\tau=(a_ia_j)\\)，那么
\begin{align\*}
\sigma\tau\sigma^{-1} &= (a_{i+1}a_{j+1})\\\\
\sigma^2\tau\sigma^{-2} &= (a_{i+2}a_{j+2})\\\\
\sigma^3\tau\sigma^{-3} &= (a_{i+3}a_{j+3})\\\\
\sigma^4\tau\sigma^{-4} &= (a_{i+4}a_{j+4})\\\\
\end{align\*}

为了使下标的加法运算总是成立，规定 \\(a_6=a_1\\)，\\(a_7=a_2\\)，依此类推。不失一般性，设 \\(i < j\\)，并设 \\(d=j-i\\)，则 \\(d=1,2,3,4\\)。下面证明任意对换 \\((a_ra_s)\\) 总是可以由上述 5 个对换复合而成。因为 \\(r=1,2,3,4,5\\)，所以上述 5 个对换中恰有一个是 \\((a_ra_{r+d})\\)。同理，恰有一个是 \\((a_{r+d}\\,a_{r+2d})\\)。依此类推，经复合可以得到 \\((a_ra_{r+kd})\\)，其中 \\(k\\) 是任意整数。如果令 \\((a_ra_{r+kd})=(a_ra_s)\\)，则它等价于
\\[r+kd \equiv s\pmod 5\\]
它等价于关于 \\(k,t\\) 的方程
\\[kd+5t = s-r\\]
因为 \\(\gcd(d,5)=1\\)，所以根据 Bézout 定理可知该方程有整数解。这就证明了任意对换都可以由 \\(\sigma\\) 和 \\(\tau\\) 复合构成。

因为任意置换都可以分解为对换的复合，所以 \\(S_n\\) 的所有元素都可以由 \\(\sigma\\) 和 \\(\tau\\) 复合构成。根据群的封闭性，如果 \\(S_n\\) 的子群包含了 \\(\sigma\\) 和 \\(\tau\\)，那么它就包含了 \\(S_n\\) 的所有元素。

## G. 与自同构和 Galois 群相关的简短问题

设 F 为域，K 是 F 的有限扩张。假定 a,b &isin; K。

证明 1--3.

__G.1__ 如果 h 是 K 的自同构，且固定 F 和 a，那么 h 固定 F(a)。

**证明** 根据已知条件，h(a) = a，并且 h(x) = x 对任意 x &isin; F 都成立。因为 K 是有限扩张，所以 a 是 F 上的代数元，于是 F(a) 中的任意数 y 都可以表示为 y = c₀ + c₁a + &ctdot; + c<sub>n</sub>a<sup>n</sup>，其中 c₀,c₁,&hellip;,c<sub>n</sub> &isin; F。根据 h 是同态可知 h(y) = y。因此 h 固定 F(a)。

---

__G.2__ F(a,b)<sup>\*</sup> = F(a)<sup>\*</sup> &cap; F(b)<sup>\*</sup>。

**证明** 设 h &isin; F(a,b)<sup>\*</sup>。显然 h 固定 F。因为 a &isin; F(a,b)，所以 h 固定 a。根据 G.1 可知 h 固定 F(a)。这表明 h &isin; F(a)<sup>\*</sup>。同理可证 h &isin; F(b)<sup>\*</sup>。于是 h &isin; F(a)<sup>\*</sup> &cap; F(b)<sup>\*</sup>

另一方面，设 g &isin; F(a)<sup>\*</sup> &cap; F(b)<sup>\*</sup>。根据定义，g 固定 F、a、b。而 F(a,b) 中的每个元素都可以表示为形如 ka<sup>i</sup>b<sup>j</sup> 的和的形式。因此应用 g 后保持不变。这表明 g &isin; F(a,b)<sup>\*</sup>。

综上所述，F(a,b)<sup>\*</sup> = F(a)<sup>\*</sup> &cap; F(b)<sup>\*</sup>。

---

__G.3__ 除了恒等映射外，不存在 \\(\Q(\sqrt[3]2)\\) 的自同构固定 \\(\Q\\)。

**证明** 记 \\(\alpha=\sqrt[3]2\\)，那么 \\(\alpha^3=2\\)。设 \\(h\\) 是 \\(\Q(\alpha)\\) 的自同构且固定 \\(Q\\)，那么
\\[h(\alpha)^3=h(2)=2\\]
设 \\(\beta=h(\alpha) \in\Q(\alpha)\\)。因为在复数范围内 \\(\beta\\) 只有 3 种可能的取值，即单位立方根。但 \\(\beta\\) 是实数，所以只可能是 \\(\beta=\alpha\\)。

于是 \\(h(\alpha)=\alpha\\)。这表明 \\(h\\) 固定 \\(\Q(\alpha)\\)，因此它是恒等映射。这就说明了不存在恒等映射以外的映射满足条件。

---

__G.4__ 说明为什么 G.3 的结论不违反定理 1。

**答** 定理 1 说的是分裂域 K 和 F 的关系：Gal(K:F) = [K:F]。但 G.3 中的域不是分裂域，所以不能应用定理 1。

---

在接下来的三个问题中设 \\(\omega\\) 是本原单位 \\(p\\) 次方根，其中 \\(p\\) 是质数。

__G.5__ 证明：如果 \\(h\in \Gal(\Q(\omega):\Q)\\)，那么 \\(h(\omega)=\omega^k\\) 对某个 \\(1\le k\le p-1\\) 成立。

**证明** 不可约多项式 \\(a(x)=x^{p-1}+x^{p-2}+\cdots+x+1=\frac{x^p-1}{x-1}\\) 的所有根为 \\(\omega,\omega^2,\ldots,\omega^{p-1} \in \Q(\omega)\\)，因此 \\(\Q(\omega)\\) 是 \\(a(x)\\) 的分裂域。因为 \\(h\in \Gal(\Q(\omega):\Q)\\)，因此 \\(h\\) 把 \\(a(x)\\) 的根映射到 \\(a(x)\\) 的根，即存在 \\(1\le k\le p-1\\) 使得 \\(h(\omega)=\omega^k\\)。

---

__G.6__ 使用 G.5 证明 \\(\Gal(\Q(\omega):\Q)\\) 是 Abel 群。

**证明** 设 \\(h,h'\in \Gal(\Q(\omega):\Q)\\)，根据 G.5 可知 \\(h(\omega)=\omega^k\\) 且 \\(h'(\omega)=\omega^{k'}\\)，其中 \\(1\le k,k'\le p-1\\)。设 \\(x\in\Q(\omega)\\)，则 \\(x=\sum_{i=0}^n c_i\omega^i\\)。于是
\\[h(h'(x)) = \sum_{i=0}^n c_i\omega^{kk'i},\quad h'(h(x)) = \sum_{i=0}^n c_i\omega^{k'ki}\\]
这表明 \\(h\circ h'=h'\circ h\\)，因此 \\(\Gal(\Q(\omega):\Q)\\) 是 Abel 群。

---

__G.7__ 使用 G.5 证明 \\(\Gal(\Q(\omega):\Q)\\) 是循环群。

**证明** 根据 G.5 可知 \\(\Gal(\Q(\omega):\Q)\\) 中的全部元素可以描述为
\\[h_i(\omega) = \omega^i\qquad(1\le i\le p-1)\\]
于是 \\((h_i\circ h_j)(\omega) = \omega^{ij}\\)。这表明 \\(h_i\circ h_j = h_k\\) 当且仅当 \\(ij\equiv k\pmod p\\)。因此 \\(\Gal(\Q(\omega):\Q) \cong \mathbb Z_p^\ast\\) （同构为 \\(h_i\leftrightarrow \bar i\\)），而后者是循环群。

## H. &Copf; 的自同构群

__H.2__ 求证：&Ropf; 的任意自同构把平方元素映射到平方元素，即把正数映射到正数。

**证明** 设 h: &Ropf;&rarr;&Ropf; 是自同构，并设 x = a² &isin; &Ropf;。那么 h(x) = h(a)²。可见 h(x) 是平方元素。设 x' 是正数，因为正数在实数范围内总是可以开平方根，因此 x' 是平方元素。因为 x' &ne; 0，所以 h(x) &ne; 0。由此可知 h(x') 是非零平方数，因此是正数。

---

__H.3__ 使用 H.2 证明：如果 h 是 &Ropf; 的自同构，那么 a < b 蕴含 h(a) < h(b)。

**证明** a < b 等价于 b-a 是正数，根据 H.2 知 h(b-a) 也是正数。因此 h(b-a) = h(b)-h(a) > 0。因此 h(a) < h(b)。

---

__H.4__ 使用 H.1 和 H.3 证明 &Ropf; 的自同构只有恒等映射。

**证明** 设 h 是 &Ropf; 的自同构。仿照 H.1 可知 h(x) = x 对任意 x &isin; &Qopf; 都成立。
假设存在 a &isin; &Ropf; 满足 h(a) &ne; a。如果 a < h(a)，那么根据有理数的稠密性可知存在 b &isin; &Qopf; 使得 a < b < h(a)。根据 H.3 可知 a < b 蕴含 h(a) < h(b)。而 h(b) = b，于是得到 h(a) < b < h(a)，这是不可能的。如果 a > h(b) 那么可以推出类似的矛盾。因此假设不可能成立，只可能是 h(x) = x 对任意 x &isin; &Ropf; 都成立。

**注** 这里用到了有理数的稠密性，这是抽象代数以外的知识。

---

__H.5__ 列出 Gal(&Copf;:&Ropf;) 中的元素。

**解** 由 [&Copf;:&Ropf;] = 2 可知 Gal(&Copf;:&Ropf;) 共有 2 个元素。显然恒等映射 &epsilon; 是一个元素。另一个元素 &sigma; 是将每个复数映射为它的共轭复数；不难证明它是同构。Gal(&Copf;:&Ropf;) = {&epsilon;, &sigma;}。

---

__H.6__ 证明恒等映射和映射 a+bi &rarr; a-bi 是仅有的固定 &Ropf; 的 &Copf; 的自同构。

**证明** 根据 Galois 群的定义和 H.5 的结论可知。

## I. 关于 Galois 群的更多问题

在下列问题中，设 \\(K\\) 是 \\(F\\) 的分裂域，设 \\(\mathbf G = \Gal(K:F)\\)，并设 \\(I\\) 是中间域。证明下列命题。

__I.1__ \\(I^\ast = \Gal(K:I)\\) 是 \\(\mathbf G\\) 的子群。

**证明** 不难证明 \\(I^\ast\\) 非空（含有恒等映射）且是 \\(\mathbf G\\) 的子集（固定 \\(I\\) 的自同构一定固定 \\(F\\)）。

设 \\(h_1,h_2\in I^\ast\\)，则对任意 \\(x\in I\\) 有 \\(h_1(h_2(x)) = h_1(x) = x\\)，因此 \\(h_1\circ h_2 \in I^\ast\\)。同理可证 \\(h_2\circ h_1\in I^\ast\\)。

设 \\(h\in I^\ast\\)，则对任意 \\(x\in I\\) 有 \\(h(x)=x\\)。两边应用 \\(h^{-1}\\) 得 \\(h^{-1}(x)=x\\)。因此 \\(h^{-1}\in I^\ast\\)。

综上所述，\\(I^\ast \le\mathbf G\\)。

---

__I.2__ 如果 \\(H\le \mathbf G\\) 且 \\(H^\circ = \\{a\in K : \forall \pi\in H,\\,\pi(a) = a\\}\\)，那么 \\(F\subseteq H^\circ\subseteq K\\)。

**证明** 显然 \\(H^\circ\\) 是 \\(K\\) 的子集。

设 \\(a,b \in H^\circ\\)，那么对任意 \\(\pi\in\ H\\) 有 \\(\pi(a)=a\\) 且 \\(\pi(b)=b\\)。因为 \\(\pi\\) 是同构，所以 \\(\pi(a-b) = \pi(a)-\pi(b) = a-b\\)。这表明 \\(a-b \in H^\circ\\)。可见，\\(H^\circ\\) 对减法封闭，因而是 \\(K\\) 的子环。又因为对任意 \\(a\in H^\circ\\)，有 \\(\pi(a^{-1})=\pi(a)^{-1} = a^{-1}\\)，所以 \\(H^\circ\\) 对乘法逆元封闭。由此可见 \\(H^\circ\\) 是 \\(K\\) 的子域。

设 \\(x\in F\\)，那么对于任意 \\(\pi\in H\subseteq \mathbf G\\)，有 \\(\pi(x) = x\\)，因此 \\(x\in H^\circ\\)。这就证明了 \\(F\subseteq H^\circ\\)。

---

__I.3__ 设 \\(H\\) 是 \\(I\\) 的固定子，且 \\(I'\\) 是 \\(H\\) 的固定域，那么 \\(I\subseteq I'\\)。设 \\(I\\) 是 \\(H\\) 的固定域，且 \\(I^\ast\\) 是 \\(I\\) 的固定子，那么 \\(H\subseteq I^\ast\\)。

**证明** 这些命题在本章正文部分被描述为“根据定义可知”。这里只是详细地按照定义进行推导。

（命题一）设 \\(x\in I\\)，因为 \\(H\\) 是 \\(I\\) 的固定子，所以对于任意 \\(\pi\in H\\) 都有 \\(\pi(x)=x\\)。按照固定域的定义：
\\[I'=\\{a\in K : \forall \pi\in H,\\,\pi(a)=a\\}\\]
这表明 \\(x\in I'\\)。因此 \\(I\subseteq I'\\)。

（命题二）因为 \\(I^\ast\\) 是 \\(I\\) 的固定子，所以它包含了所有固定 \\(I\\) 的 \\(K\\) 的自同构。
设 \\(\pi\in H\\)。按固定域的定义：
\\[I=\\{a\in K : \forall \pi\in H,\\,\pi(a)=a\\}\\]
由此可见 \\(\pi\\) 固定 \\(I\\)，从而有 \\(\pi\in I^\ast\\)。因此 \\(H\subseteq I^\ast\\)。

---

__I.4__ 设 \\(I\\) 是 \\(F\\) 的正规扩张。如果 \\(\mathbf G\\) 是 Abel 群，那么 \\(\Gal(K:I)\\) 和 \\(\Gal(I:F)\\) 都是 Abel 群。

**证明** 根据 I.1 可知 \\(H=\Gal(K:I)\\) 是 \\(\mathbf G\\) 的子群。因为 \\(\mathbf G\\) 是 Abel 群，具备交换律，所以具备相同运算法则的子群 \\(\Gal(K:I)\\) 也是 Abel 群。

根据本章定理 4，有 \\(\Gal(I:F) \cong \mathbf G / H\\)。右侧的商群中的每个元素都是 \\(Ha\\) 的形式。根据陪集的运算法则：
\\[Ha \cdot Hb = H(ab) = H(ba) = Hb \cdot Ha\\]
因此商群也是 Abel 群。这就证明了 \\(\Gal(I:F)\\) 是 Abel 群。

---

__I.5__ 设 \\(I\\) 是 \\(F\\) 的正规扩张。如果 \\(\mathbf G\\) 是循环群，那么 \\(\Gal(K:I)\\) 和 \\(\Gal(I:F)\\) 都是循环群。

**引理** 循环群的子群都是循环群。

设 \\(G=\langle g\rangle\\) 是循环群，其中 \\(g\\) 是它的生成元。设 \\(H\\) 是 \\(G\\) 的子群。
如果 \\(H\\) 是平凡群，那么显然它是循环群。下面只考虑非平凡群的情况。

\\(H\\) 中的元素都具备 \\(g^k\\) 的形式。根据良序原理，设 \\(k\\) 能取到的最小的正整数是 \\(a\\)。对于任意的 \\(x=g^b\in H\\)，做带余除法 \\(b=aq+r\\)，其中 \\(q,r\\) 是整数且 \\(0\le r < a\\)。那么 \\(g^r = g^b(g^a)^{-q}\in H\\)。但 \\(a\\) 是最小的正指数，所以只可能是 \\(r=0\\)。于是 \\(x=(g^a)^q\\)。这表明 \\(g^a\\) 是 \\(H\\) 的生成元。由此可知 \\(H\\) 是循环群。

**证明** 根据 I.1 可知 \\(H=\Gal(K:I)\\) 是 \\(\mathbf G\\) 的子群。因为 \\(\mathbf G\\) 是循环群，根据引理可知 \\(\Gal(K:I)\\) 也是循环群。

根据本章定理 4，有 \\(\Gal(I:F) \cong \mathbf G / H\\)。设 \\(\mathbf G = \langle \pi\rangle\\)。如果 \\(H\\) 是平凡群，则 \\(\Gal(I:F) \cong \mathbf G\\) 显然是循环群。否则，根据引理可设 \\(H=\langle \pi^k\rangle\\)，其中 \\(k\\) 为正整数。那么商群 \\(\mathbf G / H\\) 中的所有元素为
\\[H,H\pi,H\pi^2,\ldots,H\pi^{k-1}\\]
可见 \\(\mathbf G/H = \langle H\pi\rangle\\) 是循环群。

---

__I.6__ 如果 \\(\mathbf G\\) 是循环群，那么对于任意整除 \\([K:F]\\) 的正数 \\(k\\)，恰有一个 \\(k\\) 次中间域 \\(I\\)。

**证明** 设 \\([K:F]=n\\)。则根据本章定理 1 可知 \\(|\mathbf G|=n\\)。

设 \\(\mathbf G = \langle \pi\rangle\\)，并设 \\(m=n/k\\)，那么 \\(\mathbf G\\) 的 \\(m\\) 阶子群只能是 \\(H=\langle\pi^k\rangle\\)。设 \\(I\\) 是 \\(H\\) 的固定域。根据本章定理 1，有 \\([K:I]=|H|=m\\)。于是 \\([I:F]=[K:F]/[K:I]=k\\)。

根据 Galois 对应，\\(\mathbf G\\) 的子群和中间域一一对应。所以上述中间域 \\(I\\) 是唯一的。

## J. 正规扩张与正规子群

假定 \\(F\subseteq K\\)，其中 \\(K\\) 是 \\(F\\) 的正规扩张。设 \\(I_1\subseteq I_2\\) 为中间域。

__J.1__ 从定理 4 推导：如果 \\(I_2\\) 是 \\(I_1\\) 的正规扩张，那么 \\(I_2^\ast\\) 是 \\(I_1^\ast\\) 的正规子群。

**证明** 因为 \\(F\subseteq I_2\\) 而 \\(K\\) 是 \\(F\\) 的分裂域，所以 \\(K\\) 是 \\(I_2\\) 的分裂域。根据定理 4 有 \\(\Gal(I_2:I_1)=\Gal(K:I_1)/\Gal(K:I_2) = I_1^\ast/I_2^\ast\\)。根据商群的定义，\\(I_2^\ast\unlhd I_1^\ast\\)。

---

__J.2__ 证明一下命题对任意中间域 \\(I\\) 都成立：设 \\(h\in\Gal(K:F)\\)，\\(g\in I^\ast\\)，\\(a\in I\\) 且 \\(b=h(a)\\)。那么 \\(\[h\circ g\circ h^{-1}] (b) = b\\)。做出结论：\\(hI^\ast h^{-1}\subseteq h(I)^\ast\\)。

**证明**
\begin{align\*}
\[h\circ g\circ h^{-1}] (b)
&= \[h\circ g\circ h^{-1}] (h(a))\\\\
&= h(g(h^{-1}(h(a)))) \\\\
&= h(g(a)) = h(a) = b
\end{align\*}

因为 \\(b\in h(I)\\) 和 \\(g\in I^\ast\\) 可以任取，所以 \\(hI^\ast h^{-1}\\) 中的任意元素都固定 \\(h(I)\\) 中的元素。这就说明了 \\(hI^\ast h^{-1}\subseteq h(I)^\ast\\)。

---

__J.3__ 使用 J.2 证明 \\(hI^\ast h^{-1} = h(I)^\ast\\)。

**证明** 设 \\(g\in h(I)^\ast\\)。对任意 \\(a\in I\\) 有 \\(h(a)\in h(I)\\)，于是
\\[g(h(a)) = h(a)\\]
两边应用 \\(h^{-1}\\) 得
\\[h^{-1}(g(h(a))) = a\\]
这表明 \\(\hat g = h^{-1}\circ g\circ h\\) 固定 \\(I\\)。于是 \\(\hat g\in I^\ast\\)。由此可知
\\[g=h\circ \hat g\circ h^{-1} \in h I^\ast h^{-1}\\]
这表明 \\(h(I)^\ast \subseteq hI^\ast h^{-1}\\)。而 J.2 已经证明了另一个方向的包含关系，因此 \\(h(I)^\ast=hI^\ast h^{-1}\\)

---

两个中间域被称为*共轭的 (conjugate)* 当且仅当存在自同构 \\(i\in\Gal(K:F)\\) 使得 \\(i(I_1)=I_2\\)。

__J.4__ 使用 J.3 证明 \\(I_1\\) 和 \\(I_2\\) 是共轭的当且仅当 \\(I_1^\ast\\) 和 \\(I_2^\ast\\) 是 Galois 群的共轭子群。

**证明**

（必要性）当 \\(I_1\\) 和 \\(I_2\\) 是共轭时，存在 \\(i\in\Gal(K:F)\\) 使得 \\(i(I_1)=I_2\\)。根据 J.3 得 \\(I_2^\ast = i(I_1)^\ast = iI_1^\ast i^{-1}\\)，因此 \\(I_2^\ast\\) 和 \\(I_1^\ast\\) 为共轭子群。

（充分性）当 \\(I_2^\ast\\) 和 \\(I_1^\ast\\) 为共轭子群时，存在 \\(i\in\Gal(K:F)\\) 使得 \\(I_2^\ast = iI_1^\ast i^{-1}\\)。根据 J.3 得 \\(iI_1^\ast i^{-1} = i(I_1)^{\ast}\\)，因此 \\(I_2^\ast=i(I_1)^\ast\\)。因为 Galois 对应是中间域和 Galois 子群之间的一一对应，所以 \\(I_2=i(I_1)\\)。这表明 \\(I_1\\) 和 \\(I_2\\) 是共轭的。

---

__J.5__ 使用 J.4 证明对于任意两个中间域 \\(I_1\\) 和 \\(I_2\\)，如果 \\(I_2^\ast\\) 是 \\(I_1^\ast\\) 的正规子群，那么 \\(I_2\\) 是 \\(I_1\\) 的正规扩张。

**证明** 设 \\(x\in I_2^\ast\\)，因为 \\(I_2^\ast \unlhd I_1^\ast\\)，所以对任意 \\(h\in I_1^\ast\\) 都有 \\(hxh^{-1}\in I_2^\ast\\)。设将 \\(h\\) 的定义域限制为 \\(I_2\\) 得到的同构为 \\(\hat h\\)，则 \\(\hat hI_2^\ast \hat h^{-1} \subseteq I_2^\ast\\)。根据 J.4 可知 \\(\hat h(I_2)^\ast = \hat hI_2^\ast \hat h^{-1}\\)，所以 \\(\hat h(I_2)^\ast \subseteq I_2^\ast\\)。因为 \\(\hat h\\) 是 \\(I_2\\) 的固定 \\(I_1\\) 的自同构，所以根据第三十一章习题 K.2 可知 \\(I_2\\) 是 \\(I_1\\) 的正规扩张。

---

结合 J.1 和 J.5 可知：\\(I_2\\) 是 \\(I_1\\) 的正规扩张当且仅当 \\(I_2^\ast\\) 是 \\(I_1^\ast\\) 的正规子群。（历史上，这个结论是术语“正规子群”中“正规”的来源。）