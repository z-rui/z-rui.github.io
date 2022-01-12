---
title: 抽象代数习题(9) -- 同构
date: 2022-01-10T19:53:07-08:00
---

《抽象代数》第九章讲了群的同构 (isomorphism)。群 (G,·) 和 (H,\*) **同构**则它们本质上是一样的，只有表面上的区别（元素名）。可以找到一个**双射** f: G→H, f(x·y) = f(x)\*f(y) 将它们对应起来。

<!--more-->

---

__D.1--D.3__ 将下列群按同构分组：

1. ℤ₄, ℤ₂×ℤ₂, P₂, V。其中 P₂ 是二元集合的幂集和对称差运算构成的集合。V是复数四元乘法群 {1, i, -1, -i}。
2. S₃, ℤ₆, ℤ₃×ℤ₂, ℤ₇\*
3. ℤ₈, P₃, ℤ₂×ℤ₂×ℤ₂, D₄

说明：
- P₂ 是 2 元素集合的所有子集和对称差运算构成的群。
- V = {1, i, -1, -i} 乘法群。
- ℤ₇\* 是整数 1--6 和模 7 乘法构成的群。
- D₄ 是对应于正方形的二面体群。

**解** 对每一组群，同构的群仅给出它们的同构映射，不同构的群将给出理由。

1. - ℤ₄ ≅ V
      - f(x) = i<sup>x</sup>
   - ℤ₂×ℤ₂ ≅ P₂
      - 设二元集合为 {a₁, a₂}。 取 f(x₁,x₂) = {S : aᵢ ∈ S 当且仅当 xᵢ = 1}。
   - ℤ₄ ≇ P₂
      - 满足 x²=e 的元素在前者有 2 个 （0 和 2），而在后者有 4 个。
2. - ℤ₃×ℤ₂ ≅ ℤ₆ ≅ ℤ₇\*
      - f: ℤ₃×ℤ₂→ℤ₆, f(x,y) = 2x+3y mod 6
      - g: ℤ₆→ℤ₇\*, g(x) = 3<sup>x</sup> mod 7 （注：3 是模 7 的原根）
   - S₃ ≇ ℤ₆
      - 满足 x²=e 的元素在前者中有 4 个（恒等和 3 个对换），而在后者中只有 2 个（0 和 3）。
3. - ℤ₂×ℤ₂×ℤ₂ ≅ P₃
      - 仿照 ℤ₂×ℤ₂ ≅ P₂ 的证明即可。
   - 其余均不同构，因为：
      - 满足 x²=e 的元素在 ℤ₈ 中有 2 个（0 和 4），在  D₄ 有 6 个 （恒等、旋转 180 度、关于 4 个对称轴的翻转），在 P₃ 中有 8 个。

---

__E.5__ 证明 \\(\mathbb Z\\) 和 \\(\mathbb Q\\) 不同构。

**证明** 用反证法。假设 \\(\mathbb Z \cong \mathbb Q\\)，则存在双射 \\(f: \mathbb Z \to \mathbb Q,\\, f(x+y) = f(x)+f(y)\\)。
当 \\(x>0\\) 时，
\\[f(x) = f(\underbrace{1+\cdots+1}\_{x}) = \underbrace{f(1)+\cdots+f(1)}\_{x} = xf(1)\\]
又有 \\(f(0)=0\\), \\(f(-x) = -f(x)\\)。故 \\(f(x) = xf(1)\\) 对任意 \\(x\in\mathbb Z\\) 恒成立。

设 \\(f(1)=\frac{p}{q}\\) 为既约分数 \\(\\)。显然 \\(\frac{1}{2}\in\mathbb Q\\)，根据 \\(f\\) 是满射，存在 \\(x\in\mathbb Z\\) 满足 \\(f(x) = \frac{1}{2}\\)，得整数方程
\\[2px = q\\]
这表明 \\(q\\) 是 \\(p\\) 的偶数倍，和 \\(p\over q\\) 是既约分数矛盾！

所以假设不能成立，这样的双射 \\(f\\) 是不存在的。所以 \\(\mathbb Z \ncong \mathbb Q\\)。

**注** 直观地说，\\(\mathbb Z=\langle 1\rangle\\)，全部元素可以由一个元素生成。而 \\(\mathbb Q\\) 没有这样的性质。

---

__E.6__ 我们已经证明了 \\(\mathbb R \cong \mathbb R^{\mathrm{pos}}\\)。但是，可以证明 \\(\mathbb Q \ncong \mathbb Q^{\mathrm{pos}}\\)。请证明。

说明：\\(\mathbb Q^{\mathrm{pos}}\\) 是正有理数乘法群。

**引理** 设正的既约分数 \\(p/q \ne 1\\)，则存在正整数 \\(k\\) 使得 \\((p/q)^{1/k}\\) 为无理数。 证明如下：

