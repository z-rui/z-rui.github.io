---
date: 2016-04-08T20:39:00+08:00
title: 运算法求解动态电路
---

如图所示的电路，在\\(t=0\\)时刻，有\\(i\_{Lr}(0)=I\_0\\), \\(u\_{Cr}(0)=0\\)。试求\\(t>0\\)时的\\(i\_{Lr}(t)\\)和\\(u\_{Cr}(t)\\)。

{{<figure src="/media/dynamic-circuit-1.svg">}}

这电路含有动态元件，可以使用运算法来求解。

<!--more-->

{{<figure src="/media/dynamic-circuit-2.svg">}}

如图，作出运算电路，在电感和电容之间的节点处，由基尔霍夫电流定律得
\\[{U\_i/s - u\_{Cr}(s) \over sL\_{r}} + {i\_{Lr}(0) \over s} - {u\_{Cr}(s) \over 1/(sC\_{r})} - {I\_0 \over s} = 0.\\]

解得\\[u\_{Cr}(s) = {U\_i\over s}{1\over1+L\_rC\_rs^2} = U\_i{\omega\_r^2\over s(s^2+\omega\_r^2)},\\]
其中\\(\omega\_r={1\over\sqrt{L\_rC\_r}}\\)。

电感电流是运算电路中的电感电流加上附带的电流源的电流，即
\\[i\_{Lr}(s) = {U\_i/s - u\_{Cr}(s) \over sL\_{r}} + {i\_{Lr}(0) \over s} = {U\_iC\_r\over1+L\_rC\_rs^2} + {i\_{Lr}(0) \over s} = {U\_i\over Z}{\omega\_r\over s^2+\omega\_r^2} + {i\_{Lr}(0) \over s},\\]
其中\\(Z=\sqrt{L\_r\over C\_r}\\)。

根据拉普拉斯变换表，
\\[\eqalign{
{\cal L}\\{1\\} &= {1\over s}, \cr
{\cal L}\\{\sin{\omega t}\\} &= {\omega \over s^2 + \omega^2}, \cr
{\cal L}\\{1-\cos{\omega t}\\} &= {\omega^2 \over s(s^2 + \omega^2)}. \cr
}\\]

可知时域的解为
\\[\eqalign{
i\_{Lr}(t) &= {U\_i\over Z}\sin{\omega\_rt} + I\_0, \cr
u\_{Cr}(t) &= U\_i(1-\cos{\omega\_rt}).
}\\]
