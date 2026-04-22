---
date: 2026-04-21T22:01:52-07:00
title: OCaml 网络编程经历
---

最近尝试用 OCaml 写了一些有实用价值的程序。其中包括一个基于 Eio 的 SOCKS5 协议服务器。

Eio 使用 OCaml 5.0 新引入的 Effects 特性，允许使用者以类似同步 I/O 的方法写代码，但是底层使用异步 I/O 并利用 Effects 实现用户态调度，来实现并发 I/O 的效果。这和 Go 的并发编程类似（但是 Goroutine 是抢占式调度，并且默认在多个系统线程上调度，而 Eio 的调度器默认只在一个系统线程里）。

因为 OCaml 5.0 对 Windows 也提供 Tier 1 支持。这意味着我写的程序天然可以跨平台。我在 Windows 编译我的项目，确实顺利通过了，并且似乎也可以正常工作。

然而，这只是故事的开始……

## CPU占用率过高

当服务器接受一个连接以后，CPU 占用率达到了单核 100%。经过排查，这是 Eio 的 Windows 后端实现中的一个 bug。它在调度器的轮询实现中，没有正确管理需要被轮询的文件描述符列表，导致某个套接字在可写状态但并没有代码在等待写入时，会陷入轮询该套接字的无限循环中。

## Unix.select 抛出异常

解决了 CPU 占用过高问题，接下来遇到的问题是服务器偶尔会抛出异常。内容是
```
select.c: original file handle not found
```
经过排查，这是 OCaml 官方 Unix 库的 Windows 实现的一个 bug。Unix 库和 Windows 程序有什么关系呢？简单地说，因为 Windows 没有标准的 Unix 接口，OCaml 官方的 Unix 库在 Windows 上实现了一个“模拟”层，这样上层应用就可以继续使用 Unix 接口进行编程（特别是网络编程）。Eio 的 Windows 后端在内部就是使用 Unix.select 进行轮询的。不幸的是，select_win32.c 实现的 Unix.select 逻辑比较复杂，导致里面藏有若干个 bug。

1. 实现中涉及到文件描述符 (fd) 到句柄 (handle) 的对应。但是这个对应的实现是错误的，导致有时候会找不到和 handle 对应的 fd（就会抛出上述异常），或者返回一个错误的 fd。
2. Windows 的套接字编程中的 select 只支持套接字。但是 Unix.select 理论上还要能处理普通文件、管道和控制台等在 Unix 中均被统一为文件的对象。所以，Windows 中的 Unix.select 用 WaitForMultipleObjects 模拟了这个功能。但是 WaitForMultipleObjects 有一个非常诡异的限制：一次只能等待最多 64 个对象。为了支持 select 更多个对象，这个库内部做了一个线程池。但是，对线程池的链表操作有一个 bug，导致可能会丢失链表中的节点。这会造成内存泄漏和丢失文件描述符。

因为 Eio 的 Windows 后端使用的就是 Unix.select，所以毫无疑问也有可能遇到这些问题。这些问题是偶发的，因为当传入的全是套接字时， Unix.select 会直接使用 Winsock 提供的 select 函数，也就不会遇到上述问题了。

## 总结

select_win32.c 的主要逻辑很多年前就写好了。这么长时间以来，没有人发现这些问题，说明：

1. 测试覆盖度极小（事实上，我发现对 Unix.select 几乎没有测试）。
2. 用户基数极小，估计没有多少人在 Windows 上用 OCaml。

这么看来，OCaml 连官方的库都远没有达到“生产可用”的程度。那就更不必说第三方库了。OCaml 声称对 Windows 的“Tier 1”支持，也值得打个问号。在我看来远没有达到生产可用的程度。相比之下，更活跃（或者说背后有充足商业支持）的语言，比如 Go 或者 Rust，在这些方面应该会好很多。
