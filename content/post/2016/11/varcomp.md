---
date: 2016-11-01T00:46:00+08:00
title: 变压器二次侧的无功补偿
---

为了提高功率因数，在变压器二次侧进行无功补偿。现在需要使得一次侧的功率因数不低于0.9，那么二次侧的功率因数肯定需要补偿到0.9以上，补偿到多少合适呢？

一个经验值是，二次侧功率因数补偿到0.92，可以使一次侧功率因数约等于0.9。这个数值是怎么得到的呢？

设二次侧的视在功率是\\(S_2\\)，功率因数角是\\(\varphi_2\\)。则二次侧的有功功率
\\[P_2=S_2\cos\varphi_2,\\]
无功功率
\\[Q_2=S_2\sin\varphi_2.\\]

变压器会带来额外的损耗，而且以无功居多，这也是导致一次侧功率因数低于二次侧的原因。为了定量地分析，需要一些额外的参数。典型的变压器消耗的有功功率
\\[\Delta P_T\approx 0.015S_2,\\]
无功功率
\\[\Delta Q_T\approx 0.06S_2.\\]
由此可知一次侧的有功功率
\\[P_1=P_2+\Delta P_T=S_2(\cos\varphi_2 + 0.015),\tag{1}\\]
无功功率
\\[Q_1=Q_2+\Delta Q_T=S_2(\sin\varphi_2 + 0.06).\tag{2}\\]

<!--more-->

\\(\frac{(2)}{(1)}\\)，得
\\[\frac{\sin\varphi_2+0.06}{\cos\varphi_2+0.015} = \frac{Q_1}{P_1} = \tan\varphi_1=\frac{\sin\varphi_1}{\cos\varphi_1}.\tag{3}\\]
其中\\(\varphi_1\\)是一次侧的功率因数角。将\\((3)\\)式整理得到
\\[\sin\varphi_2\cos\varphi_1-\cos\varphi_2\sin\varphi_1=0.015\sin\varphi_1-0.06\cos\varphi_1.\\]

左边运用三角恒等式，得
\begin{align}
\sin(\varphi_2-\varphi_1) &= 0.015\sin\varphi_1-0.06\cos\varphi_1, \\\\\\
\varphi_2 &= \varphi_1 + \arcsin(0.015\sin\varphi_1-0.06\cos\varphi_1).
\end{align}

令一次侧的功率因数\\(\cos\varphi_1 = 0.9\\)，即\\(\varphi_1 = \arccos 0.9\\)，则
\begin{align}
\sin\varphi_1 &= \sqrt{1-0.9^2} \approx 0.436, \\\\\\
\cos\varphi_2 &= \cos\big[\arccos 0.9 + \arcsin(0.015\times 0.436 - 0.06\times 0.9)\big] \\\\\\
&= \cos\big[\arccos 0.9 + \arcsin(-0.04746)\big] \\\\\\
&= 0.9\times\sqrt{1-(-0.04746)^2} - \sqrt{1-0.9^2}\times(-0.04746) \\\\\\
&\approx 0.92
\end{align}

这就是经验值0.92的来源。
