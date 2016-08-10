---
date: 2016-08-11T01:47:00
title: 幻方生成器
---

幻方(magic square)是休闲数学(recreational mathematics)所感兴趣的话题。相传在古代曾有神龟浮出水面，背上有图案，称为《洛书》：

> 载九履一，左三右七，二四为肩，六八为足，五居中腹。

这是一个3阶幻方，如下所示。它的特点是每行、每列、每条对角线上的数字之和都相等。

    4 9 2
    3 5 7
    8 1 6

<!--more-->

奇数阶的幻方有一种很容易的构造方法，称为[Siamese方法](https://en.wikipedia.org/wiki/Siamese_method)。用模拟的办法，很容易将它写成程序，以便自动生成幻方。

偶数阶的幻方的构造比较困难，我之前并不会。下面是一个4阶幻方：

     1 15 14  4
    12  6  7  9
     8 10 11  5
    13  3  2 16

查了[维基百科](https://en.wikipedia.org/wiki/Magic_square#A_method_of_constructing_a_magic_square_of_doubly_even_order)知道，4n阶幻方有相对容易的构造方法。编写成程序时，为了简单，采用一种固定的策略：把行和列平均分成4等分，则整个幻方分为16个子块，如下：

    AbcD
    eFGh
    iJKl
    MnoP

按照行优先、从左到右、从上到下的顺序填数。如果当前位置位于大写字母所在的子块，则填写这个格子的正数序数，否则填写倒数序数。这样构造出来的幻方就满足各行、列、对角线之和相等的条件。

最困难的是4n+2阶幻方，能查到的构造方法有[LUX法](https://en.wikipedia.org/wiki/LUX_method_for_magic_squares)和[Strachey法](https://en.wikipedia.org/wiki/Strachey_method_for_magic_squares)。这些方法乍看毫无道理可言，所以能够想到而且还能证明正确性在我看来的确是一件了不起的事情。我采用了Strachey法构造这种类型的幻方。

至此，所有阶数的幻方的构造方法都已经具备。作为唯一的特例，2阶幻方是不存在的。

我用Go语言编写了幻方生成器。使用Go的好处之一是安装非常方便：

<pre><code>go get <a href="https://github.com/z-rui/msquare">github.com/z-rui/msquare</a></code></pre>

简单的一条命令，就可以将相应程序安装到`$GOPATH/bin`目录中去。一切自动，无需下载、解压、`./configure && make`，不用纠结依赖问题。

用`msquare`生成一个幻方看看吧！

```
$ msquare 6
35  1  6 26 19 24
 3 32  7 21 23 25
31  9  2 22 27 20
 8 28 33 17 10 15
30  5 34 12 14 16
 4 36 29 13 18 11
```
