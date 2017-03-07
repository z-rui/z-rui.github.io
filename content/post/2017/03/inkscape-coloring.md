---
date: 2017-03-07T11:48:34+08:00
title: 用 Inkscape 给 svg 文件上色
---

{{<figure src="/media/ctanlion-1.svg"
  caption="Duane Bibby 为 TeXbook 绘制的插图 (来自 http://ctan.org/lion)">}}

{{<figure src="/media/ctanlion-2.svg"
  caption="上色后的效果">}}

<!--more-->

使用 Inkscape 给 svg 文件上色的办法是： 首先删除原图中的白色成分， 变成透明。
然后创建新的图层， 位于原图层的下方。
在新的图层中， 用钢笔 （贝塞尔曲线） 工具描绘原图的轮廓。
描完以后选择所需的颜色进行填充， 并把线条颜色设置为透明。

对于开放边界的物体 （如图中的衣服、 书架上的书等），
可以使用渐变色使得填充色在开口处逐渐消失。
