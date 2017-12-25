---
date: 2017-12-24T21:57:00-05:00
title: 自制 IBus 输入法
---

IBus (Intelligent Input Bus) 是 Unix 系统上可用的一种输入法框架。我尝试自制了一个简单的“查表”式的输入法，类似于用微软的IME编辑器生成的古老输入法。

{{<figure src="/media/ibus-1.png" caption="IBus输入法界面">}}

重要的参考文档是[IBus参考手册](http://ibus.github.io/docs/ibus-1.5/index.html)。但是这个文档的质量非常一般。[官方Wiki上的条目](https://github.com/ibus/ibus/wiki/Develop)给了一个 Python 和 C 的开发模版，虽然没有解释 IBus 的相关概念，但是值得参考。

另外，还可以用 Vala 编写 IBus 组件，这样可以省去不少 GObject 的繁文缛节。只需继承 `IBus.Engine` 即可。最重要的是重载它的 `process_key_event` 方法，拦截键盘输入，并更新候选项。候选项采用 `IBusLookupTable` 维护。

<!--more-->

关于输入法的本地安装。默认情况下， IBus 从 `/usr/share/ibus/component` 读取 IBus 组件列表。没有 root 权限的用户可采用如下方案：编写自定义的 xml 文件（内容参考现有的组件的即可），放在 `~/.local/share/ibus/component/` 下（也可以选择其他位置）。然后运行
```
IBUS_COMPONENT_PATH=~/.local/share/ibus/component:/usr/share/ibus/component ibus write-cache
```
然后重启 IBus 即可。

关于查表输入法的实现。用链表储存所有（编码，词条）对，按编码排序。用三叉搜索树建立索引，记录每个编码对应的词条的子链表，实现快速查寻。用伸展 (splaying) 策略维护搜索树的平衡。至于 IBusLookupTable，我一次只在其中放 10 个候选项。当用户按“=”​“-”号翻页时，清空这个表，再填充下一页的内容。

要实现“词组联想”功能（一个很鸡肋的功能）也很容易：用三叉搜索树建立另一个索引，以词组为键，就可以高效地进行前缀匹配。
