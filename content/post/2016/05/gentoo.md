---
date: 2016-05-14T18:06:00+08:00
title: Gentoo构建日志
---

我对虚拟机中运行的FreeBSD总是很满意。所以产生了在真实环境中运行的念头。但是我天真了。尝试了才发现FreeBSD并不能驱动我的无线网卡，上不了网的话其他的也不要谈了。

只好乖乖地转投了Linux对怀抱。在Arch和Gentoo之间选择了一会儿。Arch很好很方便，我还用了装上去用了一会儿。它的软件版本之新也着实把我吓了一跳(例如gcc已经更新到了6.1.0版本)。

我最终选择了更加折腾的Gentoo。Gentoo配置起来更麻烦一些，所以有必要把相关步骤记录下来，以供日后参考。

<!--more-->

讽刺的是，我是用Arch Linux的安装盘安装的Gentoo，原因是：Gentoo的最小安装光盘也不能驱动我的无线网卡。Arch的安装盘包含的内核模块要全面一些，所以操作起来没多大困难，这是我很佩服Arch的地方。当然，也可以用别的安装盘，只要能连上网就可以了。

## 准备安装介质

到Arch Linux的官方网站上[下载](https://www.archlinux.org/download/)最新的安装镜像。由于我的计算机没有光驱，我用[Rufus](https://rufus.akeo.ie/?locale=zh_CN)把镜像烧写到USB驱动器。

烧写完毕后，最好把Gentoo的stage3也[下载](https://www.gentoo.org/downloads/)到USB驱动器中，因为在控制台的安装界面里盯着curl的下载进度条慢慢地跑是一件很无聊的事情。使用多线程下载工具，很快就可以下载好。也可以把portage快照一并下载了；163.com维护着一个[国内的镜像](http://mirrors.163.com/gentoo/snapshots/)。

准备工作做好后，重启计算机，使用USB驱动器作为启动设备，就可以进入Arch Linux的界面。

## 设置网络

我的计算机必须通过Wifi连接到网络，所以配置起来有一定的门槛。首先，我的网卡型号是RTL8723AU，这个型号在Realtek公司的官方网站上查询不到，是一个很冷门的硬件。幸运的是Arch Linux的安装盘里包含它的驱动。

但是，这里还有一个陷阱：默认情况下，内核同时加载了名为`rtl8xxxu`和`r8723au`的模块。在这种情况下，即便连上了网络，只要有一些通讯，很快就会断线。这个问题困扰了我很久，而且互联网上似乎没有找到解决办法。经过一番捣鼓，我找到了解决方案：

1. 使用命令`modprobe -r rtl8xxxu r8723au`禁用这两个模块；
2. 使用命令`modprobe r8723au`加载网卡模块；
3. 现在可以运行`wifi-menu`配置无线网络了。

现在想来，都不知道是怎么给我找出来这个解决方案的。从这些步骤里可以看出，大约是`rtl8xxxu`这个模块有问题，跟我的网卡不太兼容。

安装光盘里提供的`wifi-menu`配置无线网络非常方便，只需选择需要连接的网络，然后输入无线密码就可以了。可以说Arch Linux制作安装光盘是很用心的。

## 磁盘分区

如果磁盘上已经安装了Windows，可以使用“计算机管理”工具把其中一个分区缩小，这样就多出了空闲的空间可供安装Linux。这个步骤虽然在Linux中也可以完成，但是比较麻烦，搞不好的话会危及到数据。

运行`gdisk`工具修改分区表，在空闲的区域创建若干新的分区，我创建的分区有

1. EFI启动分区，200MiB。
2. Linux根目录分区，8.8GiB。
3. Linux交换分区，1GiB。

然后格式化这三个分区：EFI分区必须使用vfat格式，Linux的分区可以使用多种[文件系统](https://wiki.gentoo.org/wiki/Handbook:AMD64/Installation/Disks#Filesystems)，我使用的是[Btrfs](https://en.wikipedia.org/wiki/Btrfs)，而交换分区需要用`mkswap`命令创建。

```
mkfs.vfat /dev/sda10
mkfs.btrfs /dev/sda11
mkswap /dev/sda12
swapon /dev/sda12
```

## 安装系统

现在要开始安装Gentoo系统了。按照Gentoo Handbook上的[步骤](https://wiki.gentoo.org/wiki/Handbook:AMD64/Installation/Stage)操作。中间可能还需要参考[Installation from non-Gentoo LiveCDs](https://wiki.gentoo.org/wiki/Installation_alternatives#Installation_from_non-Gentoo_LiveCDs)一节的相关说明。

## 配置内核

沿着Handbook一直读下去，直到内核配置这一步。我采用手动配置的方法，在配置内核的时候，应当注意选择和硬/软件相匹配的项目，例如：

* Btrfs文件系统。这样才能加载根目录。
* RTL8723AU驱动。这个驱动在Staging Drivers子项中。
* EFI Stub。这使得内核可以直接被EFI固件加载并启动。
* NTFS。我的硬盘上还有2个NTFS分区，为了读写其中的文件需要编译此模块。

内核配置好之后，运行`make`开始编译，这时可以外出散个步。回来以后使用

```
make install modules_install
```

安装内核和内核模块。

还应当安装`sys-kernel/linux-firmware`，我的网卡驱动需要用到。

## 配置EFI启动

Gentoo官方指南推荐使用Grub2作为引导器。实际上，在使用UEFI的硬件上，可以不使用额外的引导器。

在配置内核的时候，Built-in kernel command line(`CMDLINE_BOOT`)选项里可以填写启动参数，填上

```
root=/dev/sda11
```

这样内核就知道应该怎么加载哪个分区作为根目录。

如果需要initramfs，那么还可以指定initrd参数(`BLK_DEV_INITRD`)。因为我的分区结构比较简单，所以就不需要了。

将编译好的内核(bzImage或者文件名为`vmlinuz-x.y.z-gentoo`)复制到EFI分区的`\boot\efi`路径下，并且命名为`bootx64.efi`，这样UEFI固件就能找到Linux内核并且成功加载。

如果没有在内核编译选项中设置根目录分区和initramfs文件，那么这些参数需要在启动时现场提供。可以使用efibootmgr修改UEFI固件来做到这一点：

```
efibootmgr -c -d /dev/sda -p 10 -L "Gentoo" -l "\vmlinuz-x.y.z-gentoo" -u "root=/dev/sda11"
```

这时候就不必把内核放在`\boot\efi\bootx64.efi`位置了。

如果没能正确地设置EFI参数，则可能出现如下情况：

* UEFI启动菜单中没有Gentoo选项。这通常是因为UEFI固件找不到所设置的文件路径。
* 启动过程中发生kernel panic，提示找不到root。这说明启动参数设置错误。

如果需要高级一点的启动办法，那么可以选择[安装Grub 2](https://wiki.gentoo.org/wiki/Handbook:AMD64/Installation/Bootloader#Default:_Using_GRUB2)。Grub 2提供了一个图形界面的启动菜单，在这个界面里可以现场修改启动参数（例如进入单用户模式等）。

## 配置新系统

重启到新系统之前，记得要做几件事情。

1. 运行`passwd`，设置root密码，否则根本没法登录。
2. 设置[无线网络连接](https://wiki.gentoo.org/wiki/Handbook:AMD64/Networking/Wireless)。至少要安装必要的软件包，如wpa_supplicant。
3. 从chroot环境中退出，然后干净地重启系统，确保挂载的几个文件系统被正确地卸载。

进入新系统后就可以进行一系列的配置了。

可以考虑安装ccache，开启编译器缓存，对于需要多次编译的Gentoo系统而言，有可能对编译速度有帮助。

```
emerge dev-util/ccache
```

安装完毕后，需要编辑`/etc/portage/make.conf`，增加

```
FEATURES="ccache"
CCACHE_SIZE="2G"
```

启用portage的编译器缓存功能(缓存默认存放在`/var/tmp/ccache`)，并将缓存大小设置为2GB。

一个很重要的软件是X窗口系统。我的计算机使用intel核心显卡，因此在`make.conf`加入特别的选项，使得X窗口系统可以加载intel专用驱动，提高图形性能。

```
echo 'VIDEO_CARDS="intel"' >> /etc/portage/make.conf
emerge xorg-server
```

终端下能做的事情非常有限，有了X窗口系统（配合其他软件包），就可以方便地：

* 以窗口化的形式进行多任务操作。
* 浏览WWW。
* 查看、输入中文。


## 轻量级桌面环境

市面上有很多标榜“轻量级”的桌面环境。事实是，它们都是重量级的。要做到轻量级，那就只能自己选择窗口管理器和其他组件。

我使用dwm+dmenu构成一个最小化的桌面环境，可以说没有任何“多余”功能（实际上也缺少了很多我想要的功能）。

目前，portage仓库中的dwm稳定版只更新到了6.0。这个版本没有使用Xft渲染文字，因此显示中文字符会存在一些问题。我使用6.1版本的dwm。

```
echo ">=x11-wm/dwm-6.1" >> /etc/package.accept_keywords
emerge x11-wm/dwm
```

`dwm`的配置是直接写在源代码的`.h`文件里的，编译完毕后就不再提供任何配置选项了。为了实现功能自定义，应该打开名为`savedconfig`的USE。然后编辑`/etc/portage/savedconfig/x11-wm/dwm-6.1`。

值得修改的几处地方有：

1. 在`fonts[]`数组后面增添一个中文字体。不这么做虽然也可以显示中文，但是大小可能不正确。
2. 把`termcmd[]`改成所使用的终端程序。我使用的是`uxterm`。

3. 其他就是一些颜色、快捷键之类的设置，可以根据自己的偏好调整。

修改好了之后需要重新`emerge dwm`。

dwm有一个配套的程序dmenu。

```
emerge x11-misc/dmenu
```

和dwm一样，dmenu的配置也是通过修改`.h`文件完成。但是目前portage维护的dmenu包并不支持`savedconfig`功能，而只能采用用户补丁的形式。将下面的内容保存为`/etc/portage/patch/x11-misc/dmenu-4.6/dmenu-config.patch`:

```diff
--- config.def.h	2016-05-14 16:34:17.335458392 +0800
+++ config.h	2016-05-14 17:27:45.128315375 +0800
@@ -4,7 +4,8 @@
 static int topbar = 1;                      /* -b  option; if 0, dmenu appears at bottom     */
 /* -fn option overrides fonts[0]; default X11 font or font set */
 static const char *fonts[] = {
-	"monospace:size=10"
+	"monospace:size=10",
+	"Droid Sans Fallback:size=10"
 };
 static const char *prompt      = NULL;      /* -p  option; prompt to the left of input field */
 static const char *normbgcolor = "#222222"; /* -nb option; normal background                 */
```

我用的是Droid Sans Fallback字体，也可以改成其他的。

创建补丁之后再`emerge dmenu`。

然后修改`~/.xinitrc`使之启动`dwm`：

```
xrdb -merge $HOME/.Xresources

exec dwm
```

这时就可以在终端下运行`startx`启动X窗口系统了。可以看到一个简单的dwm任务栏。默认的快捷键有：

- `Alt-Shift-Enter`启动终端模拟器。
- `Alt+p`打开dmenu，可以在其中输入命令并执行。
- `Alt+Shift+c`，关闭当前窗口。
- `Alt+Shift+q`，退出dwm。小心，这会关闭所有正在运行的窗口程序，并且不会提示确认。

## 中文字体和输入法

我使用Google的Droid Sans Fallback作为中文字体。

```
emerge media-fonts/droid
```

注意不要在`eselect fontconfig`中启用`59-google-droid-sans.conf`这个配置文件。它会把`Droid Sans Fallback`这个字体名映射到`Droid Sans`，并造成xterm把中文显示为方块i和一些额其它问题。

如果没有安装其他字体，那么可以启用`59-google-droid-sans-mono.conf`设置缺省的等宽字体。但是这个字体有个缺点，那就是`0`和`O`长得太像了。

此外还有一些高质量的中文字体，例如`source-han-sans`。不过体积也比较大。

输入法我使用的是fcitx。

```
emerge app-i18n/fcitx
```

为了使fcitx随X窗口系统而启动，并且能够在应用程序中使用，需要修改`~/.xinitrc`，增加如下内容

```sh
export XMODIFIERS="@im-fcitx"
export GTK_IM_MODULE="xim"

fcitx
```

注意要放在`exec dwm`之前，因为之后的任何语句都不会被执行到。

## 浏览器

我使用基于webkit-gtk的浏览器：surf。

```
emerge www-client/surf
```

浏览器的核心`webkit-gtk`需要编译很久，而这个浏览器本身则仅仅是一个外壳，功能非常简陋。除了页面上的交互需要鼠标以外，其他功能需用键盘完成，例如：

- `Ctrl-g`，然后输入URL并按Enter键。跳转到相应的URL。
- `Ctrl-/`，然后输入要查找的文本并按Enter键。在页面上查找相应的文本。

这两个功能都是借助上面提到的dmenu来实现文本输入功能的。不幸的是，4.6版本的dmenu并不支持中文输入法，现象是：在dmenu的输入框中，输入法无法切换到中文状态。所以想在网页中搜索中文词语就成了一个难题。这个问题可以通过我在[上一篇文章]({{<ref "post/2016/05/suckless.md">}})中提到的补丁来解决。

将那两个补丁放到`/etc/portage/patch/x11-misc/dmenu-4.6`目录下，重新`emerge dmenu`即可。补丁后的dmenu可以正常地接受来自中文输入法的输入。

值得注意的是，这个补丁对scim无效。如果使用scim作为输入法，则补丁后的dmenu不仅不能接受中文输入，反而会陷入假死状态。我对X窗口系统的输入法原理并不了解，所以也不明白这是怎么一回事。

除此以外，`Ctrl-Space`在surf默认为“向下翻页”的快捷键，这和fcitx默认的“调出输入法”的快捷键冲突，有可能导致在surf中无法调出中文输入法。解决办法有两种：

1. 修改fcitx的调出输入法的快捷键（使用`fcitx-configtool`）。
2. 修改surf的savedconfig，把`Ctrl-Space`改成其他的组合。

## 结语

有图形界面、有终端、有浏览器、有输入法。到这里基本上什么事情都可以做了。这就是一个配置好了的操作系统。

{{<figure src="/media/gentoo-1.png">}}
