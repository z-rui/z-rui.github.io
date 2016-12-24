---
title: 数学填空题
date: 2016-06-09T22:00:00+08:00
---

<script>
mathjax
</script>

基本上只有到了一年一度的高考日子才有兴趣研究那么几道数学题。我已脱离高考三年，能力也是退化了许多。户枢不蠹，流水不腐。重温几道数学题，仿佛可以将已锈蚀的脑子润滑起来。

问题：已知锐角\\(\triangle ABC\\)满足\\(\sin A = 2\sin B \sin C\\)。则\\(\tan A \tan B \tan C\\)的最小值为______。

解答：因为
\\[ \tan A \tan B \tan C = \frac{\sin A \sin B \sin C}{\cos A \cos B \cos C}, \\]
代入
\\[\left\lbrace\begin{aligned}
\sin A &= 2\sin B\sin C, \\\\ 
\cos A &= -\cos(B + C) = \sin B \sin C - \cos B \cos C
\end{aligned}\right.\\]
得 <!--more-->
\\[\begin{align}
\tan A \tan B \tan C &= \frac{2\sin^2 B \sin^2 C}{(\sin B \sin C - \cos B \cos C)\cos B \cos C} \\\\ 
&= \frac{2\tan^2 B \tan^2 C}{\tan B \tan C - 1} \\\\ 
&= f(\tan B \tan C)
\end{align}\\]
其中\\(f(x) = \frac{2x^2}{x-1}\\)。只要能够先确定\\(\tan B \tan C\\)的取值范围，就能得到\\(f(\tan B \tan C)\\)的取值范围（我们只需求最小值）。

为了确定\\(\tan B \tan C\\)的取值范围，考虑到这是一个三角形，不妨采用以边代角的策略。因此作出\\(\triangle ABC\\)的图像。

{{<figure src="/media/math-1.svg">}}

过A作BC的垂线，垂足为D。并设\\(AD=h\\), \\(BD=a_1\\), \\(DC=a_2\\)。从图中可以直接读出\\(B\\)的\\(C\\)的三角函数。

根据题中条件，得
\\[\sin A = 2\sin B \sin C = 2\cdot\frac{h}{c}\cdot\frac{h}{b} = \frac{2h^2}{bc}.\\]

另一方面，关于三角形的面积\\(S\\)有如下等式成立：
\\[\frac{1}{2}bc\sin A = S = \frac{1}{2}ah.\\]
因此\\(\sin A = \frac{ah}{bc}\\)。

由此可知\\(\frac{ah}{bc} = \frac{2h^2}{bc}\\)，解得\\(h=\frac{a}{2}\\)，是一个定值，这是一个重要的发现。

注意到
\\[\tan B \tan C = \frac{h}{a_1}\cdot\frac{h}{a_2} = \frac{a^2}{4a_1a_2}.\\]

根据基本不等式，\\(a_1a_2 \leq \left(\frac{a_1+a_2}{2}\right)^2 = \frac{a^2}{4}\\)，当且仅当\\(a_1 = a_2 = \frac{a}{2}\\)时，取“=”。所以\\(a_1a_2\\)的取值范围是\\(\left(0, \frac{a^2}{4}\right)\\)[^1]，于是\\(\tan B \tan C\\)的取值范围是\\((1, +\infty)\\)。

对函数\\(f(x) = \frac{2x^2}{x-1}, x \in (1, +\infty)\\)求导，得
\\[f'(x) = \frac{2x(2-x)}{(x-1)^2}.\\]
由此可知，在\\((1,2)\\), \\(f'(x) < 0\\)，函数单调递减；在\\((2, +\infty)\\), \\(f'(x) > 0\\)，函数单调递增。所以最小值为\\(f(2) = 8\\)。

[^1]: 这里有一个小细节，实际上等号不成立，因为当\\(a_1 = a_2 = \frac{a}{2}\\)时，\\(A\\)刚好为直角，不满足锐角三角形的前提。这意味着最大值取不到，但可以任意接近。所以这里的取值范围是开区间而不是左开右闭区间。
