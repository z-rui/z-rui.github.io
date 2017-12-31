---
date: 2017-12-30T21:52:00-05:00
title: 自制 IBus 输入法 (2)
---

今天我进一步完善了我自制的 IBus 输入法。主要有两方面的改进：

1. 码表不再是 hardcode 在代码中的了，而是运行时加载。这样，可以支持多个码表，只需修改 [XML 文件](https://github.com/ibus/ibus/wiki/DevXML) 的 `<engines>` 项目，不需要重新编译程序。
2. 用 `IBusProperty` 来指示输入法的中/英文状态。

<!--more-->

根据第 1 条，我又做出了一个拼音输入法。当然，因为是简单的查表，输入体验是很垃圾的。大多数 Linux 发行版有一个叫 `ibus-libpinyin` 的输入模块，在 IBus 中的名字叫做“Intelligent Pinyin”。受此启发，我把我的输入法命名为“Dumb Pinyin”。

{{<figure src="/media/ibus-2.png" caption="Dumb Pinyin">}}

关于第 2 条，新版本的 IBus 多了这样一个功能：在托盘图标中动态显示输入法的状态。
这是通过设置 `icon-prop-key` 属性实现的。但是，在 WWW 上能找到的[官方的 API 参考](http://ibus.github.io/docs/ibus-1.5/index.html)的版本仍是 1.5.4，其中是找不到这个属性的。

关于这点，还有另一个问题：IBus 用一种特殊的<span style="background-color: #444444; color: #415099">蓝色</span>显示这个状态。你的系统配色很可能和它不搭，导致你根本看不清。怎么修改？IBus 的设置选项里找不到。我经过很仔细的网上搜索才在一个[日语网页](http://tomcat.nyanta.jp/sb2/sb.cgi?eid=669)上找到了解决办法：
```
$ gsettings set org.freedesktop.ibus.panel xkb-icon-rgba '#ffffff'
```
这个设置深深地埋藏在 dconf 中，你基本上也没有机会自己找到它。
我完全不理解 IBus 为什么不用标准的主题色，就像窗口的标题栏文字那样，随着主题切换而自动选用合适的颜色。
