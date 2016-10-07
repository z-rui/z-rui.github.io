---
date: 2016-09-29T15:58:00+08:00
title: 使用PSTricks作图
---

制作需要打印的文件的时候，插图的分辨率一定要足够高，否则看起来的效果十分恶心：

{{<figure src="/media/sot223-1.png" >}}

像这种在屏幕上显示就已经糊了的图（可能是在放大比例不正确的情况下通过截图得到的），打印出来更是惨不忍睹。

文档中许多非照片类的插图，如示意图、流程图等，可以用[PSTricks](http://tug.org/PSTricks/main.cgi/)来画，画出来的是高质量的矢量图，这样就无需关心分辨率的问题。

下图是我用PSTricks画的同一个零件的图纸。实际上，对于零件图纸，我觉得最好的方案是直接用CAD软件导出矢量图(SVG或者PostScript)，非常省事。(但是似乎不是所有CAD软件都有导出矢量图的选项。)

{{<figure src="/media/sot223-1.svg" >}}

<!--more-->

我以前也用[MetaPost](http://www.tug.org/metapost.html)画图，画过最复杂的一张是一次作业里的插图：

{{<figure src="/media/hw8-3.svg" >}}

后来因为需要在插图中写一些中文，而

1. 我用XeTeX引擎配合TrueType字体排版中文；
2. MetaPost不支持XeTeX引擎；

所以没法用MetaPost。于是转而使用了PSTricks。

PSTricks主要的特点是自带的功能比较多。例如，画一个矩形，只需指定它的两个对角，同时可以直接指定是否需要画圆角。MetaPost里面如果要画矩形恐怕需要分别画它的四条边，圆角也需要手动绘制（当然，可以定义一个宏，方便以后使用），画一些复杂的图形如果不是特别娴熟的话还是感觉有点力不从心。坐标系、统计图等比较复杂的图形，在PSTricks中可以用一条命令可以直接画出来。

PSTricks另一个特点是它作为TeX的宏包而不是独立程序，作图的代码是直接嵌入TeX源文件的，TeX引擎排版的时候会自动处理这些图形。相反，MetaPost的作图语句需要保存在一个单独的`.mp`文件中，调用`mpost`程序可以生成一系列PostScript文件。这些文件可以被插入TeX文件中，也可以用于其他目的。

MetaPost有一套不错的变量系统，可以用来储存所画的图像（并对其进行处理）、储存坐标、求解线性方程组等。MetaPost的宏用起来也算比较方便。PSTricks则完全依赖TeX，它的宏即TeX的宏。能否进行上述复杂的运算我还不太清楚（目前没用到这么高级的功能）。

总的来说，画一般的插图（其实主要就是：直线、圆、箭头、文字）我估计PSTricks还是要方便一点，以后我会试着多用PSTricks。

还有一个流行的做图工具是叫PGF/TikZ。这个东西据说非常高级，但我总觉得太复杂，一直没用过。在一个介绍电学文章里面用过基于它的`circuitikz`宏包，用来画一些电路图，还算比较方便，但也不是特别好用。据说PSTricks也可以用来画电路图，以后有机会我就试试看吧。


## 注：输出SVG

文中的SVG是这样得到的：

```
\documentclass{standalone}
\usepackage{pstricks}
\begin{document}
\begin{pspicture*}
% PSTricks代码 ...
\end{pspicture*}
\end{document}
```

```
latex figure.tex
dvisvgm -n figure.dvi
```

`dvisvgm`的`-n`选项使得图中的文字都用矢量曲线描述，从而保留了字体的形状，但是丢失了文字信息。如果不加这个选项则保留文字信息，据我所知没有办法保留字体的形状。

要支持中文，可以使用XeTeX引擎。在导言区加入

```
\usepackage{xeCJK}
\setCJKmainfont{...} % 选择一个中文字体
```

然后执行

```
xelatex -no-pdf figure.tex
dvisvgm -n figure.xdv
```

MetaPost本身就支持输出SVG，只要在代码中添加

```
outputformat := "svg";
```

即可。但是因为不支持XeTeX引擎，要支持中文，我估计只能用LaTeX的CJK宏包或者更老的其他宏包。
