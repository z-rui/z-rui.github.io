---
date: 2015-10-14
title: "诡异的OpenSSH行为"
---

最近几天在FreeBSD上尝试搭建SSH服务器，主要是想利用SSH的[端口转发](https://en.wikipedia.org/wiki/Port_forwarding)功能。然而却遇到了一个诡异的问题，耗费了我许多时间。

我希望限制每个用户的登录数，即，同一用户在同一时间只能在一个主机上连接SSH服务器进行端口转发。网上查了老半天，基本上没有什么有用的信息。

据说Linux中可以使用`/etc/security/limits.conf`来限制用户的登录数量。也有资料说这个办法只能限制shell的登录数量，对不需要shell的端口转发连接无效。

`limits.conf`通过`pam_limits`模块产生作用，FreeBSD无此模块，因此我无法尝试。受此启发，我找到了`pam_exec`模块，允许我在开启/关闭会话的时候执行任意的脚本。这样我只需要自己编写一个脚本来检测是否存在重复登录即可。

```sh
#!/bin/sh
# file: pam_session.sh
# prevent multiple ssh connections from a single user

USER_SESSION="/var/run/${PAM_USER}.ssh_session"
LOG="/var/log/ssh_session.log"

echo `date` "$PAM_USER" "$PAM_RHOST" "$PAM_SM_FUNC" >> $LOG

if [ "$PAM_SM_FUNC" = "pam_sm_open_session" ]; then
        if [ -f $USER_SESSION ]; then
                echo BLOCKED by `cat $USER_SESSION` >> $LOG
                exit 1
        fi
        echo "$PAM_RHOST" > $USER_SESSION
elif [ "$PAM_SM_FUNC" = "pam_sm_close_session" ]; then
        rm -f $USER_SESSION
fi
```

保存为`/etc/ssh/pam_session.sh`，然后在`/etc/pam.d/sshd`中添加一行
```
session	required	pam_exec.so	/etc/ssh/pam_session.sh
```
即告成功。诡异的事现在发生了：我在一个主机上登录SSH，然后在另一个主机上也登录SSH，但后者只进行端口转发，不登录shell。结果后者也登录成功了！

我在Windows上用[Bitvise的SSH客户端](https://www.bitvise.com/ssh-client)，连接并没有中断。

我在Android上用[ConnectBot](https://play.google.com/store/apps/details?id=org.connectbot)连接，在不登录shell的情况下，连接也没有中断。

我的脚本工作正常，PAM的Session应该是认证失败的(系统日志中有`Permission Denied`的记录)。也就是说，OpenSSH忽略了PAM会话认证失败而继续保持着连接！

沿着这样的思路，我尝试了其他各种方法，试图阻断重复的SSH连接，均告失败。绝望的我去查看了OpenSSH的源代码，想搞清楚作者为什么指定了这样的行为。

看到[第1052行](https://github.com/openssh/openssh-portable/blob/8408218c1ca88cb17d15278174a24a94a6f65fe1/auth-pam.c#L1052)：

```c
	sshpam_err = pam_open_session(sshpam_handle, 0);
	if (sshpam_err == PAM_SUCCESS)
		sshpam_session_open = 1;
	else {
		sshpam_session_open = 0;
/*1052*/	disable_forwarding();
		error("PAM: pam_open_session(): %s",
		    pam_strerror(sshpam_handle, sshpam_err));
	}
```

瞬间释然，其实OpenSSH在PAM会话认证失败的情况下是会禁用端口转发的。至于连接为什么没有中断，我不知道。

且不提OpenSSH的行为是否合适，仅仅因为连接没有中断就判定端口转发仍然可以进行，显然我陷入了惯性思维的陷阱，为此还浪费了大量的时间。应当引以为戒。

<!--more-->
