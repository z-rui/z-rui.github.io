---
date: 2015-09-20
title: "Google的新字体noto"
---

最近我在[VirtualBox][1]上跑[FreeBSD][2]系统。安装好X窗口系统后，自带的中文字体非常丑陋，所以有必要安装一些新的字体。

根据我以往的经验，[文泉驿][3]提供了一套非常优质的中文字体，被许多Linux的发行版选作了默认的中文字体。在FreeBSD中也有`wqy`包可供安装。

文泉驿有一款字体称为微米黑，其基于Google的Droid Sans Fallback，通过部件拼接来构造汉字，从而在保持字体体积较小的情况下包含大量的字符。文泉驿制作微米黑是为了补充Droid Sans Fallback缺失的字符。

今天我查看Wikipedia页面的时候，看到Google发布了一款新的字体：[Noto][4]。其名称有一个来源：浏览网页时，字体中不包含的字符通常会被显示为豆腐块。而Noto的目标是消灭豆腐块(**No** more **to**fu)。这个字体从2014年开工，一年不到就已完工，整个字体的体积高达460MB，其中绝大部分(400MB以上)是CJK字体。CJK字体包含了简体、繁体、日文和朝鲜文4种变体，以迎合不同地区的习惯。

于是我卸载了文泉驿，改而安装Noto字体。

[1]: http://www.virtualbox.org/
[2]: http://www.freebsd.org/
[3]: http://www.wenq.org/
[4]: https://www.google.com/get/noto/

<!--more-->

顺便一提，`xorg`这个包太大了，包含了那些并不需要的过时字体。可以安装FreeBSD提供的`xorg-minimal`库，减少不必要的安装。桌面环境我也没有选择Gnome或者KDE，这两者安装完毕后所占用的空间分别是2GB和3GB。我选择了相对而言比较轻量的Xfce。

安装完firefox之后(TL;DC)，就可以愉快地浏览WWW了。

{{< figure src="/media/freebsd-1.png" >}}

