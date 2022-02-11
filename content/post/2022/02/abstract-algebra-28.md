---
title: 抽象代数习题(28) -- 向量空间
date: 2022-02-05T19:48:30-08:00
---

《抽象代数》第二十八章讲的是向量空间的基础知识。本书对读者（比如我）的基础知识要求不高，专门用了一个章节讨论这些通常在线性代数课中应已涵盖的内容。在后续的章节中，常会把用域 F 中的元素视为数，扩域 K 中的元素视为“向量”，使他们构成一个向量空间，然后用向量空间的性质证明一些结论。

<!--more-->

__B.2__ 证明 \\(\\{(a,b,c)\in\mathbb R^3 : 2a-3b+c=0\\}\\) 是 \\(\mathbb R^3\\) 的子空间。

**证明** 设 \\(U=\\{(a,b,c)\in\mathbb R^3 : 2a-3b+c=0\\}\\)。显然 \\(U\subseteq \mathbb R^3\\)。设 \\(\boldsymbol x=(a_1,b_1,c_1),\boldsymbol y=(a_2,b_2,c_2)\in U\\)，则有
\\[2(a_1+a_2)-3(b_1+b_2)+(c_1+c_2) = 0\\]
这表明 \\(\boldsymbol x+\boldsymbol y\in U\\)，因此 \\(U\\) 对加法封闭。

这里讨论的数域 \\(F=\mathbb R\\)。对任意 \\(k\in \mathbb R\\) 有
\\[2(ka_1)-3(kb_1)+(kc_1) = 0\\]
这表明 \\(k\boldsymbol x \in U\\)，因此 \\(U\\) 对数乘封闭。

综上所述，\\(U\\) 是 \\(\mathbb R^3\\) 的子空间。

---

__C.5__ 求下列 \\(\mathbb R^3\\) 的子空间的一组基。

1. \\(S_1=\\{(x,y,z)\in\mathbb R^3 : 3x-2y+z=0\\}\\)
2. （其余略）

**解** \\(S_1\\) 的一组基为 \\(B=\\{(1,0,-3),(0,1,2)\\}\\)。首先，不难验证这一组向量线性无关。其次，\\(B\\) 中向量的任意线性组合都在 \\(S_1\\) 中。最后，对于 \\(S_1\\) 的任意向量 \\((x,y,z)\\)，可以将其分解为
\\[(x,y,z) = x(1,0,-3) + y(0,1,2)\\]
可见，\\(B\\) 生成 \\(S_1\\)。因此，\\(B\\) 是 \\(S_1\\) 的基。

---

__C.6__ 求 \\(\mathbb R^3\\) 的由下述向量集合生成的子空间的一组基：所有满足 \\(x^2+y^2+z^2=1\\) 的向量。

**解** 易知 \\(\boldsymbol e_1=(1,0,0)\\), \\(\boldsymbol e_2=(0,1,0)\\), \\(\boldsymbol e_3=(0,0,1)\\) 都满足 \\(x^2+y^2+z^2=1\\)。它们线性无关。（事实上，它们生成的子空间就是 \\(\mathbb R^3\\) 本身。）所以 \\(\\{\boldsymbol e_1, \boldsymbol e_2, \boldsymbol e_3\\}\\) 就是满足要求的一组基。

**注** 这题问的不是 \\(\\{(x,y,z)\in \mathbb R^3 : x^2+y^2+z^2\\}\\) 的一组基；事实上，它根本就不是向量空间。

---

__C.7__ 设 \\(U\\) 是 \\(\mathscr F(\mathbb R)\\) 的子空间且由 \\(\\{\cos^2 x, \sin^2 x, \cos 2x\\}\\) 生成。求 \\(U\\) 的维数，并求 \\(U\\) 的一组基。

