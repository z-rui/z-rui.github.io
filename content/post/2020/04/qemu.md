---
date: 2020-04-19T18:28:14-07:00
title: QEMU 笔记
---

我最近尝试了在 Gentoo 上用 [QEMU](https://qemu.org/)
运行 Windows 10。 我想把过程中的一些细节记录在这里。

{{< figure src="/media/qemu-1.png" >}}

[Gentoo Wiki](https://wiki.gentoo.org/wiki/QEMU) 和 [ArchWiki](https://wiki.gentoo.org/wiki/QEMU) 上的 QEMU 条目也是很好的参考。

<!--more-->

## 设置

安装 `qemu-system-x86_64`。 启用编译选项 `gtk`。
启用 Linux 内核中的 `KVM` 选项。
[KVM](https://www.linux-kvm.org/page/Main_Page) 极大地提高虚拟机的运行效率。

创建磁盘镜像：
```sh
$ qemu-img create -f qcow2 win10.qcow2 20G
```

创建启动虚拟机的脚本文件 `start.sh`：
```sh
#!/bin/bash

CMD="qemu-system-x86_64 "
CMD+="-M q35 "
CMD+="-accel kvm "
CMD+="-cpu host "
CMD+="-smp 4,sockets=1,cores=2,threads=2 "
CMD+="-m 4G "
CMD+="-hda win10.qcow2 "
CMD+="-cdrom Win10_1909_English_x64.iso "
CMD+="-net nic "
CMD+="-net user "
CMD+="-usb "
CMD+="-device usb-tablet "
CMD+="-rtc base=localtime "
CMD+="-display gtk "
CMD+="-daemonize "
CMD+="-name 'Windows 10' "

echo "$CMD" && eval "$CMD"
```
注：
- `-smp` 选项应根据宿主机上的 CPU 的情况和虚拟机的需求而定。
- `-m` 选项应根据宿主机上的 RAM 的情况和虚拟机的需求而定。

运行此脚本并完成安装流程。 重启后可将 `-cdrom` 选项去除。

## 使用

可以用上述 `start.sh` 启动虚拟机。 打开 GTK 界面中的 “Grab on hover” 选项。
按 Ctrl+Alt+M 隐藏菜单， Ctrl+Alt+F 进入全屏。 

注： 如果不隐藏菜单， 全屏显示可能有位移。 但隐藏菜单后， Ctrl+Alt+G 失效。

### USB 设备

要在虚拟机中使用宿主机上的 USB 设备， 先找到设备的总线(bus)和设备(device)编号：
```
$ lsusb
Bus 001 Device 003: ID 04f2:b5ce Chicony Electronics Co., Ltd Integrated Camera
```
在 QEMU 的 GTK 界面中按 Ctrl+Alt+2 进入控制台。 使用 `device_add` 命令：
```
(qemu) device_add usb-host,hostbus=1,hostaddr=3,id=usb1
```
按 Ctrl+Alt+1 回到 VGA 显示。

要移除设备， 使用 `device_del` 命令：
```
(qemu) device_del usb1
```

用户需要有访问 USB 设备的权限（root， 或在 usb 组中）。

### 音频

我在 QEMU 的命令行中加上了 `-soundhw all` 后可以在虚拟机中播放声音。
效果比较差。 如果宿主机上有 PulseAudio， 可以参考 ArchWiki 中的描述。

## 镜像管理

### 在宿主机上挂载

宿主机上应安装有 `nbd` 和 `ntfs` 内核模块。
[nbd](https://en.wikipedia.org/wiki/Network_block_device)
是一种在用户态中实现块设备的方式。
```
# modprobe nbd
# qemu-nbd -c /dev/nbd0 win10.qcow2  # 将 qcow2 映射为一个 nbd
# mount /dev/nbd0 /mnt/windows  # 挂载 nbd
...
# umount /dev/nbd0
# qemu-nbd -d /dev/nbd0
```

### 扩张磁盘

我一开始使用了 10G 的磁盘镜像。 安装完后发现磁盘已满。 可以增加磁盘大小：
```sh
$ qemu-img resize win10.qcow2 +10G
```
然后调整分区大小， 在 Windows 中可以使用磁盘管理工具 (`diskmgmt.msc`) 完成。

### 压缩镜像

当磁盘较大时， 即使虚拟机没有使用全部空间， 镜像文件也会随着使用越来越大。
此时可以压缩镜像。 做此操作前应先尽可能释放虚拟机中的空间，
例如删除不必要文件、进行磁盘清理 (`cleanmgr`) 和[映像清理](https://docs.microsoft.com/en-us/windows-hardware/manufacture/desktop/clean-up-the-winsxs-folder#dismexe)。

用 `qemu-img` 创建磁盘镜像的副本：
```sh
$ mv win10.qcow2 win10.qcow2~
$ qemu-img convert -p -O qcow2 win10.qcow2~ win10.qcow2
```
另外， 也可加上 `-c` 选项， 这样还会对磁盘中已被占用的部分应用压缩算法。

启动虚拟机， 确认磁盘镜像无问题后， 可以删除旧的镜像。

## 后记

QEMU 的作者是 [Fabrice Bellard](https://bellard.org/)。这人还有许多其他有名的作品，包括 [FFmpeg](http://ffmpeg.org/) （被广泛使用和[抄袭](https://zhuanlan.zhihu.com/p/25794414)的媒体编码程序库） 和 [TinyCC](https://bellard.org/tcc/)。
他还发明了一种快速计算圆周率的[算法](https://bellard.org/pi/)，并曾以此成为世界纪录保持者。
