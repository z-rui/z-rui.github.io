---
date: 2020-04-26T21:17:43-07:00
title: 用SquashFS存储Portage树
---

Gentoo系统中的[Portage树](https://wiki.gentoo.org/wiki/Ebuild_repository)存储了软件仓库中**所有**软件的元信息。其中最主要的是[Ebuild脚本](https://wiki.gentoo.org/wiki/Ebuild)，它记载了软件的编译方法、依赖关系等。它通常位于`/usr/portage`。

可想而知，这个树中包含了大量的文件，因此占用了很大的磁盘空间。根据我最近的测量，
- 所有文件的大小总和超过300MiB；
- 实际占用磁盘空间超过600MiB (ext4)。

但实际上，其中大部分文件都比较小，且属于较易被压缩的文本文件。因此，值得考虑使用一种压缩的文件系统存放Portage树。

[SquashFS](https://www.kernel.org/doc/Documentation/filesystems/squashfs.txt)是一种可压缩的只读文件系统。将Portage树的全部内容放在其中，只需约50MiB的存储空间。

在配置SquashFS的过程中，走了一些弯路。因此，我把操作过程记录在这里，以便日后参考。

<!--more-->

## 准备工作

传统上，Portage把源码包 (distfiles) 和二进制包 (packages) 都放在`/usr/portage`目录下。它们占用比较大的空间，但不应该被放进SquashFS中。因此，修改`make.conf`：
```sh
DISTDIR="/var/cache/distfiles"
PKGDIR="/var/cache/binpkgs"
```
并把它们移到其他位置：
```sh
$ mv /usr/portage/distfiles /var/cache
$ mv /usr/portage/packages /var/cache/binpkgs
```

## 只读Portage树

最简单的配置方案是使用只读的Portage树。要使用SquashFS，应在内核选项中启用`CONFIG_SQUASHFS`。

### 创建SquashFS

Gentoo的[官方镜像](http://distfiles.gentoo.org/snapshots/squashfs/)维护着每天更新的SquashFS镜像。因此，简单的解决方案是从镜像获取SquashFS：
```sh
$ wget http://distfiles.gentoo.org/snapshots/squashfs/gentoo-current.xz.sqfs
```

手动创建SquashFS的方法是，先更新Portage树，然后使用`mksquashfs`命令（需安装squashfs-tools）：
```sh
$ mksquashfs /usr/portage gentoo.sfs -comp xz
```
更多选项参见[mksquashfs(1)](https://manpages.debian.org/jessie/squashfs-tools/mksquashfs.1.en.html)。
有了SquashFS镜像，就可以删除文件系统中的Portage树了。

### 挂载SquashFS

```sh
$ mount -v -o loop gentoo.sfs /usr/portage
```
这样得到的是只读的Portage树。可以在此条件下使用Portage管理软件包，但不能更新Portage树。如果要更新，则应该卸载后重新创建/获取SquashFS镜像。
```sh
$ umount -v /usr/portage
$ wget ...
$ mount ...
```

## 可写Portage树

利用[overlay文件系统](https://www.kernel.org/doc/Documentation/filesystems/overlayfs.txt)，可以在节省磁盘空间的前提下实现一个可写的Portage树。
其原理是，配置一个由两个文件系统复合的文件系统：
- 下层：SquashFS，只读。
- 上层：ext4或其他任何可写文件系统。

向这个文件系统中写入内容时，写入上层文件系统。读取时，先看上层是否有对应文件，如果没有，则在下层中查找。
当Portage树更新时，只有变化的部分写入了上层。

要使用overlay文件系统，应启用`CONFIG_OVERLAY_FS`内核选项。

### 创建Overlay

首先准备3个空目录供overlay使用：
```sh
$ mkdir base delta tmp
$ chown portage:portage base delta tmp
```
它们的用处是：SquashFS将被挂载到`base`，overlay中的修改将被写入`delta`，而`tmp`是overlay内部使用的工作目录（须和`delta`位于同一文件系统）。
```sh
$ mount -v -o loop gentoo.sfs base
$ mount -v -t overlay none /usr/portage \
	-o lowerdir=base,upperdir=delta,workdir=tmp
```
这时检查`/usr/portage`的所有者，应为`portage`：
```sh
$ stat -c%U:%G /usr/portage/
portage:portage
```

### 更新Portage树

禁用Portage的硬链接功能，因为[它和overlay不兼容](https://www.gentoo.org/support/news-items/2018-07-11-portage-sync-allow-hardlinks.html)：编辑`/etc/portage/repos.conf/gentoo.conf`：
```ini
[gentoo]
...
sync-allow-hardlinks = no
```

之后就可以像使用普通文件系统一样，更新Portage树：
```sh
$ emerge --sync
$ du -hd0 gentoo.sfs delta/
51M     gentoo.sfs
17M     delta/
```
上面的例子中，一次更新带来了17M的变化。

### 合成新SquashFS

当上层文件系统中的内容较多时，可以考虑合成一个新的SquashFS镜像，并丢弃上层文件系统。
```sh
$ mksquashfs /usr/portage gentoo.sfs.1 -comp xz
$ umount /usr/portage
$ umount base
$ mv gentoo.sfs gentoo.sfs~
$ mv gentoo.sfs.1 gentoo.sfs
$ find delta/ -delete
```

## 后续

可以写init scripts，实现：
- 开机加载overlay；
- 关机卸载overlay；
- `delta/`超过一定大小后，生成新的SquashFS镜像。

这样，对用户来说就是透明的。

即使是只读Portage树，在crontab中写一个每天定时更新SquashFS镜像的任务，也可以起到很好的效果。
