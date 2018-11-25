---
date: 2018-10-01T11:57:33-04:00
title: WiFi 和睡眠
---

本文要讨论的不是人的睡眠而是计算机的睡眠 (suspend) 操作。

我的 Gentoo 系统上有一个困扰了我很长一段时间的问题：
当计算机从睡眠状态中恢复时， 我的无线网络连接往往会失效。
这时， 我需要手动重置无线网络服务：

```
/etc/init.d/net.wlp4s0 restart
```

究其原因， 是 DHCP 客户端未能在唤醒时重新申请新的 lease， 
路由表也没有被更新。

看 `/etc/wpa_supplicant/wpa_cli.sh`， 这个脚本在无线网络连接/断开时会被触发，
但是并不会触发 DHCP 客户端重新获取 lease。
所以一个简单的解决方案是在这里加上 DHCP 的操作。 查看 udhcpc 的帮助，
SIGUSR2 和 SIGUSR1 分别可以用于释放和重新申请 lease：
<!--more-->
```diff
--- a/wpa_cli.sh	2018-10-01 12:13:22.537918716 -0400
+++ b/wpa_cli.sh	2018-09-28 14:47:48.542782202 -0400
@@ -23,14 +23,19 @@
 	exit 1
 fi
 
+PIDFILE="/var/run/udhcpc-${INTERFACE}.pid"
+[ -f "$PIDFILE" ] && PID=`cat "$PIDFILE"`
+
 case ${ACTION} in
 	CONNECTED)
 		EXEC="${EXEC} start"
+		[ -z "$PID" ] || /bin/kill -SIGUSR1 $PID
 		;;
 	DISCONNECTED)
 		# Deactivated, since stopping /etc/init.d/net.wlanX
 		# stops the network completly.
 		EXEC="false ${EXEC} stop"
+		[ -z "$PID" ] || /bin/kill -SIGUSR2 $PID
 		;;
 	*)
 		logger -t wpa_cli "Unknown action ${ACTION}"
```
这个 patch 有一点点 dirty， 因为硬编码了 udhcpc 客户端。
如果我改用其他 DHCP 客户端的话， 这个脚本还要做相应的修改。

Works like a charm!
