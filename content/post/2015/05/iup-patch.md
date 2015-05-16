---
date: 2015-05-15T15:02:00+08:00
title: "IUP的两个补丁"
categories:
  - computing
  - hack
tags:
  - IUP
  - GUI
  - 补丁
---

我正计划编写一个[SQLite的图形前端](https://www.github.com/z-rui/yasf)。为此我选择[IUP](http://webserver2.tecgraf.puc-rio.br/iup/)作为图形界面库。

IUP具有如下特点符合我的口味，因此被我选作图形界面库：

1. C语言接口。GUI常用面向对象的方法处理，所以使用C++的图形界面库更常见。IUP使用C语言实现了一套类型系统，并提供了非常简洁的C语言接口。
2. 跨平台。使用IUP编写的图形界面程序可以运行在Windows和UNIX操作系统上。
2. 相对GTK+、Qt等大型的图形界面库(Qt应该算作综合工具箱)，体积小巧便于发布，配置起来也比较容易。
3. 控件虽然没有其他工具箱来得丰富，但基本够用。文档比较详细。

IUP目前最新的版本是3.14。它仍然存在一些问题，所以我在这里给出两个补丁。

第一个问题是IupSplit中嵌入的IupMatrix有时候会崩溃。我向IUP的作者反映了这个问题。作者回复说他已经在SVN中修复。所以，IUP的下一个版本中将不会再有这个问题。

补丁：（来自[SVN](http://sourceforge.net/p/iup/iup/2897/tree//trunk/iup/srccontrols/matrix/iupmat_edit.c?diff=53d9aa9d0910d44ffbc10019:2896)）

```patch
--- a/srccontrols/matrix/iupmat_edit.c
+++ b/srccontrols/matrix/iupmat_edit.c
@@ -678,6 +678,7 @@
   IupSetAttribute(ih->data->texth, "VALUE",  "");
   IupSetAttribute(ih->data->texth, "VISIBLE", "NO");
   IupSetAttribute(ih->data->texth, "ACTIVE",  "NO");
+  IupSetAttribute(ih->data->texth, "FLOATING", "IGNORE");
 
 
   /******** DROPDOWN *************/
@@ -696,4 +697,5 @@
   IupSetAttribute(ih->data->droph, "MULTIPLE", "NO");
   IupSetAttribute(ih->data->droph, "VISIBLE", "NO");
   IupSetAttribute(ih->data->droph, "ACTIVE",  "NO");
-}
+  IupSetAttribute(ih->data->droph, "FLOATING", "IGNORE");
+}
```

(注：截至写本文时，本站用于语法高亮的脚本[Highlight.js](https://highlightjs.org/)的版本为8.5，它似乎不能正确显示上述代码：最后一部分被作为注释处理了。)

第二个问题是IupTree控件的`NAME`属性不起作用。即，无法使用`IupGetDialogChild`找到相应的控件。这是出于兼容性考虑，IupTree控件的`NAME`属性默认为是`TITLE`属性的同义词。

IUP的下一个版本也许还会保留这个行为，但我并不需要它。所以我也许应该修改IUP的代码，取消掉这样的兼容行为。要修改这个行为，需要修改平台相关的驱动代码。因为我目前在Windows下开发，所以我只修改了Windows相关的文件。其他平台做类似修改即可。

```patch
--- a/src/win/iupwin_tree.c
+++ b/src/win/iupwin_tree.c
@@ -3060,7 +3060,6 @@
   iupClassRegisterAttributeId(ic, "DEPTH",  winTreeGetDepthAttrib,  NULL, IUPAF_READONLY|IUPAF_NO_INHERIT);
   iupClassRegisterAttributeId(ic, "KIND",   winTreeGetKindAttrib,   NULL, IUPAF_READONLY|IUPAF_NO_INHERIT);
   iupClassRegisterAttributeId(ic, "PARENT", winTreeGetParentAttrib, NULL, IUPAF_READONLY|IUPAF_NO_INHERIT);
-  iupClassRegisterAttributeId(ic, "NAME",   winTreeGetTitleAttrib,  winTreeSetTitleAttrib, IUPAF_NO_INHERIT);
   iupClassRegisterAttributeId(ic, "TITLE",  winTreeGetTitleAttrib,  winTreeSetTitleAttrib, IUPAF_NO_INHERIT);
   iupClassRegisterAttributeId(ic, "CHILDCOUNT", winTreeGetChildCountAttrib, NULL, IUPAF_READONLY|IUPAF_NO_INHERIT);
   iupClassRegisterAttributeId(ic, "COLOR", winTreeGetColorAttrib, winTreeSetColorAttrib, IUPAF_NO_INHERIT);
```

我不是很确定修改IUP的源码以使它符合我的需求是不是明智的选择。

一旦修改了IUP的源码，那么基本上来说，我应该将IUP静态链接到我的程序中。因为编译出一个修改过的动态链接库似乎并没有什么意义（不能被其他程序共用）。

还有一种做法是绕过IUP现在提供的`IupGetDialogChild`机制，另外建立一套获取窗体中的控件的机制。IUP传统的机制是为每一个控件指定一个全局的标识符。像`IupSetAtt`这样的便捷函数就是在鼓励使用这样的机制（也是为了兼容旧的LED窗体描述机制）。这种做法固然简单有效，但我觉得很丑陋。只可惜像IupTree这样的控件出于兼容性考虑还不能支持`NAME`属性。

我认为每个窗体有一个单独的名字空间才是正确的做法。不知道是不是出于同样的观点，IUP现在提供了一套新的机制，就是使用控件的`NAME`属性。这样，控件的标识符的作用域就仅限于其所在的窗体。使用`IupGetDialogChild(dlg, "NAME")`这样的函数调用来获得`dlg`窗体中标识符为`NAME`的控件。

<!--more-->

