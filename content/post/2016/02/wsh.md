---
date: 2016-02-04T04:04:00+08:00
title: Windows Script Host技巧二则
---

Windows Script Host (WSH)是Windows 98推出的脚本工具，至今已有18年历史，应该属于被淘汰的技术之一。尽管如此，微软的[MSDN](https://msdn.microsoft.com/en-us/library/9bbdkx3k.aspx)和[TechNet](https://technet.microsoft.com/library/ee156603.aspx)上仍维护者相关的文档。

相对于批处理这个更加古老的技术，WSH的优点是它使用VBScript或者JScript语言，语法更加接近通用的程序设计语言，因此更适合处理复杂问题。（参考perl和sh脚本之间的区别。）另一个优点是它可以直接利用所有现成的[COM](https://en.wikipedia.org/wiki/Component_Object_Model)对象，操纵各种应用程序；不过COM也是一个有着20多年历史的老古董技术了。

本文介绍WSH的两个使用功能。其中的代码均为JScript，将代码保存为`.js`文件，则运行脚本和运行一般的程序没有区别。

<!--more-->

## 以隐藏模式运行程序

使用`WshShell.Run`方法可以运行程序，并且设置程序窗口的显示模式，其中就包含隐藏这一种模式。具体做法是

```js
CreateObject("WScript.Shell").Run("daemon.exe", 0);
```

第二个参数0即表示隐藏窗口。也可以设置成其他值，对应不同的模式。

[WshShell](https://msdn.microsoft.com/en-us/subscriptions/aew9yb99(v=vs.84\).aspx)还有其他一些有用的方法，例如`SendKeys`向窗口中发送按键，配合`WshShell.Run`使用，可以实现自动操作某些程序的功能。

## 获取网页内容

使用称为`MSXML2.XMLHTTP`对象可以发送HTTP请求，从而获得网页内容。

```js
var req = WScript.CreateObject("MSXML2.XMLHTTP");
req.open("Get", "http://zhr.io/ip", 0);
req.send();
WScript.Echo(req.responseText);
```
