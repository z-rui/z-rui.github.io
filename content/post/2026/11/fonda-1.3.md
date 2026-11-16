---
title: 数学分析习题 (1.1--1.3)
date: 2026-11-13T21:04:09-08:00
---

1. 在有理数域 \\(\mathbb Q\\) 内讨论问题。设
$$
A=\\{r\in \mathbb Q \mid r > 0 \text{ 且 } r^2 < 2\\},\\,
B=\\{r\in \mathbb Q \mid r > 0 \text{ 且 } r^2 > 2\\}.
$$
   1) 证明：对任意 \\(a\in A\\) 与 \\(b\in B\\)，均有 \\(a < b\\)；
   2) 证明：\\(A\\) 在 \\(\mathbb Q\\) 中没有上确界，\\(B\\) 在 \\(\mathbb Q\\) 中也没有下确界。此结论说明有理数集破坏了实数的哪一条核心公理？

   <details><summary>答案</summary>
   
   1) 设\\(a\in A\\), \\(b\in B\\)，那么\\(a^2 < 2 < b^2\\)。根据平方比较引理（\\(a\le b \Rightarrow a^2 \le b^2\\)）可知 \\(a < b\\)。
   
   2) 假设存在\\(s\in\mathbb Q\\) 使得 \\(s = \sup A\\)。
   首先，因为 \\(1\in A\\)，所以\\(s\ge 1>0\\)。
   
      1. 如果 \\(s^2 > 2\\)，那么对任意自然数 \\(n \ge 1\\)：
   $$ \left(s - \frac{1}{n} \right)^2 = s^2 - \frac{2s}{n} + \frac{1}{n^2} > s^2 - \frac{2s}{n}. $$
   取 \\(n > \frac{2s}{s^2-2}\\) （由阿基米德性质保证可取）时，有
   $$ s - \frac{1}{n} > 0 \quad\text{且}\quad \left(s - \frac{1}{n} \right)^2 > 2.$$
   因此 \\(s-\frac{1}{n} \in B\\)。根据第1小问的结论可知 \\(s-\frac{1}{n}\\) 是 \\(A\\) 的上界。但 \\(s\\) 是 \\(A\\) 的上确界，因此 \\(s < s-\frac{1}{n}\\)。显然不可能。
   
      2. 如果 \\(s^2 < 2\\)，那么对任意自然数 \\(n \ge 1\\)：
   $$ \left(s + \frac{1}{n} \right)^2 = s^2 + \frac{2s}{n} + \frac{1}{n^2} < s^2 - \frac{2s}{n} + \frac{1}{n} = s^2 - \frac{2s+1}{n}. $$
   取 \\(n > \frac{2s+1}{2-s^2}\\) 时，有 \\(s+\frac{1}{n})^2 < 2\\)。
   显然\\(s + \frac{1}{n}>0\\)，因此 \\(s + \frac{1}{n} \in A\\)。而\\(s\\)是\\(A\\)的上界，所以\\(s \ge s + \frac{1}{n}\\)。显然也不可能。
   
      综上所述，\\(s^2 < 2\\) 和 \\(s^2 > 2\\) 均不可能。于是只可能 \\(s^2 = 2\\)。但是，\\(2\\) 没有有理数的平方根是众所周知的（参见定理 1.2），因此产生矛盾，假设不能成立。这就说明 \\(A\\) 在 \\(\mathbb Q\\) 中没有上确界。
   
      仿此可证明 \\(B\\) 在 \\(\mathbb Q\\) 中没有下确界，这里从略。
   
      此结论说明 \\(\mathbb Q\\) 破坏了实数的**完备性**公理。
   
   </details>

