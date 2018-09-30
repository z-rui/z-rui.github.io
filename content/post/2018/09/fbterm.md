---
date: 2018-09-30T16:01:20-04:00
title: "FbTerm: 终端下的中文显示和输入解决方案"
---

Linux 内核支持 Unicode。 但是它的显示驱动只支持 256 个不同的字符。
所以没有办法显示中文。 一般的解决办法是使用 [fbterm](https://code.google.com/archive/p/fbterm/)
（该项目已停止开发）。

FbTerm 用最新版本的 FreeType 渲染时会有一些问题， 主要表现为西文字母显示出较多毛刺。
这个问题可以通过[这个补丁](https://github.com/z-rui/gentoo-patches/blob/master/app-i18n/fbterm-1.7/fix-hinting.diff)解决。

FbTerm 也提供了一个输入法框架。 我把我之前为 ibus 编写的输入法移植到了 FbTerm 中， 只不过功能更加薄弱了：
<video width="100%" controls>
<source src="/media/fbterm-im.mp4" type="video/mp4">
屏幕录像
</video>
