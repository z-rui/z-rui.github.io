---
date: 2017-11-18T22:52:00-05:00
title: 二叉树的作图
---

画图是观察数据结构的好办法。 二叉树是一种常见的数据结构， 我在调试的时候往往通过先序遍历把整个树的结构打印出来。 然后拿一张草稿纸， 根据打印出的信息， 用笔画出整个树的样子， 然后判断该进行什么操作 （或者我已经执行的操作是否正确）。

为了偷懒， 我在想有没有办法能够让计算机自动生成二叉树的图像。
我发现使用 MetaPost 可以方便地实现。

{{<figure src="/media/rbgraph-1.svg" width="100%;" caption="用MetaPost画出的一棵二叉树">}}

<!--more-->
用 MetaPost 作图时， 无需直接生成每个结点的作图指令， 而只需告诉 MetaPost 各个结点之间的相互关系。 首先， 按照从左到右的顺序将各结点编号。 上图中 11, 15, 16, ... 结点分别编号为 1, 2, 3, ... 然后告诉 MetaPost 各个结点的关键信息。
例如， 我用 `V` 数组保存结点的值， `C` 数组保存结点的颜色， 则我只需写
```
V1:="11";
C1:=red;
V2:="15";
C2:=black;
%...
```
此外我还必须描述结点之间的层次关系。 而一般的二叉树存储方式不同， 这里只需描述每个结点的父结点即可。 我用 `P` 数组保存这个信息。
```
P1:=2;
%...
```
图中的树共有 26 个结点。 当我描述完了所有结点后， 就调用我另外编写的宏
```
drawtree(26);
```
来画出整个树。
```
input boxes;
def drawtree(expr n) =
	z1=(0,0);
	for i = 2 upto n:
		x[i] - x[i-1] = hoffset;
	endfor
	for i = 1 upto n:
		if known P[i]:
			y[P[i]] - y[i] = voffset;
		fi
	endfor

	for i = 1 upto n:
		circleit.X[i](V[i]);
		X[i].c = z[i];
		fixpos(X[i]);
	endfor
	for i = 1 upto n:
		if known P[i]:
			drawarrow z[P[i]]--z[i]
				cutbefore bpath.X[P[i]]
				cutafter bpath.X[i]
				withcolor C[i];
		fi
	endfor
	for i = 1 upto n:
		if C[i]=red:
			fill bpath.X[i] withcolor .3[white,red];
		fi
		draw bpath.X[i] withcolor C[i]
			if C[i]=red: withpen thickpen fi;
		drawunboxed(X[i]);
	endfor
enddef;
```

其中， `hoffset` 是结点之间的横向间距而 `voffset` 是纵向间距。
这些参数可以根据需要调整 （例如， 画结点较多的树时可能希望使用较大的纵向间距）。

n 个点的坐标共有 2n 个未知数。 关于 x 我列了 n-1 个方程， 关于 y， 因为有一个结点没有父结点， 所以也列了 n-1 个方程。 这样， 我只需随意指定一个点的坐标 （如这里令第一个点坐标是 (0,0)）， MetaPost 就可以解出所有 n 个点的坐标。 这是 MetaPost 优于其他作图工具之处。

之后， 就是利用 `boxes.mp` 里的工具， 画出 n 个结点， 以及父结点到子结点的连线。
注意， 顺序是： 先画连线， 再涂色， 再画边框， 最后画结点中的文字。

显然， 前面的 `V` `P` 等数组都是可以用程序生成的 （一遍中序遍历， 同时记录父结点即可）。 而画出的树的风格则可以通过修改 `drawtree` 宏来控制。