2. 求下列数集的上确界与下确界，并判断它们是否属于原集合。
   1) \\(A = \left\\{ \frac{(-1)^n}{n} + \frac{1+(-1)^n}{2} \\;\middle|\\; n \in\mathbb Z_+\right\\}\\),
   2) \\(B = \left\\{ \frac{n}{m+n} \\;\middle|\\; m, n \in\mathbb Z_+\right\\}\\).

   <details> <summary>答案</summary>

   1) 设通项为 \\(a_n\\)。
      1. 当 \\(n\\) 为奇数时，\\(a_n = -\frac{1}{n}\\)。因为 \\(n\ge 1\\) 且无上界，所以 \\(-1 \le a_n < 0\\)。
      2. 当 \\(n\\) 为偶数时，\\(a_n = \frac{1}{n} + 1\\)。因为 \\(n\ge 2\\) 且无上界，所以 \\(1 < a_n \le \frac{3}{2}\\)。

      综上有 \\(-1 \le a_n \le \frac{3}{2}\\)。以上等号均能取得（\\(n = 1\\) 和 \\(n = 2\\)）。根据确界定义可知 \\(\inf A = -1\\), \\(\sup A = \frac{3}{2}\\)，且都属于 \\(A\\)。

   2) 因为 \\(m, n\in\mathbb Z_+\\)，所以 \\(0 < n < m+n\\)。于是
$$ 0 < \frac{n}{m+n} < 1.$$
可见 \\(0\\) 和 \\(1\\) 分别是 \\(B\\) 的下界和上界。下面证明这两个界是紧的。

      1. 设 \\(\sup B = c\\)。假设 \\(c < 1\\)，则由 \\(\frac{n}{m+n} \le c\\) 得 \\(n \le c(m+n)\\)，即 \\((1-c) n \le cm\\)。

         固定 \\(m=1\\)，则 \\(n \le \frac{c}{1-c}\\)。这和 \\(n\\) 可取任意正整数（从而没有上界）矛盾。

      2. 设 \\(\inf B = c'\\)。假设 \\(c' > 0\\)，则由 \\(\frac{n}{m+n} \ge c'\\) 得 \\(n \ge c'(m+n)\\)。

         固定 \\(n = 1\\)，则 \\(m \le \frac{1}{c'} - 1\\)。这和 \\(m\\) 可取任意正整数（从而没有上界）矛盾。

      综上所述，\\(\inf B = 0\\), \\(\sup B = 1\\)，且均不属于 \\(B\\)。
   </details>

3. 设 \\(z\in\mathbb C\\) 满足 \\(|z| = 1\\) 且 \\(z \ne \pm1\\)。
   1) 证明 \\(\frac{z-1}{z+1}\\) 为纯虚数。
   2) 对任意复数 \\(z_1, z_2\in C\\)，利用共轭复数的运算性质，证明恒等式
$$|z_1 + z_2|^2 + |z_1 - z_2|^2 = 2(|z_1|^2 + |z_2|^2).$$
并以此计算：若 \\(|z_1| = |z_2| = 1\\) 且 \\(|z_1+z_2| = \sqrt3\\)，求 \\(|z_1 - z_2|\\)。

   <details> <summary>答案</summary>

   1) 设 \\(z = a + b\\,\mathrm i\\)，其中 \\(a, b\in\mathbb R\\)。那么
\\begin{align*}
\frac{z-1}{z+1} &= \frac{(a-1) + b\\,\mathrm i}{(a+1) + b\\,\mathrm i} \\\\
&= \frac{[(a-1) + b\\,\mathrm i][(a+1) - b\\,\mathrm i]}{[(a+1) + b\\,\mathrm i][(a+1) - b\\,\mathrm i]} \\\\
&= \frac{a^2 + b^2 - 1 + 2b\\,\mathrm i}{(a+1)^2 + b^2}.
\\end{align*}
又根据 \\(|z|=1\\) 得 \\(a^2 + b^2 = 1\\)，于是
$$\frac{z-1}{z+1} = \frac{b\\,\mathrm i}{a + 1}.$$
因为 \\(z \ne \pm1\\)，所以 \\(b \ne 0\\)，这就说明 \\(\frac{z-1}{z+1}\\) 的实部为零，而虚部非零，因此是纯虚数。

    2) &nbsp; \\begin{align*}
