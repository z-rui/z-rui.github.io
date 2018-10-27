---
date: 2018-10-27T14:43:52-04:00
title: 古代科技： desk calculator
---

`dc` 是 UNIX 系统上的计算器 (desk calculator) 程序。
它采用逆波兰表达式 (RPN) 语法， 和平常使用的算术表达式稍有不同。
如： `1 2 + 3 *` 表示 (1+2)*3。
RPN 的好处是不用输入括号， 且熟练掌握后效率很高；
缺点是难以上手。

`dc` 的计算能力不仅限于此。
实际上， 它是一个图灵完备的语言， 并且据说是第一个被移植到 UNIX 系统上的语言。
由于可以定义并调用宏， 它具备了分支和循环的能力， 因此可以写出各种各样的程序。

## 水仙花数

水仙花数指的是三位数 \\(\overline{abc}\\) 满足
\\(a^3+b^3+c^3=\overline{abc}\\)。
下面的程序输出所有的水仙花数。
```
[lnp]sP[la3^lb3^+lc3^+la10*lb+10*lc+dsn=PLc1+dSc10>C]sC
[0sclCxLb1+dSb10>B]sB[0sblBxLa1+dSa10>A]dsA1sax
```

## 斐波那契数列

斐波那契数列 \\(F\_n\\) 满足递归方程 \\(F\_n = F\_{n-1} = F\_{n-2}\\)，
且 \\(F_1 = F_2 = 1\\)。
下面的程序输入一个整数 \\(N\\)，输出 \\(F_1\hbox{---}F_n\\)。
```
?0Sa1Sb[1-Lalbp+LbSaSbd0<F]sFd0<F
```
因为 `dc` 可以处理任意精度的数字， 该程序可以输出很多项而不会发生溢出。

## 欧几里得算法

欧几里得算法求两数的最大公约数
\\(\def\gcd{\mathop{\rm gcd}}\gcd(a, b)\\)， 基于以下等式：
\\[\eqalign{
\gcd(a, 0) &= a\cr
\gcd(a, b) &= \gcd(b, a \bmod b)\qquad (b\ne0)\cr
}\\]
```
[a=]P?Sa
[b=]P?Sb
[lbLaLb%SbSalb0<G]sG
lb0<G[gcd(a,b)=]PLap
```
该程序在运行时会输出提示， 是一个交互式程序。


## 筛法求素数

下面的程序输入一个整数 N，输出 2---N 之间的所有素数。
```
?1+sN
[d;@0=y1+dlN>x]sx
[pdd*dlN>zs.]sy
[d1r:@S.dL.+dlN>z]sz
2dlN>x
```
GNU dc 的数组采用了较为低效的实现， 因此当 N 较大时运行会很慢。

## N 皇后求解器

在 N x N 的棋盘上放置 N 个相互不攻击的皇后
（参考[国际象棋规则](https://zh.wikipedia.org/wiki/%E5%9C%8B%E9%9A%9B%E8%B1%A1%E6%A3%8B#%E8%A6%8F%E5%89%87)）。
下面的程序输入一个整数 N， 输出一个可行解。

```
? sN

[q]sq
[[ ]P1-d0<p]sp
[d;b d0<p[Q
]Ps. 1+dlN>P]sP
[0dlN>PlN1+Q]s#     # Found a solution

[lilN=#0dlN>rs.]sR  # Recursive search
[l@x1+dlN>r]sr
[d;c1=q dli+;a1=q dli-lN+;d1=q
dli:b dli+1r:a dli-lN+1r:d d1r:c
Li1+Si lRx Li1-Si
d0r:c dli-lN+0r:d dli+0r:a]s@

0silRx
```

# 总结

如果真的用 `dc` 写程序， 它的古怪语法导致了几乎无法理解的代码，
并且编写的难度也很大 （必须维护栈的状态， 没有显式的控制逻辑）。
POSIX 并没有将其标准化， 而是选择将历史上作为 `dc` 前端的 `bc`
（使用算术表达式和类似 C 的语法）标准化。
现在 `dc` 除了娱乐用途以外， 大概只有作为 shell 脚本中嵌入的神秘命令了吧。
