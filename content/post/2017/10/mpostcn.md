---
date: 2017-10-02T20:44:00-04:00
title: 在MetaPost中使用中文标签
---

[MetaPost](http://www.tug.org/metapost.html)是非常经典的矢量图绘制工具。[^1]
之前我用 PSTricks 配合 XeTeX 绘制含中文标签的图形，因为 MetaPost 中不方便使用中文标签。确实如此。

__TL; DR__  目前比较简单的办法是用 LuaTeX，因为它内置了 MetaPost 库 MPLib，可以直接输出 PDF，字体部分由 LuaTeX 负责处理。

{{<figure src="/media/mplib-1.svg" caption="使用 LuaTeX + MPLib 绘制的矢量图">}}

<!--more-->

```tex
\input luamplib.sty
% 设置中文字体，例如载入 luaotfload.sty 并定义相关的字体
\mplibcode
input figures.mp;
% --- 或者 ---
% （直接输入 metapost 代码）
input boxes;
beginfig(1);
boxit.a(btex 动物界 etex);

boxjoin(a.s - b.n = (0, 12pt));
boxit.b1(btex 脊索动物门 etex);
boxit.b2(btex 哺乳纲 etex);
boxit.b3(btex 灵长目 etex);
boxit.b4(btex 人科 etex);
boxit.b5(btex 人属 etex);
boxit.b6(btex 智人 etex);

boxjoin(a.s - b.n = (0, 12pt));
boxit.c1(btex 节肢动物门 etex);
boxit.c2(btex 昆虫纲 etex);
boxit.c3(btex 双翅目 etex);
boxit.c4(btex 果蝇科 etex);
boxit.c5(btex 果蝇属 etex);
boxit.c6(btex 黑腹果蝇 etex);

a.s - b1.n = (a.s - c1.n) xscaled -1 = (whatever, 16pt);
b1.e - c1.w = (-12pt, 0);

drawboxed(a);
for i = 1 upto 6: drawboxed(b[i], c[i]); endfor
linecap := butt;
drawarrow a.s -- b1.n;
drawarrow a.s -- c1.n;
for i = 1 upto 5:
  drawarrow b[i].s -- b[i+1].n;
  drawarrow c[i].s -- c[i+1].n;
endfor
endfig;
\endmplibcode
\bye
```

通过恰当地设置输出例程，可以让每个图像正好占据一个 PDF 页面（且页面大小正好为图像大小）。很多新的 TeX 引擎（PDFTeX、XeTeX、LuaTeX）支持嵌入 PDF 内容。这样，可以单独编译 MetaPost 代码，之后把图片嵌入正文中，就不必反复编译了。

要注意一个问题：如果使用 `TEX()` 动态生成标签，则不要 `input TEX;`。因为 mplib 自带了 `TEX()`；用 `TEX.mp` 里面定义的 `TEX()` 反而会引起问题。

LuaTeX 也有它的缺点。TrueType/OpenType 的字体支持是通过 luaotfload 宏包实现的。它的载入速度相当慢。我自定义了一个预先载入中文字体的格式，这使得编译时间缩短了几秒（代价是不能随意更换字体），但是仍然比传统的 TeX/MetaPost 慢很多。

这个办法还有一个缺点，就是错误消息非常难读，不像传统的 MetaPost 以交互式的方式显示错误信息。可能是我设置的不对，我可以再研究一下。

---

我另外写了一个非常方便的 `boxpages.tex`，它可以把文档中的每个 box 作为单独的一页输出到 PDF 文件中，且页面的大小正好等于 box 的大小。

```tex
\vsize=0pt
\hoffset=-1in
\voffset=-1in

\output={
  \setbox0=\vbox{\unvbox255\global\setbox1\lastbox}
  \pagewidth\wd1
  \pageheight\ht1
  \advance\pageheight\dp1
  \shipout\box1
  \advancepageno}
```

这样也方便接下来的处理， 例如将 TeX 输出转换为图片。

---

传统的 MetaPost 程序是独立运行的，生成若干 Encapsulated PostScript 文件。它只认识 TeX 字体，需要读取 TFM 文件（才能知道文字占多少空间）。TFM 是一种古老的格式，每种字体只支持 256 个字符，所以不适合描述中文字体。

要改善这个尴尬情况，最理想的办法是修改 TeX 引擎使其在每种字体中支持更多字符。
不幸的是 TeX （以及 TFM 格式）是一个冻结的项目，不可能有多大改变。

中华儿女多奇志。就算不改 TeX 引擎，也有办法：生成多套字体，通过某种处理（预处理程序，或者玩弄 TeX 的 `\catcode` 机制），把每个汉字转换成“切换字体”、“当前字体内该汉字对应的序号”（0-255）、“其他调整”（间距、断行等）三个部分。做完这些处理之后，TeX 就可以开心地排版中文文档了。

这大概就是 CCT 和 LaTeX 的 CJK 宏包的原理。这些做法不仅繁琐，而且配置起来非常麻烦。理论上讲，把这些方法应用于 MetaPost，那么也可以排版处含中文标签的图像。我没有试过。

[^1]: 另外两个非常流行的矢量图绘制工具是[PSTricks](http://tug.org/PSTricks/main.cgi/)和[PGF/TikZ](http://pgf.sourceforge.net/)。
