---
date: 2016-04-08T20:39:00+08:00
title: 运算法求解动态电路
---

如图所示的电路，在\\(t=0\\)时刻，有\\(i\_{L_r}(0)=I\_0\\), \\(u\_{C_r}(0)=0\\)。试求\\(t>0\\)时的\\(i\_{L_r}(t)\\)和\\(u\_{C_r}(t)\\)。

{{<figure src="/media/dynamic-circuit-1.svg">}}

这电路含有动态元件，可以使用运算法来求解。

<!--more-->

{{<figure src="/media/dynamic-circuit-2.svg">}}

如图，作出运算电路，在电感和电容之间的节点处，由基尔霍夫电流定律得
\\[{U\_i/s - u\_{C_r}(s) \over sL_r} + {i\_{L_r}(0) \over s} - {u\_{C_r}(s) \over 1/(sC_r)} - {I_0 \over s} = 0.\\]

解得\\[u\_{C_r}(s) = {U_i\over s}{1\over1+L_rC_rs^2} = U_i{\omega_r^2\over s(s^2+\omega_r^2)},\\]
其中\\(\omega_r={1\over\sqrt{L_rC_r}}\\)。

电感电流是运算电路中的电感电流加上附带的电流源的电流，即
\\[\eqalign{
i\_{L_r}(s) &= {U_i/s - u\_{C_r}(s) \over sL_r} + {i\_{L_r}(0) \over s} \cr
&= {U_iC_r\over1+L_rC_rs^2} + {i\_{L_r}(0) \over s} \cr
&= {U_i\over Z}{\omega_r\over s^2+\omega_r^2} + {i\_{L_r}(0) \over s},
}\\]
其中\\(Z=\sqrt{L_r\over C_r}\\)。

根据拉普拉斯变换表，
\\[\eqalign{
{\cal L}\lbrace 1\rbrace &= {1\over s}, \cr
{\cal L}\lbrace \sin{\omega t}\rbrace &= {\omega \over s^2 + \omega^2}, \cr
{\cal L}\lbrace 1-\cos{\omega t}\rbrace &= {\omega^2 \over s(s^2 + \omega^2)}. \cr
}\\]

可知时域的解为
\\[\eqalign{
i\_{L_r}(t) &= {U_i\over Z}\sin{\omega_rt} + I_0, \cr
u\_{C_r}(t) &= U_i(1-\cos{\omega_rt}).
}\\]
