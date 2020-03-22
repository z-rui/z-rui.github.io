---
date: 2020-03-21T21:37:03-07:00
title: 近代科技：IRC
---

IRC (Internet Relay Chat) 是 90 年代提出的互联网协议。
用户可以连接到 IRC 上与其他用户进行即时通讯。多个用户可以在一个频道（类似聊天室的概念）交流，也可以进行两用户间的一对一交流。
协议很简陋，只能传输文本内容。即便如此，这个协议体现了早期互联网标准化和开放的特点：任何人可以根据标准，制作自己的实现；不同的实现可以互联互通。

和IRC有关的更多信息可以参考 http://www.irchelp.org/。

<!--more-->

## UnrealIRCd

[UnrealIRCd](https://www.unrealircd.org/) 是一个流行的IRC服务器软件。
在一个具有公网IP的主机上安装并运行UnrealIRCd，便拥有了自己的IRC服务器。
默认设置下，IRC端口为6667（明文）和6697（TLS）。
要使用TLS，最好有一个真正的证书，而不是自带的自签名证书（部分客户端不支持）。
证书可以购买，或者使用[Let's Encrypt](http://letsencrypt.org/)
（[操作教程](https://www.unrealircd.org/docs/Using_Let%27s_Encrypt_with_UnrealIRCd)）。

（注：在UnrealIRCd-4.2.4.1中，配置文件中的`tls-options`应为`ssl-options`。）

## 客户端

我知道的客户端如下：

- [HexChat](https://hexchat.github.io/) (Linux/Windows, GUI)
- [Revolution IRC](https://play.google.com/store/apps/details?id=io.mrarm.irc) (Android)
- [Irssi](https://irssi.org/), [WeeChat](https://weechat.org/) (Terminal)

此外还有网页版的IRC客户端：[KiwiIRC](https://kiwiirc.com/nextclient/)。

## ZNC

IRC是一个特别简陋的协议。只有用户连接到IRC服务器时，才能收到各频道的消息。
一旦断开连接，该频道中新产生的消息就会被错过，下次再次连接时也无法看见。
也就是说，IRC服务器不保存消息记录。

可以使用[ZNC](https://znc.in/)实现IRC“始终在线”的效果。
原理是，ZNC程序作为客户端连接到IRC服务器。
当用户连接到ZNC时，ZNC的角色是IRC代理，在用户和IRC服务器之间传递消息。
当用户断开到ZNC的连接时，ZNC继续和IRC服务器保持连接，并储存所有新收到的消息。
当用户再次连接时，将收到这些错过的消息。

我在我的 Raspberry Pi 上运行着ZNC。

## 附言

当今的互联网商业化程度更高，个别商业公司垄断了绝大多数的在线服务。
用户不得不在这些商业化的平台上注册账号，并接受其使用条款（例如，允许服务提供商收集个人信息并用于商业用的）。
用户被迫使用提供的Web或客户端软件，它们几乎都是专有的。
不同商家的服务一般也很难互联互通。在一些存在不正当竞争的市场，
甚至存在不同的平台之间互相屏蔽的现象。
这样的现状给少数公司提供了大量的利润，也使它们垄断了虚拟世界的“话语权”。
不一定是一件好事。
