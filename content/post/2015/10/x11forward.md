---
date: 2015-10-18
title: "通过SSH进行X11转发"
---

```
          Host A                                         Host B           
+--------------------+------+                +------+--------------------+
| +------------+     |      |                |      |     +------------+ |
| |            |     | SSH  |    Internet    | SSH  |     |            | |
| |            +-----> Port +----------------> Port +----->            | |
| |            <-----+      <----------------+      <-----+            | |
| |    SSH     |     |      |                | 22   |     |    SSH     | |
| |   Client   |     +------+                +------+     |   Server   | |
| |            <-----+      |                |      <-----+            | |
| |            +----->      |                |      +----->            | |
| +------------+     | X11  |                | X11  |     +------------+ |
| +------------+     | Port |                | Port |     +------------+ |
| | X11 Server +----->      |                |      +-----> X11 Client | |
| |            <-----+ 6000 |                | 6010 <-----+            | |
| +------------+     |      |                |      |     +------------+ |
+--------------------+------+                +------+--------------------+
```

<!--more-->

SSH协议除了可以进行普通的TCP端口转发以外，还可以进行X11的转发。如图所示，要在主机B上运行X客户程序，并在主机A上显示出来，需要在主机A上启动X服务器，然后通过SSH的X11转发功能来实现。

在主机B上，首先需要安装`xauth`，其次需要修改`/etc/ssh/sshd_config`，确保启用了

    X11Forwarding yes

在主机A上需要安装SSH客户端和X服务器。在Windows中，可以分别使用[PuTTY](http://www.chiark.greenend.org.uk/~sgtatham/putty/)和[Cygwin/X](http://x.cygwin.com/)。

要进行X11转发，首先在主机A上开启X服务器。在cygwin的命令行中运行

```sh
$ startxwin -- :0 -multiwindow -listen tcp &
```

需要加上`-listen tcp`参数，这是因为最新的X服务器默认不再监听TCP/IP连接，从而可能造成无法转发X11的问题。参见[Cygwin/X FAQ](http://x.cygwin.com/docs/faq/cygwin-x-faq.html#q-xserver-nolisten-tcp-default)。

启动了X服务器后，通知区域里会显示X窗口系统的图标。接下来启动PuTTY：

{{< figure src="/media/putty-1.png" >}}

填写好主机B的连接信息后，转到**Connection -> SSH -> X11**界面，勾选**Enable X11 forwarding**，地址填写`127.0.0.1:0.0`。最后还要记得选择`.Xauthority`文件，它位于cygwin的home目录中。可以在cygwin的命令行中运行

```sh
$ cygpath -aw ~/.Xauthority
```

找到这个文件的路径。

{{< figure src="/media/putty-2.png" >}}

然后就可以启动SSH连接了。登录成功之后，在主机B上运行X客户程序，即可在主机A中显示。

{{< figure src="/media/putty-3.png" link="/media/putty-3.png" >}}

如果认证失败，请检查主机B中是否有`~/.ssh/rc`或者`/etc/ssh/sshrc`脚本。若有，务必确保它们正确调用了`xauth`程序，通常可以用如下的片段来实现。

```
if read proto cookie && [ -n "$DISPLAY" ]; then
        if [ 'echo $DISPLAY | cut -c1-10' = 'localhost:' ]; then
                # X11UseLocalhost=yes
                echo add unix:`echo $DISPLAY | cut -c11-` $proto $cookie
        else
                # X11UseLocalhost=no
                echo add $DISPLAY $proto $cookie
        fi | xauth -q -
fi
```

