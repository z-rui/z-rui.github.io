---
date: 2015-07-06T23:06:00+08:00
title: "基于Docker的自动构建"
---

[Wercker]现在支持基于[Docker]的自动部署。我不是很清楚Docker的运作原理。就我现在的理解，Docker是一个虚拟机快照，其中已经配置好了用户所需要的软件。每次运行时，就是解包这个虚拟机然后执行任务。

之前因为[ArjenSchwarz/wercker-step-hugo-build](https://github.com/ArjenSchwarz/wercker-step-hugo-build/)这个脚本不支持基于Docker的自动构建，所以我还在使用Wercker的旧的构建模式。

问题在于，在目前基于Docker的构建模式中，默认的虚拟机`debian`只是一个空壳，像git和wget这样的软件统统都没有安装。

在构建的时候，脚本需要使用wget下载hugo程序。我曾试过在构建之前执行

```bash
apt-get -y install wget
```

但是似乎找不到这个软件包。今天我发现构建脚本更新了，不知道作者是不是也遇到了同样的问题，所以改而使用curl。

我还需要安装git。这个过程会下载很多软件包并安装。构建的时候做一次，部署的时候又做一次，浪费时间和计算资源，我觉得很不合理。

我觉得合理的做法是，在Docker的虚拟机中预先安装好git和hugo，这样每次构建的时候只需解包虚拟机并且运行就可以了，并不需要安装额外的软件包。只是我现在还不知道怎样实现这样的Docker功能。

目前最好的结果是，我找到了[yesops/git](https://registry.hub.docker.com/u/yesops/git/)这个Docker，其中已经安装好了git和curl。这样我就不用再去劳烦`apt-get`了，应该可以省去不少重复劳动，虽然不是人力劳动而是自动化的机器劳动。


[Wercker]: http://www.wercker.com/
[Docker]: http://www.docker.com/

<!--more-->