|z_1 + z_2|^2 &= (z_1 + z_2)(z_1^\* + z_2^\*) \\\\
&= z\_1 z\_1^\* + z\_1 z\_2^* + z\_2 z\_1^\* + z\_2 z\_2^\* \\\\
&= |z_1|^2 + z\_1 z\_2^\* + z\_2 z\_1^\* + |z_2|^2, \\\\
|z_1 - z_2|^2 &= |z_1|^2 - z\_1 z\_2^\* - z\_2 z\_1^\* + |z_2|^2. \\\\
\\end{align*}
两式相加即得要证的式子。

       计算：\\(|z_1-z_2|^2 = 2(|z_1|^2+|z_2|^2) - |z_1+z_2|^2 = 2\times(1+1) - 3 = 1\\)。因此 \\(|z_1-z_2|=1\\)。
    </details>

4. 判断下列区间是否满足 Cantor 定理（定理 1.9）的全部条件。若不满足，之处破坏了哪一条假设。求出 \\(\bigcap_{n=1}^\infty I_n\\)。
   1) \\(I_n = (0, \frac{1}{n}]\\),
   2) \\(I_n = [n, +\infty)\\),
   3) \\(I_n = [-\frac{1}{n}, 1+\frac{1}{n}]\\).

   <details> <summary>答案</summary>

   1. 不满足“有界闭区间”（是开区间）。\\(\bigcap_{n=1}^\infty I_n = \varnothing\\)，理由如下：

      设 \\(x\in \bigcap_{n=1}^\infty I_n\\)。由 \\(x\in I_1\\) 得 \\(x > 0\\)。那么，存在 \\(n > \frac{1}{x}\\) （由阿基米德性质保证），即 \\(x > \frac{1}{n}\\)，又由 \\(x\in I_n\\) 得 \\(0 < x \le \frac{1}{n}\\)。矛盾！故假设不成立，\\(\bigcap_{n=1}^\infty I_n = \varnothing\\)。

   2. 不满足“有界闭区间”（区间无界）。\\(\bigcap_{n=1}^\infty I_n = \varnothing\\)，理由如下：

      设 \\(x\in \bigcap_{n=1}^\infty I_n\\)。那么，存在 \\(n > x\\) （由阿基米德性质保证），又由 \\(x\in I_n\\) 得 \\(x \ge n\\)。矛盾！故假设不成立，\\(\bigcap_{n=1}^\infty I_n = \varnothing\\)。

   3. 满足全部条件。\\(\bigcap_{n=1}^\infty I_n = [0, 1]\\)，理由如下：

      设 \\(S = \bigcap_{n=1}^\infty I_n\\)。设 \\(0 \le x \le 1\\)，则易知
$$ -\frac{1}{n} < 0 \le x \le 1 < 1 + \frac{1}{n}\qquad n = 1, 2, 3, \ldots $$
可见对任意正整数 \\(n\\) 都有 \\(x\in I_n\\)。这说明 \\(S \supseteq [0, 1]\\)。另一方面，设 \\(y \in S\\)。
      * 假设 \\(y < 0\\)，则当 \\(n > -\frac{1}{y}\\) 时，\\(-\frac{1}{n} > y\\)，于是 \\(y\notin I_n\\)，和 \\(y\in S\\) 矛盾。
      * 假设 \\(y > 1\\)，则当 \\(n > \frac{1}{y-1}\\) 时，\\(1+\frac{1}{n} < y\\)，于是 \\(y\notin I_n\\)，和 \\(y\in S\\) 矛盾。

      以上矛盾说明 \\(0 \le y \le 1\\)，因此 \\(S \subseteq [0, 1]\\)。综上所述，\\(S = [0, 1]\\)。
   </details>

5. 构造一个开区间 \\((0, 1)\\) 到 \\(\mathbb R\\) 的显式双射。并以此说明 \\((0, 1)\\) 为不可数集。

   <details> <summary>答案</summary>

   定义 \\(f: (0, 1) \to \mathbb R\\) 为
