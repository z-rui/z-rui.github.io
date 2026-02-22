---
date: 2026-02-21T23:11:30-08:00
title: 在Msys2环境下配置OCaml
---

OCaml 是一个函数式编程语言．它现在[支持Windows](https://ocaml.org/docs/ocaml-on-windows)．可以直接用
```sh
winget install OCaml.opam
```
安装Opam包管理器．然后运行`opam init`即可初始化构建环境（它需要git，也可以通过`winget`来安装）．默认的初始化方式会管理一个Cygwin环境．根据安装过程中的提示，也可以采用系统上已有的Cygwin/Msys2环境．我把我的配置过程和遇到的问题记录在这里．

总的来说，我认为官方支持的内置Cygwin环境遇到的问题会少很多．

<!--more-->

## 设置 OPAMROOT
Opam是OCaml的包管理器．它会管理编译环境，包括编译器和各种OCaml库和工具．在Windows上，它默认把所有文件安装到 `%USERPROFILE%\AppData\Local\opam`．其实也可以放在别的地方（比如[Dev Drive](https://learn.microsoft.com/en-us/windows/dev-drive/)上）．办法是：在运行`opam init`之前，在Windows环境变量设置中，定义`OPAMROOT=D:\opam`，则opam的文件都会安装到`D:\opam`．运行`opam init`前需要重启命令行，使环境变量更改生效．

## Ocamlfind 不兼容问题
这个问题在官方的内置Cygwin方案下是不存在的．当用自己的Msys2环境时，opam会禁止在ocaml 5.0以上的编译器中安装ocamlfind．如果用`opam pin --edit`强行取消这一限制，则会发生错误．目前的解决办法是使用
```sh
opam pin add -n ocamlfind git+https://github.com/dra27/ocamlfind.git#win-fixes
```
安装一个带补丁的ocamlfind．注意：因为ocamlfind是一些包的依赖，所以如果安装其他包也出现了Package conflict错误（Opam认为必须 ocaml < 5.0），可以考虑是不是ocamlfind出了问题．

## Raylib 构建错误

构建Raylib时会提示找不到`x86_64-w64-mingw32-ar.exe`．我不知道有没有哪个包提供这个程序，但是安装`mingw-w64-x86_64-toolchain`之后只能找到`x86_64-w64-mingw32-gcc-ar.exe`．我做了一个简单的复制，解决了这个问题．