**解** 设 \\(B=\\{\cos^2 x, \sin^2 x, \cos 2x\\}\\)。根据三角函数知识可知 \\(\cos 2x = \cos^2 x - \sin^2 x\\)。根据本章引理 2，\\(B\\) 中删去 \\(\cos 2x\\) 后得到的 \\(B'=\\{\cos^2x, \sin^2x\\}\\) 仍然生成 \\(U\\)。这时，列方程
\\[k_1\cos^2x + k_2\sin^2x = 0\\]
因为 \\(\cos^2x + \sin^2x = 1\\)，所以方程等价于
\\[(k_1-k_2)\cos^2x + k_2 = 0\\]
假设 \\(k_1-k_2\ne 0\\)，则
\\[\cos^2x = \frac{k_2}{k_2-k_1}\\]
这是不可能的，因为等式右边是常数，而 \\(\cos^2x\\) 显然不是一个常数函数。所以只可能是 \\(k_1-k_2=0\\)，进而可以解得 \\(k_1=k_2=0\\)。这表明 \\(B'\\) 中的向量线性无关。

综上所述，\\(B'=\\{\cos^2x,\sin^2x\\}\\) 是 \\(U\\) 的一组基。因此 \\(\dim U=2\\)。

---

设 \\(V\\) 是有限维向量空间。记 \\(\dim V\\) 为 \\(V\\) 的维数。证明下列命题。

__D.1__ 如果 \\(U\\) 是 \\(V\\) 的子空间，则 \\(\dim U\le \dim V\\)。

**证明** 设 \\(n=\dim U\\), \\(m=\dim V\\)。那么 \\(\\{\boldsymbol e_1,\ldots,\boldsymbol e_n\\}\\) 是 \\(U\\) 的一组基，于是 \\(\boldsymbol e_1,\ldots,\boldsymbol e_n\\) 线性无关。假设 \\(n > m\\)，则 \\(\\{\boldsymbol e_1,\ldots,\boldsymbol e_m\\}\\) 是 \\(V\\) 中一组线性无关的向量，从而构成 \\(V\\) 的一组基。而 \\(\boldsymbol e_{m+1}\in U\subseteq V\\)，这意味着 \\(\boldsymbol e_{m+1}\\) 可以用 \\(\\{\boldsymbol e_1,\ldots,\boldsymbol e_m\\}\\) 的线性组合表示，这和 \\(\boldsymbol e_1,\ldots,\boldsymbol e_n\\) 线性无关矛盾。因此假设不成立，只可能是 \\(n\le m\\)。

---

__D.2__ 如果 \\(U\\) 是 \\(V\\) 的子空间，且 \\(\dim U = \dim V\\)，则 \\(U=V\\)。

**证明** 设 \\(n=\dim U=\dim V\\)。那么 \\(\\{\boldsymbol e_1,\ldots,\boldsymbol e_n\\}\\) 是 \\(U\\) 的一组基。因为 \\(U\\) 是 \\(V\\) 的子空间，所以 \\(\boldsymbol e_1,\ldots,\boldsymbol e_n\\) 都是 \\(V\\) 中的向量，而 \\(\dim V=n\\)，所以这一组向量也是 \\(V\\) 的一组基。这表明 \\(U\\) 和 \\(V\\) 由同一组向量生成，所以 \\(U=V\\)。

---

__D.6__ 如果 \\(\\{\boldsymbol a,\boldsymbol b,\boldsymbol c\\}\\) 线性无关，那么 \\(\\{\boldsymbol a+\boldsymbol b,\boldsymbol b+\boldsymbol c,\boldsymbol a+\boldsymbol c\\}\\) 也线性无关。

**证明** 列方程
\\[k_1(\boldsymbol a+\boldsymbol b)+k_2(\boldsymbol b+\boldsymbol c)+k_2(\boldsymbol a+\boldsymbol c)=0\\]
该方程等价于
\\[(k_1+k_3)\boldsymbol a + (k_1+k_2)\boldsymbol b + (k_2+k_3)\boldsymbol c = 0\\]
根据 \\(\\{\boldsymbol a,\boldsymbol b,\boldsymbol c\\}\\) 线性无关可知 \\(k_1+k_3=k_1+k_2=k_2+k_3=0\\)。解得 \\(k_1+k_2+k_3=0\\)。因此 \\(\\{\boldsymbol a+\boldsymbol b,\boldsymbol b+\boldsymbol c,\boldsymbol a+\boldsymbol c\\}\\) 也线性无关。

---

设 \\(U\\) 和 \\(V\\) 是 \\(F\\) 上的有限维向量空间，并设 \\(h\colon U\to V\\) 是线性变换。证明 1--3。

__E.1__ \\(h\\) 的核是 \\(U\\) 的子空间。（这称为 \\(h\\) 的*零空间 (null space)*。）

**证明** 设 \\(K\\) 是 \\(h\\) 的核。显然 \\(K\subseteq U\\)。设 \\(\boldsymbol x,\boldsymbol y\in K\\)，那么 \\(h(\boldsymbol x)=h(\boldsymbol y)=\boldsymbol 0\\)。于是
\\[h(\boldsymbol x + \boldsymbol y) = h(\boldsymbol x)+h(\boldsymbol y)=\boldsymbol 0+\boldsymbol 0=\boldsymbol 0\\]
这意味着 \\(K\\) 对加法封闭。

另一方面，
\\[h(k\boldsymbol x) = kh(\boldsymbol x) = k\boldsymbol 0 = \boldsymbol 0\\]
这意味着 \\(K\\) 对数乘封闭。

综上所述，\\(K\\) 是 \\(U\\) 的子空间。

---

__E.2__ \\(h\\) 的值域是 \\(V\\) 的子空间。（这称为 \\(h\\) 的*值域空间 (range space)*。）

**证明** 值域中的任意元素都有原像。仿照 E.1 的证明，利用 \\(h\\) 的线性性质证明值域对加法和数乘封闭即可。

---

设 \\(\mathscr N\\) 为 \\(h\\) 的零空间，且 \\(\mathscr R\\) 为 \\(h\\) 的值域空间。设 \\(\\{\boldsymbol a_1,\ldots,\boldsymbol a_r\\}\\) 为 \\(\mathscr N\\) 的一组基。将其扩展为 \\(U\\) 的一组基 \\(\\{\boldsymbol a_1,\ldots,\boldsymbol a_r,\ldots,\boldsymbol a_n\\}\\)。

证明 4--6。

__E.4__ 任意向量 \\(\boldsymbol b\in\mathscr R\\) 都是 \\(h(\boldsymbol a_{r+1}),\ldots,h(\boldsymbol a_n)\\) 的线性组合。

**证明** 因为 \\(\boldsymbol b \in \mathscr R\\)，所以存在 \\(\boldsymbol a\in U\\) 使得 \\(h(\boldsymbol a) = \boldsymbol b\\)。因为 \\(\\{\boldsymbol a_1,\ldots,\boldsymbol a_r,\ldots,\boldsymbol a_n\\}\\)是 \\(U\\) 的一组基，所以可以将 \\(\boldsymbol a\\) 表示为它们的线性组合：存在 \\(k_1,\cdots,k_n\in F\\) 使得
\\[\boldsymbol a = k_1\boldsymbol a_1+\ldots+k_n\boldsymbol a_n\\]
根据线性映射的线性性质，有
\begin{align\*}
\boldsymbol b=h(\boldsymbol a) &= h(k_1\boldsymbol a_1+\ldots+k_n\boldsymbol a_n)\\\\
&=k_1\underbrace{h(\boldsymbol a_1)}\_{=0}+\cdots+k_r\underbrace{h(\boldsymbol a_r)}\_{=0}+k_{r+1}h(\boldsymbol a_{r+1})+\cdots+k_nh(\boldsymbol a_n)\\\\
&=k_{r+1}h(\boldsymbol a_{r+1})+\cdots+k_nh(\boldsymbol a_n)
\end{align\*}
这表明 \\(\boldsymbol b\\) 可以表示为 \\(h(\boldsymbol a_r),\ldots,h(\boldsymbol a_n)\\) 的线性组合。这就是所要证明的。

---

__E.5__ \\(\\{h(\boldsymbol a_{r+1}),\ldots,h(\boldsymbol a_n)\\}\\) 线性无关。

**证明** 假设 \\(\\{h(\boldsymbol a_{r+1}),\ldots,h(\boldsymbol a_n)\\}\\) 线性相关，那么存在不全为零的 \\(k_{r+1},\ldots,k_n\in F\\) 使得
\\[k_{r+1}h(\boldsymbol a_{r+1})+\ldots+k_n h(\boldsymbol a_n) = \boldsymbol 0\\]
设
\\[\boldsymbol a = k_{r+1}\boldsymbol a_{r+1}+\cdots+k_n\boldsymbol a_n\\]
那么，根据线性映射的性质可知 \\(h(\boldsymbol a) = \boldsymbol 0\\)。这表明 \\(\boldsymbol a\in \mathscr N\\)，因此它可以表示为 \\(\boldsymbol a_1,\ldots,\boldsymbol a_r\\) 的线性组合：存在 \\(k_1,\ldots,k_r\in F\\) 使得
\\[\boldsymbol a = k_1\boldsymbol a_1+\cdots+k_r\boldsymbol a_r\\]
于是
\\[k_1\boldsymbol a_1+\cdots+k_r\boldsymbol a_r-k_{r+1}\boldsymbol a_{r+1}-\cdots-k_n\boldsymbol a_n = \boldsymbol 0\\]
注意以上等式中的系数不全为零，这意味着 \\(\\{\boldsymbol a_1,\ldots,\boldsymbol a_n\\}\\) 线性相关。这和 \\(\\{\boldsymbol a_1,\ldots,\boldsymbol a_n\\}\\) 是 \\(U\\) 的一组基（应当线性无关）矛盾！所以假设不成立，因此只可能是 \\(\\{h(\boldsymbol a_{r+1}),\ldots,h(\boldsymbol a_n)\\}\\) 线性无关。

---

__E.6__ \\(\mathscr R\\) 的维数是 \\(n-r\\)。

**证明** 根据 E.3 和 E.4 可知 \\(\\{h(\boldsymbol a_{r+1}),\ldots,h(\boldsymbol a_n)\\}\\) 是 \\(\mathscr R\\) 的一组基。因为这组基有 \\(n-r\\) 个向量，所以 \\(\dim \mathscr R = n-r\\)。

---

__E.7__ 作出如下结论：对于任意线性变换 \\(h\\)，定义域的维数等于零空间的维数加值域空间的维数。

**证明** 根据 E.6 立即得到结论。

---

__E.8__ 设 \\(U\\) 和 \\(V\\) 有相同的维数 \\(n\\)。使用 E.7 证明 \\(h\\) 是单射当且仅当 \\(h\\) 是满射。

**证明** 设 \\(h\\) 是满射，那么 \\(h\\) 的值域空间就是 \\(V\\)。根据 E.7 可知 \\(h\\) 的零空间是零维的，即 \\(\\{\boldsymbol 0\\}\\)。设 \\(h(\boldsymbol a)=h(\boldsymbol b)\\)，其中 \\(\boldsymbol a,\boldsymbol b\in U\\)，那么
\\[h(\boldsymbol a-\boldsymbol b)=h(\boldsymbol a)-h(\boldsymbol b)=\boldsymbol 0\\]
所以 \\(\boldsymbol a - \boldsymbol b\\) 在 \\(h\\) 的零空间中，那么只可能是 \\(\boldsymbol a - \boldsymbol b = \boldsymbol 0\\)，于是 \\(\boldsymbol a = \boldsymbol b\\)。这就证明了 \\(h\\) 是单射。

反过来，设 \\(h\\) 是单射。对于 \\(h\\) 的零空间的任意元素 \\(a\\) 都有 \\(h(a) = 0\\)。而因为零空间是线性空间，所以一定包含 \\(\boldsymbol 0\\)。根据 \\(h\\) 是单射可知 \\(\boldsymbol a = 0\\)。因此，\\(h\\) 的零空间只有唯一的元素 \\(\boldsymbol 0\\)，因此是零维空间。根据 E.7 可知 \\(h\\) 的值域空间是 \\(V\\) 的 \\(n\\) 维子空间。而 \\(V\\) 的维数是 \\(n\\)，根据 D.2 可知 \\(h\\) 的值域空间就是 \\(V\\)。这就证明了 \\(h\\) 是满射。