$$ f(x) = \frac{2x-1}{1-(2x-1)^2}. $$
易知，在 \\((0, 1)\\) 内，\\(1-(2x-1)^2 \ne 0\\)，因此该定义是良好的。下面说明它是双射。

   设 \\(y\in\mathbb R\\)。如果 \\(y = 0\\)，那么由 \\(f(x) = 0\\) 得唯一解 \\(x=\frac{1}{2}\\)。以下假设 \\(y \ne 0\\)，我们将证明存在唯一的 \\(x\in (0, 1)\\) 使得 \\(f(x) = 0\\)。只须证明存在唯一的 \\(u\in (-1, 1)\\) 使 \\(\frac{u}{1-u^2} = y\\)（然后令 \\(u = 2x -1\\) 即可）。

   由 \\(\frac{u}{1-u^2} = y\\) 变形得
$$ yu^2 + u - y = 0,\qquad u\ne\pm 1. $$
这是关于 \\(u\\) 的二次方程，判别式 \\(\Delta = 1 + 4y^2 > 0\\)，故有两个相异实根
$$ u = \frac{-1 \pm \sqrt{1+4y^2}}{2y}.$$

   * 如果取“\\(-\\)”：注意到 \\(|u| = \frac{1 + \sqrt{1+4y^2}}{|2y|} > \frac{\sqrt{4y^2}}{|2y|} = 1\\)。这说明 \\(u\notin(-1,1)\\)。
   * 如果取“\\(+\\)”：注意到 \\(1+4y^2 < 1 + 4|y| + 4|y|^2 = (1 + |2y|)^2\\)，因此 \\(|u| = \frac{\sqrt{1+4y^2} - 1}{|2y|} < \frac{1+|2y|-1}{|2y|}= 1\\)。这说明 \\(u\in(-1,1)\\)。

   综上所述，对任意 \\(y\in\mathbb R\\)，存在唯一的 \\(u\in(-1, 1)\\)（即存在唯一的 \\(x\in(0, 1)\\)），使 \\(\frac{u}{1-u^2} = y\\)（即 \\(f(x) = y\\)）。因此 \\(f\\) 是双射。

   假设 \\((0, 1)\\) 可数，则存在双射 \\(\psi: \mathbb N\to (0, 1)\\)，进而 \\(f\circ \psi: \mathbb N\to R\\) 也是双射。这说明 \\(\mathbb R\\) 是可数集，但是实数集不可数是众所周知的。因此假设不成立，\\((0, 1)\\) 不可数。
   </details>

6. 设 \\(A\\), \\(B\\) 为非空有界实数集。
   1) 证明 \\(\sup(A\cup B) = \max \\{\sup A, \sup B\\}\\)；
   2) 若 \\(A \cap B \ne \varnothing\\)，证明 \\(\sup(A\cap B) \le \min\\{\sup A, \sup B\\}\\)。并举例说明严格不等号可以成立。

   <details> <summary>答案</summary>

   1) 设 \\(M = \max \\{\sup A, \sup B\\}\\)。设 \\(x \in A\cup B\\)。那么：
      * 如果 \\(x\in A\\)，则 \\(x\le\sup A\le M\\)；
      * 如果 \\(x\in B\\)，则 \\(x\le\sup B\le M\\)。

      总之，\\(x \le M\\)，故 \\(M\\) 是 \\(A\cup B\\) 的上界。

      另一方面，设 \\(M'\\) 是 \\(A\cup B\\) 的上界，则对任意 \\(a\in A \subseteq A\cup B\\)，\\(a\le M'\\)。从而 \\(M'\\) 是 \\(A\\) 的上界。根据上确界的定义，\\(M' \ge \sup A\\)。同理可证 \\(M'\ge \sup B\\)。因此 \\(M' \ge \max\\{\sup A, \sup B\\} = M\\)。

      综上所述，\\(M\\) 是 \\(A\cup B\\) 的上确界。证毕。

   2) 因为 \\(A\cap B\\) 是 \\(\mathbb R\\) 的非空子集，且 \\(A\\), \\(B\\) 有上界，所以 \\(A\cap B\\) 有上界，从而（根据确界定理）有上确界。设 \\(M = \sup(A\cap B)\\)。

      假设 \\(M > \sup A\\)，考虑 \\(M' = \frac{1}{2}(M + \sup A)\\)，易知 \\(\sup A < M' < M\\)。设 \\(x\in A\cap B\\)，由 \\(x\in A\\) 得 \\(x \le \sup A < M'\\)，故 \\(M'\\) 是 \\(A\cap B\\) 的上界，且比 \\(M\\) 更小，和 \\(M\\) 是 \\(A\cap B\\) 的上确界矛盾。因此假设不成立。同理可证 \\(s > \sup B\\) 不成立。

      综上所述，一定有 \\(s \le \sup A\\) 和 \\(s \le \sup B\\)，因此 \\(s \le \min\\{\sup A, \sup B\\}\\) 得证。

      举例：取 \\(A = \\{0, 1\\}\\), \\(B = [0, 1)\\)。此时 \\(\sup A = 1\\), \\(\sup B = 1\\)，但是 \\(\sup(A\cap B) = \sup\\{0\\} = 0 < 1\\)。
   </details>

