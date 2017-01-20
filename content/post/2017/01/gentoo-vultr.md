---
date: 2017-01-20T16:08:00+08:00
title: 在Vultr上安装Gentoo
---

[Vultr](http://www.vultr.com/?ref=7089997-3B)是和DigitalOcean类似的VPS服务提供商。它比后者多一个功能：挂载任意ISO镜像。这样就可以安装任意操作系统了。

我用这个功能安装Gentoo。主要参考[这篇文章](https://www.vultr.com/docs/installing-gentoo-linux-on-a-vultr-server)，以及Gentoo的官方手册。

我没有用Gentoo安装盘，而是用了Vultr的ISO library中已有的ArchLinux安装盘。这样省去了上传ISO镜像的步骤。而且事实上ArchLinux的安装盘好用得多。

分区、格式化、下载和解包stage3……等等步骤，都没有太多需要注意的地方。chroot之前要记得复制`/etc/resolv.conf`，否则之后无法连网。然后就是emerge一些东西，也没什么难度。

值得注意的是内核编译选项。文章中明显提到了要启用`VIRTIO_PCI`和`VIRTIO_MMIO`，实际上还要启用`VIRTIO_BLK`和`VIRTIO_NET`，才能访问硬盘和网络。很多其它硬件驱动都可以禁用。

最后按照文章里写的配置好系统就可以了。安装GRUB启动器，如果`/dev`等虚拟目录没有正确挂载到chroot的根目录下也会安装失败。重启之前记得运行`passwd`设置root密码。

<!--more-->
---

换用Vultr的原因是最近我的网络访问DigitalOcean的速度不理想。我尝试更换机房，但是更换的时候出现了新的问题：DigitalOcean的FreeBSD主机无法联网。

发了support ticket，工作人员给的解答是`/etc/rc.digitalocean.d/droplet.conf`的符号链接可能失效了。我看了一下，确实如此。把它更正之后重启，问题并未解决，而且符号链接再次失效。

观察启动过程我发现一些可疑的错误消息，找不到python的动态链接库之类。经分析，这个错误信息应该是`/etc/rc.d/digitalocean`产生的。我尝试在这个文件的`REQUIRE:`一行增加了`ldconfig`，问题解决。

我不太确定以后是不是仍然会产生类似的故障。总之目前来说已经不太愿意再用DigitalOcean上的FreeBSD了。
