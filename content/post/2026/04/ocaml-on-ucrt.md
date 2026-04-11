---
date: 2026-04-10T16:23:11-07:00
title: 安装基于 MinGW + UCRT 的 OCaml 工具链
---

OCaml 官方对 [Windows 本地编译提供 Tier 1 的支持](https://ocaml.org/docs/ocaml-on-windows)。办法是安装 Opam 并通过 Opam 安装 OCaml 5.0 以上版本。

通过这个方式安装的 Opam，会自带一个 Cygwin 环境，并在其中安装 MinGW 工具链。这个环境的意义是：

- Cygwin 环境提供 Unix 工具（比如 bash）。
- MinGW 工具链提供编译器 (gcc) 和相关库。

这样编译出来的是原生 Windows 程序（而不需要借助如 WSL 这样的环境）。

但是，有一个小问题：编译出来的可执行文件依赖[古老的 Microsoft C 运行时：msvcrt.dll](https://en.wikipedia.org/wiki/Microsoft_Windows_library_files#MSVCRT.DLL,_MSVCP*.DLL_and_CRTDLL.DLL)。这是 Visual C++ 6.0 时代（1998 年）的产物，有很多历史遗留问题（比如不兼容 C99）。从 Windows 10 开始，微软自家编译器开始使用新的运行时：[UCRT](https://en.wikipedia.org/wiki/Microsoft_Windows_library_files#UCRT)。可能是出于兼容性考虑，MinGW 工具链长期以来用的都是 msvcrt.dll。

实际上，MinGW 工具链也可以配置为使用 UCRT。[Msys2](https://www.msys2.org/docs/environments/) （Cygwin的一种替代方案）就提供这样一种配置。OCaml 官方（实际上指的是 Opam）对 Msys2 环境有[试验性的支持]({{<ref "/post/2026/02/ocaml-on-msys2">}})，尽管仍然用一些不够完善的地方。但是，**目前 Opam 完全不支持使用 UCRT**。即使用 Msys2，它用的也是基于 msvcrt 的环境（这个环境最近已被 Msys2 官方宣布[弃用](https://www.msys2.org/news/#2026-03-15-deprecating-the-mingw64-environment)）。

Opam 项目最近正在[试图增加对 UCRT 的支持](https://github.com/ocaml/opam-repository/issues/29578)。我希望能够有所进展。

在此之前，我找到了一种“欺骗”的办法，让 Opam 用上 UCRT。方法如下：

1. 安装 Msys2 环境。
2. 安装 Opam。
3. 初始化空的 Opam 环境：`opam init --bare`。当询问是否安装 Cygwin 时，选择手动指定 Cygwin/Msys2，并指定 Msys2 的安装路径（默认为 C:\msys64）。
4. （骚操作开始）找一个合适的路径，运行 `opam source msys2`。它会在当前路径下创建一个 `msys2.xxx` 的目录，编辑其中的 `opam` 文件：
```
-  "MINGW64" {msys2-mingw64:installed}
-  "UCRT64" {msys2-ucrt64:installed}
```
同时编辑 `gen_config.sh`，在 case 分支中允许 `UCRT64` 运行：
```
 case "$MSYSTEM" in
-  MINGW32|MINGW64|CLANG32|CLANG64|CLANGARM64|MINGW32|MINGW64)
+  MINGW32|MINGW64|CLANG32|CLANG64|CLANGARM64|UCRT64)
```
5. 在该目录下运行 `opam install . msys2-mingw64`。
   - 为什么要装 `msys2-mingw64` 而不是 `msys2-ucrt64`？因为目前 Opam 的很多 package 规则都硬编码依赖 `msys2-mingw64`。我们通过这种方式“借壳生蛋”。
6. 安装 OCaml：`opam switch set-invariant "ocaml=5.4.1"`。
   - 注意手动处理依赖：安装过程中，如果 Opam 发现缺 gcc 等外部依赖，会询问是否调用 `pacman`。
   - 千万不要选自动安装（即不要选 `1`），否则它会装上 `mingw-w64-x86_64-gcc`（msvcrt 版本）。
   - 正确做法：手动在 Msys2 Shell 里运行 `pacman -S mingw-w64-ucrt-x86_64-gcc`（以及其他对应包），然后回到 Opam 询问界面选择选项 `3` (忽略外部依赖)，这样 Opam 就不会再检查那个软件包是否安装了。

安装完毕后，应该就有一个可用的 OCaml 开发环境了。使用 Msys2 时会遇到的其他问题（比如无法正常安装 ocamlfind）可以参见[这篇文章]({{<ref "/post/2026/02/ocaml-on-msys2">}})。