7. 设 \\(A\\), \\(B\\) 为非空有界实数集，\\(c\in\mathbb R\\) 为常数。
   1) 证明 \\(\sup(A + B) = \sup A + \sup B\\)；
   2) 若 \\(c < 0\\)，证明 \\(\sup(cA) = c\inf A\\)（可直接用 \\(\inf(-A) = -\sup A\\)）。

   <details> <summary>答案</summary>

   1) 设 \\(x \in A+B\\)，则存在 \\(a\in A\\) 和 \\(b\in B\\) 满足 \\(x = a + b\\)。而 \\(a \le \sup A\\), \\(b\le \sup B\\)，因此 \\(x \le \sup A + \sup B\\)。

      设 \\(\varepsilon > 0\\)，由上确界性质知存在 \\(a'\in A\\) 和 \\(b'\in B\\) 使得 \\(a' > \sup A - \frac{\varepsilon}{2}\\) 且 \\(b' > \sup B - \frac{\varepsilon}{2}\\)。设 \\(x' = a' + b'\\)，则 \\(x'\in A+B\\) 且 \\(x' > \sup A + \sup B - \varepsilon\\)。

      综上所述，根据上确界的ε刻画可知，\\(\sup(A+B) = \sup A + \sup B\\)。

   2) \\(c < 0\\) 时 \\(-c > 0\\)。由已知条件可知，\\(-\sup (cA) = \inf(-cA) = \inf ((-c)A)\\)。因此，只须证明 \\(\inf((-c)A) = (-c)\inf A\\)。

      设 \\(x\in A\\)，则 \\(x \ge \inf A\\)。于是 \\((-c) x \ge (-c)\inf A\\)。

      设 \\(\varepsilon > 0\\)，由上确界性质知存在 \\(x'\in A\\) 使得 \\(x' < \inf A + \frac{\varepsilon}{-c}\\)。那么 \\((-c) x' < (-c) \inf A + \varepsilon\\)。

      综上所述，根据下确界的ε刻画可知，\\(\inf((-c)A) = (-c)\inf A\\)。证毕。
   </details>

8. 设 \\(A\\), \\(B\\) 为非空有界实数集。定义
$$ AB = \\{ ab \mid a\in A,\\, b\in B \\}. $$
   1) 若 \\(A,B \subseteq (0, +\infty)\\)，证明 \\(\sup(AB) = \sup A\cdot\sup B\\)；
   2) 若去掉“正数集”限制，等式 \\(\sup(AB) = \sup A\cdot\sup B\\) 是否依然成立？请举反例说明。

   <details> <summary>答案</summary>

   1) 设 \\(x\in AB\\)，则存在 \\(x\in A\\), \\(b\in B\\) 使得 \\(x = ab\\)。又因为 \\(0<a\le \sup A\\), \\(0<b\le \sup B\\)，所以 \\(ab \le \sup A\sup B\\)。

      设 \\(\varepsilon > 0\\)，并设
$$ \varepsilon' = \min\left\\{\frac{\varepsilon}{\sup A + \sup B}, \sup A, \sup B\right\\}, $$
那么存在 \\(a'\in A\\) 使得 \\(a' > \sup A - \varepsilon' \ge 0\\)，且存在 \\(b'\in B\\) 使得 \\(b' > \sup B - \varepsilon' \ge 0\\)。于是 \\(a'b' \in AB\\) 且
\begin{align*}
a'b' &> (\sup A - \varepsilon')(\sup B - \varepsilon') \\\\
     &= \sup A \sup B - \underbrace{\varepsilon' (\sup A + \sup B)}\_{=\varepsilon} + \underbrace{(\varepsilon')^2}\_{\ge 0} \\\\
     &\ge \sup A \sup B - \varepsilon.
