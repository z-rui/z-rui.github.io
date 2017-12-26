---
date: 2017-12-25T17:18:00-05:00
title: Gentoo 除错日志
---

之前几天重新配置了我的 Gentoo 桌面环境。很遗憾的是官方源中的 Gnome 在不用 systemd 的情况下是无法顺利安装的，我暂时不打算更换整个 init 系统，所以我先装了 Xfce 凑合。（除了功能稍显单薄，Xfce 的各方面我都很喜欢，可能是因为它是我用过的第一个 Linux 桌面环境。）
```
rui@localhost ~ $ uname -a
Linux localhost 4.14.8-gentoo-r1 #6 SMP Mon Dec 25 16:54:41 EST 2017 x86_64 Intel(R) Core(TM) i7-6500U CPU @ 2.50GHz GenuineIntel GNU/Linux
```
<!--more-->
在内核里配置好了显卡、网卡、声卡后，桌面环境基本就可用了。这里的一个注意点是，要把驱动配置为内核模块，否则无法加载固件。固件从 `linux-firmware` 包中安装即可。为了节省空间，我利用 `savedconfig` USE 选项，只安装如下文件：
```
i915/skl_dmc_ver1_26.bin
intel/ibt-11-5.ddc
intel/ibt-11-5.sfi
iwlwifi-8000C-27.ucode
```
安装 alsautils 和 xfce4-alsa-plugin 可以实现音量控制。后者功能很弱，不支持键盘快捷键。可以在 xfce4-keyboard-settings 中手动绑定：
```
amixer set Master 5%+    # 增大音量
amixer set Master 5%-    # 减小音量
amixer set Master toggle # 切换静音
```
暂时没发现蓝牙耳机的解决方案（不使用 PulseAudio 的情况下）。目前最好的结果是可以用 speaker-test 放出声音，但是 firefox 等应用程序的声音仍输出至扬声器，且 alsamixer 的声卡列表中找不到相应选项。参考[1](https://wiki.archlinux.org/index.php/Bluetooth_headset#Headset_via_Bluez5.2Fbluez-alsa)、[2](https://bbs.archlinux.org/viewtopic.php?id=221232)。

Xfce 和 elogind 可以和平共处，这样就不需要 ConsoleKit 了。
```
USE='-consolekit elogind'
```
但是 elogind 仍处在测试阶段，要去掉 mask 才能使用。

在有 ConsoleKit/elogind 的情况下，可以从图形界面直接睡眠/关机。否则，只能用 root 执行 `poweroff` 来关机。

问题来了：我每次选择“睡眠”时，屏幕关闭，但系统要过 10 秒左右才进入睡眠状态。从睡眠状态恢复时，界面无响应约半分钟。

我怀疑是 elogind 的问题，因为之前我用 ConsoleKit 的时候并没有遇到这种问题。但是，我禁用了 elogind 后，手动睡眠
```
echo mem > /sys/power/state
```
仍有同样的问题。因此可能是内核选项配置有误。我找了和电源管理有关的选项，没发现可疑的部分。网上查了很久 “freeze suspend” 相关内容，没有找到可用的解决方案。

看 `dmesg -H`，发现睡眠前后有声卡驱动的错误信息。于是禁用声卡驱动启动，睡眠问题消失。至此确定是声卡驱动的锅。到网上查询该错误信息，得到解决方案：编辑 `/etc/modprobe.d/intel.conf`：
```
options snd-hda-intel single_cmd=1
options snd-hda-intel probe_mask=1
```
问题解决。该解决方案网上也极难找到。

安装 xfce4-power-manager 时，为了不引入 ConsoleKit 的依赖，需要启用 `systemd` USE 选项（此选项并不引入 systemd 依赖，见[#639432](https://bugs.gentoo.org/639432)）。当与 elogind 配合使用时，在笔记本盖子合上时无法自动进入睡眠状态。解决方案是
```
xfconf-query -c xfce4-power-manager -p /xfce4-power-manager/logind-handle-lid-switch -s false
```
网上能找到其他的解决方案，但对我无效。

睡眠是桌面系统的基本功能。这都能出问题，很难想象 Linux 的桌面环境在其他方面能好到哪里去。一方面是内核驱动造成的无响应问题，另一方面是 userspace 的问题：ConsoleKit、elogind、UPower……不知道是什么道理，要把一个简单的事情弄得那么复杂，而且软件之间有着各种兼容性问题。