根据唯一分解定理，
\\[
p = \prod_{i=1}^N p_i^{a_i},\quad
q = \prod_{i=1}^M q_i^{b_i}
\\]
其中 \\(p_i\\)、\\(q_i\\) 为质数，其余为自然数。 取 \\(k\\) 为大于
\\(a_1,\ldots a_N, b_1,\ldots b_M\\) 的正整数，并假设 \\((p/q)^{1/k}\\) 是既约分数 \\(r\over s\\)，则
\\[\frac{p}{q} = \frac{r^k}{s^k}\\]
根据既约分数的唯一性，有 \\(p=r^k\\), \\(q=s^k\\)。因为 \\(p\\)、\\(q\\) 均不含 \\(k\\) 次素因子，所以 \\(r=s=1\\)，得 \\(\frac{p}{q}=1\\)，不符合前提。因此假设不成立。所以 \\((p/q)^{1/k}\\) 是无理数。

**证明** 用反证法。假设 \\(\mathbb Q \cong \mathbb Q^{\mathrm{pos}}\\)，则存在双射 \\(f: \mathbb Q \to \mathbb Q^{\mathrm{pos}},\\, f(x+y) = f(x)f(y)\\)。

设 \\(f(1) = \frac{p}{q}\\) 是正的既约分数。因为 \\(f\\) 是单射，所以 \\(f(1) \ne f(0) = 1\\)。根据引理知存在 \\(1/k \in \mathbb Q^{\mathrm{pos}} \subset \mathbb Q\\) 使得 \\((p/q)^{1/k} \notin \mathbb Q\\)。

另一方面，
\\[
\frac{p}{q} =
f(1) = f\bigl(\underbrace{\tfrac{1}{k}+\cdots+\tfrac{1}{k}}\_k\bigr)
     = \underbrace{f\left(\tfrac{1}{k}\right)\cdots f\left(\tfrac{1}{k}\right)}\_k
     = \left[f\left(\tfrac{1}{k}\right)\right]^k
\\]
得 \\((p/q)^{1/k} = f(1/k) \in \mathbb Q^{\mathrm{pos}}\\)，产生了矛盾。
故假设不成立。不存在这样的双射 \\(f\\)。所以 \\(\mathbb Q \ncong \mathbb Q^{\mathrm{pos}}\\)。

**注**

1. 本题和前一题的相似之处都是利用同构的性质推出 \\(f(1)\\) 和其他一般量的联系，然后假设 \\(f(1)\\) 的形式并构造可推出矛盾的例子。
2. 函数方程 \\(f(x+y)=f(x)f(y)\\) 的解是指数函数，这在有理数范围内是成立的。本题实质是证明有理数集对指数运算不封闭。实数集不存在这个问题，从而可以证明 \\(\\mathbb R \cong \mathbb R^{\mathrm{pos}}\\)。

---

__H.3__ 设 G 是任意群。证明 G 是 Abel 群当且仅当 f(x) = x⁻¹ 是 G 到 G 的同构。

**证明** 设映射 f : G→G, f(x) = x⁻¹。显然 f 是双射。
设 x,y ∈ G，则 f(xy) = (xy)⁻¹ = y⁻¹x⁻¹ = f(y)f(x)。

因此，G 是 Abel 群 ⟺ f(x)f(y) = f(y)f(x) ⟺ f(xy)=f(x)f(y) ⟺ f 是 G 到 G 的同构。

---

__I.3__ 设 G 是任意群，a 是群中任意元素。证明 f(x) = axa⁻¹ 是 G 的自同构 (automorphism)。

**证明** 不难发现 f⁻¹(x) = a⁻¹xa，所以 f 是双射。

设 x,y ∈ G，则 f(xy) = a(xy)a⁻¹ = (axa⁻¹)(aya⁻¹) = f(x)f(y)。 所以 f 是 G 的自同构。

---

__I.4__ 因为 \\(G\\) 的自同构是 \\(G\\) 到 \\(G\\) 的双射，所以它是 \\(G\\) 的一个*置换*。
求证：所有 \\(G\\) 的自同构构成的集合 \\(\DeclareMathOperator{\Aut}{Aut}\Aut(G)\\) 是置换群 \\(S_G\\) 的子群。（注意：运算是映射的复合。）

**证明** 显然 \\(\epsilon \in \Aut(G) \subseteq S_G\\)。

考虑对复合运算的封闭性：设 \\(f,g\in\Aut(G)\\)，则有
\\[\forall x,y\in G\quad f(xy)=f(x)f(y),\\; g(xy)=g(x)g(y)\\]
因此
\\[(f\circ g)(xy) = f(g(xy)) = f(g(x)g(y)) = f(g(x))f(g(y)) = (f\circ g)(x)(f\circ g)(y)\\]
这说明 \\(f\circ g \in \Aut(G)\\)，所以复合运算在 \\(\Aut(G)\\) 内封闭。

考虑取逆的封闭性：设 \\(h\in\Aut(G)\\)，显然 \\(h^{-1}\in S_G\\) 但我们要证 \\(h^{-1}\in\Aut(G)\\)。
设 \\(x,y\in G\\)，则
\\[h^{-1}(xy) = h^{-1}\bigl\\{h\bigl[h^{-1}(x)\bigr]h\bigl[h^{-1}(y)\bigr]\bigr\\}
              = h^{-1}\bigl\\{h\bigl[h^{-1}(x)h^{-1}(y)\bigr]\bigr\\}
	      = h^{-1}(x)h^{-1}(y)\\]
这说明 \\(h^{-1} \in \Aut(G)\\) ，所以 \\(\Aut(G)\\) 对取逆封闭。

综上所述，\\(\Aut(G) \le S_G\\)。
