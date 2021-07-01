---
date: 2021-06-30T22:22:19-07:00
title: 在 Windows Subsystem for Linux 上运行 Gentoo
---

最近因为更换了新的电脑，所以尝试在Windows 10上使用 WSL 代替双系统的配置。WSL 2采用了虚拟化的技术，并且具备一个真正的Linux内核。WSL 2的VM相比传统的全系统VM而言更加精简，并且Linux和Windows系统之间的可交互性更好：

* 每个操作系统都可以通过9P协议访问对方的文件系统；
* 每个操作系统都可以较方便地调用另一个操作系统的程序。例如，在PowerShell中使用`grep`一类的工具。
* Linux的文件系统储存在 `.vhdx` 文件中，可以动态扩展和收缩，不需要单独分区；
* VM的占用的内存是根据实际使用情况动态调整的。

听起来是个不错的选择。在新的预览版中，WSL 2更提供了GPU和GUI的支持。不过，我还没有用到预览版。

{{<figure src="/media/gentoo-wsl-1.png" alt="Gentoo on WSL, running neofetch">}}
<!--more-->

安装的过程可以参照[官方文档](https://docs.microsoft.com/en-us/windows/wsl/)。官方文档里指出可以在应用商店安装Linux发行版（我不太明白为什么微软现在喜欢把东西都放在Microsoft Store中，也许是为了更新起来方便？），虽然预览版中已经可以通过命令行直接安装了。应用商店中只提供了Ubuntu、Kali、Debian和Alpine，可选项很少。实际上，WSL通过导入tar包的方式使用任意的发行版。 我下载了 Gentoo 的 [stage3.tar](https://www.gentoo.org/downloads/)，然后使用 `wsl --import` 创建了一个Gentoo的实例，非常简单。

Windows内置的终端模拟器出奇难用。此前我在Windows系统中一般使用msys2自带的mintty终端模拟器。WSL文档推荐使用新开发的[Windows Terminal](https://www.microsoft.com/en-us/p/windows-terminal/9n0dx20hk701?activetab=pivot:overviewtab)。使用了一段时间后我认为可以用它代替mintty。它自带的Cascadia Code字体具有Ligature，类似于我之前使用的Fira Code字体。我感觉不如Fira Code好看，但是使用体验还不错。值得一提的是，它还有名为"Retro terminal effects"的功能，深得我心 :) 我在Linux上找了很久具有类似功能的终端模拟器，但是没有特别好用的（主要因为不支持Ligature）。

{{<figure src="/media/gentoo-wsl-2.png" alt="Playing nethack with retro effects">}}
