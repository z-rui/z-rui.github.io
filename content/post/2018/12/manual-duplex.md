---
date: 2018-12-19T18:02:53-05:00
title: 手动双面打印
---

在不具备自动双面打印功能的硬件/软件上， 可以通过先打印偶数页、
然后将纸张放回， 再打印奇数页的方法实现手动双面打印。
在一些打印机上， 需要将放回的纸张调转方向；
这可以通过打印偶数页时调转页面方向来避免。

在使用 CUPS 的机器上， 可以使用以下脚本实现此功能：
```
#!/bin/bash

# Usage: ./duplex.sh [options] <file>

lp -o page-set=even -o orientation-requested=6 -o outputorder=reverse "$@"
printf "Put the paper in tray and hit Enter"; read
lp -o page-set=odd "$@"
```
<!--more-->
