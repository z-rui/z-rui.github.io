---
date:  2026-02-27T10:41:10-08:00
title: 在Android上编译OCaml和Opam
---

Android上可以通过[Termux](https://termux.dev)获得本地的终端环境。Termux的软件仓库有很多开发工具，不过没有OCaml或者Opam，所以如果需要开发OCaml项目，需要手动编译OCaml编译器和/或包管理器Opam。

我最近尝试在Termux中使用OCaml，遇到了一些小插曲。记录如下。

<!--more-->

## 开端

[Ocaml.org](https://ocaml.org) 官方推荐的安装方式是使用Opam的安装脚本。可惜的是在Android环境中它无法正常工作。这是因为它会检测到Linux环境，并下载预编译的opam-2.5.0-arm64-linux。这个可执行文件在Android上面是无法运行的。因此，只能从源码编译Opam。

要编译Opam，必须有一个可用的OCaml编译器。所以，第一步是从源码编译OCaml。

OCaml的运行时是用C写的，所以第零步是先安装好C编译器和必要的工具链：

```sh
pkg update -y
pkg install -y binutils build-essential clang make git curl unzip libandroid-shmem
```

## 编译OCaml

5.4版本的OCaml代码使用pthread_cancel。这是一个标准的POSIX函数。然而，[Android的C运行时十几年来从未支持过它](https://groups.google.com/g/android-platform/c/Cq05PaLaZbE/m/bmqbcqwTEtcJ)。所以，试图编译这个版本的OCaml就会[遇到问题](https://github.com/ocaml/ocaml/issues/14087)。解决办法有两种：

1. 使用更旧的版本，例如5.3.0。它还没用到pthread_cancel。
2. 使用[git仓库](https://github.com/ocaml/ocaml)中未发布的更新的分支（比如5.5分支已经修复了这个问题）。

不论选择哪一种，使用以下命令编译OCaml并安装：

```sh
./configure --prefix="$PREFIX" --disable-warn-error LDFLAGS=-landroid-shmem
make -j
make install
```

说明：
- `$PREFIX`是Termux自动提供的环境变量。因为Termux是Android上某个沙盒环境，所以它的目录结构不是标准的 /usr/bin 等等，因此需要手动指定。
- 这里`-landroid-shmem`是必需的，因为编译过程中会用到这个库中的函数但是没有正确提供链接选项（我没有追踪是哪个地方用了）。

安装完毕后，可以用`ocaml`命令运行OCaml的REPL解释器，用`ocamlc`或`ocamlopt`把OCaml源文件编译为字节码或机器码。这已经是一个可用的编译环境了，但是我们还是要安装Opam，因为它可以解决两个问题：

1. 管理OCaml的软件包 - Opam可以“一键安装”许多OCaml项目。这对于有外部依赖的项目来说是很必要的。
   - 对比来看，C/C++的项目一般由OS的包管理器来负责安装依赖，而项目的configure/CMake脚本来解决编译时寻找依赖的问题。更现代的语言一般集成了自己的包管理器。比如pip、npm、cargo、Go的模块系统等。
   - Opam的地位大约相当于virtualenv + pip。
2. 安装强大的dune构建系统 - OCaml生态现在大力推广dune，它可以简化OCaml项目的开发流程（以声明式的方式写构建脚本，而不是像Makefile那样要把具体命令写出来）。
   - 对比来看，C/C++的项目各选各的构建系统。更现代的语言一般集成了自己的构建系统（和包管理器深度绑定）。
   - dune有一个愿景，就是自己也集成（项目级别）包管理器的功能，基本取代opam。目前距离这个目标还有一段距离。
   
首先获取[Opam的源码](https://github.com/ocaml/opam)。

Opam的编译过程会用到`gmake`，但是在Termux环境中，它叫`make`。所以，要手动创建一个符号链接：
```sh
(cd "$PREFIX/bin"; ln -s make gmake)
```
然后运行
```sh
./configure --prefix="$PREFIX" --with-vendored-deps
make -j
make install
```

这样就在系统上安装了Opam。

## 在Opam分支上重新编译OCaml编译器

Opam有所谓的“分支”功能，即一个完全隔离的环境（类似Python的virtualenv）。每个分支可以安装不同的OCaml编译器和第三方库。

一般来说，使用Opam至少要创建一个分支。第一次运行Opam时，需要运行`opam init`命令。它会做基本的初始化，并创建一个名为`default`的分支，并在这个分支内重新编译一次编译器。

但是，如前所述，5.4版本的编译器在Android上有问题，所以编译会失败。要解决这个问题，需要使用Opam的高级功能：pin。pin允许用户自行安装某个软件包，自己提供源码、并自行设置它的编译选项，等等。

我们可以用
```sh
opam init --bare
```
让它不要自动编译OCaml编译器。初始化完毕后，运行：
```sh
opam pin edit ocaml-variants
```

它会打开一个编辑器。在这个编辑器中，找到`build: [`，并在这个列表的末尾（`]`之前）补上：
```c
"--disable-warn-error"
"LDFLAGS=-landroid-shmem"
```
然后，到文件的末尾，把`src: "..."`行改成
```c
src: "git+file://..."
```
其中`...`是刚才编译ocaml时，源码所在的路径（需要使用绝对路径，即以`/`开头；`file:`后面总共3个`/`）。

退出编辑器。这时Opam会自动解算需要编译/安装的内容，按Y确认。

这个过程结束后，Opam分支内的OCaml编译器就设置完毕了。然后就可以用`opam install ...`命令安装各自软件包。比如：
```sh
opam install dune
```
安装dune构建工具。根据我的尝试，像domainslib, eio这些包（及它们的依赖），在Android环境下安装都没有问题。

## 尾声

在Termux上编译OCaml编译器，我参考了[这篇文献](https://github.com/omeyenburg/unison-for-termux)。它只涉及了OCaml编译器（而且是5.3.0版本），而没有涉及Opam，所以遇到的困难比我少很多，主要是LDFLAGS。

编译和安装Opam基本上没有遇到太多问题。

在Opam的分支内编译OCaml编译器，和自己从源码编译OCaml编译器遇到的问题是一样的。关键在于怎么在Opam中“定制”编译步骤。这里要感谢[Google Gemini](https://gemini.google.com/)，是它帮我找到了`opam pin ocaml-variants`的办法。
