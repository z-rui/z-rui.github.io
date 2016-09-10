---
date: 2016-09-11T01:21:00+08:00
title: GNU Privacy Guard
---

GNU Privacy Guard简称GnuPG或GPG，是一款密码学工具。许多Linux系统上自带的`gpg`命令就是它。其最重要的功能是对数据进行加密、解密和签名。此外还带有密钥生成和管理功能。

GPG区别于一般加密工具的地方在于其使用非对称加密机制。加密时，使用收件人的公开密钥，该密钥公开发行可供所有人使用。解密时，使用收件人的私有密钥，该密钥由收件人严密保管。这样，收件人无需和发件人商定密码，就可以安全地通信。

签名是确保信息完整性的办法。使用发件人的私有密钥进行签名，其他人无法伪造，但所有人均可使用发件人的公开密钥进行验证。

我用GPG生成了一个密钥对，公共密钥已经上传到密钥服务器上，大家可以通过

    gpg --recv-keys 0DB87340

下载这个密钥。其指纹为

    2252 7AF4 8AFB 5635 72EC  AC4D 895C 7F46 0DB8 7340

理论上，如果有人要冒充我的身份，只需创造一个虚假的密钥上传到服务器，然后入侵存放这个页面的服务器，纂改上面的密钥编号和指纹。

当然要这么做也有一定难度，总之小心为好，导入密钥的时候务必需要通过可靠的方式（例如，当面）和密钥的持有人进行确认，然后对它签名，以示信任。

<!--more-->

下面是我签名过的一段消息：

```
-----BEGIN PGP MESSAGE-----
Version: GnuPG v1

owEBuAJH/ZANAwACAYlcf0YNuHNAAcuIYgBX1EWJ55y8552b556q5b6X5YOP6ZOc
6ZODCuWwhOWHuumXqueUteiIrOeahOacuueBtQrogLPmnLXnq5blvpflg4/pl6rn
lLUK5ZCs552A5LiA5YiH5Y+v55aR55qE5aOw6Z+zCuWVin7lk4jlk4h+6YKj5bCx
5piv6buR54yr6K2m6ZW/CokCHAQAAQIABgUCV9RFiQAKCRCJXH9GDbhzQKSVD/9y
q70pHZeP5lGLQ6eiTcUMpATu0RzFJC0UoIvDZ5IszI++pu1Vjcm68QPEGwntyBjU
/ovT4sNWC2halawClt+dNb052B4Y8AKvW/37fXcDAlmQgDZ7wTUwdD4YZ0ls3E9g
eHch9rfQEDqFNxNZDLOjweIIrh3FS+0ETxNU3+eCEFrNws7HEhzUyIodrQyBl0s0
zIzGBhS5Ucube67uNlf1Kh+9+M23pRydwVTltcXcQQJa92DGUhn/7hTYyGDCrGoY
nVtIExEMf44KYHmG0/rWZLh14YhVG2mTYset6M3WoIz5oxlSmZIgPuJjueY3JQj4
rifo/+4afDjQ2FkOTIA0O2gG7Jv0MurEPeH/M5AhNSHPkwJTfY9LYSr4Ddgbqtu6
01uFAaBKsMGe3DLgqGKqTfQtMIQC2x2/Qnpty7DBheVQ8wfcnvJfp5TN7g4Sf6ka
fmnVSOU1j/Ob+d4eyZA+A7s9CjZJM9X1vZAHdRNfDHMQVEUn09/rWzXURWHAdmxY
ri/054CyhONXPuACqWLdO/YriuixdGvCnmBYI5o4R4zICrw36I29sMFBb6CYS07f
RWbyh/+kPnkEN93VScbYdSDImFgSHzE7rUTH+Dn85xdXUXtjH5O4fm1eeQOhfi3n
PZjl5h0iiIP+eCaF8t1SYlsZ05ifLxfGQfSPxhAAIQ==
=hmIq
-----END PGP MESSAGE-----
```

将其输入`gpg`命令中即可看到其内容，并验证其完整性。如果没有导入我的公钥，那么是没法看到消息内容的。

在不需要非对称加密机制的地方，也可以采用简单的对称加密方法。发件人和收件人首先通过一种安全的方式商定好密码，然后使用`gpg`的`-c`选项启用对称加密。

对称加密方法不能用于签名。

下面是我用对称加密的方法加密过的消息。出于演示目的，这里直接指出密码是`112233`。
```
-----BEGIN PGP MESSAGE-----
Version: GnuPG v1

jA0EBwMCk0B0ZJrqqplg0rYBtudLvOJZ3SKg/RpbBOgfO8ePrLp2WBElqYc4vgwr
EKyzxVdPSC1uaM9uv9CeOlm3PCGD76GGDvEe5KJAGaSF1+N88VOFo0Nz8CghkyA7
0apG4zuIp43BwDGFi7dMUQuO9nAXUP5AzLZKSzTmM5sVPotzn85mptiNs6u0ozLb
5RthfGA4Ctpu6pj7ngSP3zTxBtBbLeNvh01fiR1OD+ms6hSSZvltTvpQFaWuvXF4
fJ9f1Z6pFg==
=9h3J
-----END PGP MESSAGE-----
```

同样，直接将上面的内容输入`gpg`即可解密。这时`gpg`会询问密码，输入正确的密码之后即可看到消息原文。