\end{align*}

      综上所述，根据上确界的ε刻画可知 \\(\sup (AB) = \sup A \sup B\\)。

   2) 不成立。举反例如下：取 \\(A = B = [-1, 0]\\)，则 \\(\sup A \sup B = 0\\)，但是 \\(1\in AB\\) 且 \\(1 > 0\\)。
   </details>

9. 设 \\(\\{[a_n, b_n]\\}\_n\\) 为一列闭区间，且满足 \\([a_{n+1}, b_{n+1}] \subseteq [a_n, b_n]\\)。
   1) 如果已知对任意 \\(n\in\mathbb N\\)，\\(b_n - a_n \le \frac{1}{n+1}\\)，证明存在唯一实数 \\(c\\)，使得 \\(\bigcap_{n=0}^\infty [a_n, b_n] = \\{c\\}\\)；
   2) 记 \\(\alpha = \sup\\{a_n \mid n\in\mathbb N\\}\\), \\(\beta = \inf\\{b_n \mid n\in\mathbb N\\}\\)。证明 \\(\alpha \le \beta\\) 且 \\(\bigcap_{n=0}^\infty [a_n, b_n] = [\alpha, \beta]\\)。

10. 考虑用三等分法和区间套定理证明 \\([0, 1]\\) 不可属性的过程（参见 1.2.4 节），并思考以下问题：
    1) 构造区间套 \\([a_n, b_n]\\) 避开枚举序列 \\(\\{x_n\\}\\) 时，为什么必须将区间三等分而不是二等分？（请指出若采用二等分法，在哪个具体情形下无法保证 \\(x_n\notin [a_n, b_n]\\)。
    2) 既然 \\([0, 1]\\) 不可数，那么所有端点为有理数的闭区间组成的集合
$$ \mathscr I = \\{[p, q] \mid p, q\in \mathbb Q,\\, p < q\\} $$
是可数的还是不可数的？请给出证明。

11. 已知 \\(|z_1 + z_2| \le |z_1| + |z_2|\\)，请利用此不等式推导左侧不等式
$$\bigl| |z_1| - |z_2| \bigr| \leq |z_1 + z_2|,$$
并求出等号成立的条件。（式中变量均为复数。）

12. 证明复数域 \\(\mathbb C\\) 上无法定义与通常加法和乘法相容的全序关系（即 \\(\mathbb C\\)不能成为有序域）。

13. 按照下面的方法，仅将 Cantor 定理和阿基米德性质作为已知前提（暂不使用强分离原理和确界定理），证明强分离原理成立：

    设 \\(A\\), \\(B\\) 为 \\(\mathbb R\\) 的两个非空子集，满足对任意 \\(a\in A\\), \\(b\in B\\) 都有 \\(a\le b\\)。利用二分法构造区间套 \\(\\{[a_n, b_n]\\}\_n\\)，严格证明存在 \\(c\in\mathbb R\\)，使得对一切 \\(a\in A\\), \\(b\in B\\) 都有 \\(a\le c\le b\\)。

    <details><summary>提示</summary>

    取 \\(a_0 \in A\\), \\(b_0 \in B\\)。若 \\(a_0 = b_0\\) 则结论显然；若 \\(a_0 < b_0\\)，考察中点 \\(m_0 = \frac{a_0 + b_0}{2}\\) 与集合 \\(A\\), \\(B\\) 的位置关系，并以此为依据决定下一步选取左半区间 \\([a_0, m_0]\\) 还是右半区间 \\([m_0, b_0]\\)，并利用数学归纳法说明对每个自然数 \\(n\\) 均满足：对任意 \\(a\in A\\) 和任意 \\(b\in B\\)，\\(a\le b_n\\) 且 \\(a_n\le b\\)。
    </details>
