---
date: 2015-10-20
title: "免xauth进行X11转发"
---

之前谈到如何通过SSH进行X11转发。其中有一个前提，即主机B上要装有`xauth`。事实上，X11的转发和普通的TCP端口转发并无差别。如果主机B中未安装`xauth`，则需要用户手动设置远程TCP端口转发，并设置主机B的`DISPLAY`变量：
```sh
hostA$ ssh -R6010:localhost:6000 user@hostB
hostB$ export DISPLAY=':10'
```

在图形界面客户端例如PuTTY或者[Bitvise Tunnelier]中，也都有远程TCP端口转发的设置。

在主机A中，仍然启动X服务器的TCP监听。不同的是，由于没有`.Xauthority`认证文件，需要用`xhost`允许本地主机(即SSH客户端)连接X服务器。在Cygwin中：
```sh
hostA$ startxwin -- :0 -multiwindow -listen tcp &
hostA$ xhost +localhost
```

具体采用哪一种办法见仁见智。建议在主机B中安装`xauth`，这样可以免去手工设置远程端口转发和`DISPLAY`变量。

[Bitvise Tunnelier]: https://www.bitvise.com/tunnelier

<!--more-->
