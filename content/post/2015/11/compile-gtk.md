---
date: 2015-11-08
title: "编译GTK+"
---

[GTK+](http://www.gtk.org/)是一个流行的图形界面工具包，它的结构比较复杂，有很多外部依赖：

{{< figure src="/media/gtk-dependency-1.svg" alt="GTK+ dependency graph" >}}

而我今天要尝试的是，在Windows上编译最新的GTK+ 3.19.1。所有外部依赖均从源代码开始编译。为何想到从源码编译GTK+，是因为市面上难以找到最新版的GTK+用于Windows的安装包。[GTK+ for Windows Runtime Environment](http://gtk-win.sourceforge.net/home/)是一个很好的安装包，但是只更新到GTK+ 2.24，在2012年后就再没有更新。

由依赖关系图可知，这个编译过程会非常痛苦。而选择Windows平台无疑使得难度更高。我没有安装Visual Studio，所以我没有研究过使用Visual Studio的构建方案。[这篇文章](https://wiki.gnome.org/action/show/Projects/GTK+/Win32/MSVCCompilationOfGTKStack)可能有用。

我选择以Msys2为构建平台，并且为此编写了一个[自动化构建脚本](https://gist.github.com/z-rui/61a6722d3c858197d0d8)。由Make来解析其中的依赖关系（其实写成shell脚本也可以，只要按照拓扑排序的顺序构建就可以了。）

<!--more-->

要使用此脚本，需要做准备如下：

* [Msys2](http://msys2.github.io/)。
* gcc, make, tar, curl, git和autotools(用Msys2的包管理器`pacman`安装)。
* 约1.5GB的剩余磁盘空间。

准备好后，在Msys2中执行

    git clone https://gist.github.com/61a6722d3c858197d0d8.git
    cd 61a6722d3c858197d0d8
    make -f gtk-build.mak

若相信自己的人品，则该命令开始执行后，即可出门散步，半晌后回来就可以看到所有需要构建的东西都已经全部安装到了`$(prefix)`目录中(默认为当前目录的`gtk-root`子目录 )。不过，这个命令很有可能中途失败。若是如此，则需凭借自己的聪明才智加以解决。解决后，到我的[Gist页面](https://gist.github.com/z-rui/61a6722d3c858197d0d8)上提交一个评论，指出构建脚本的哪里应当改正。

注意：以上构建脚本不会构建`libgcc`以及`pthread`兼容库。以MINGW64为例，需要手动复制

    cp /mingw64/bin/lib{gcc_s_seh,winpthread}-1.dll $(prefix)/bin

我为了编写这个自动构建脚本，自己手工测试了各个包的编译方式和选项，中间自然是走了不少弯路，花了差不多有2天的时间。编译完成后运行`gtk3-demo.exe`，效果如下：

{{< figure src="/media/gtk3-1.png" alt="GTK+ 3 Windows" >}}
