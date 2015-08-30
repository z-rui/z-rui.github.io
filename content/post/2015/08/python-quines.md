---
date: 2015-08-30
title: "Python Quines"
---

Quine指的是一种特殊的程序，其输出内容等于其自身代码。想写出这样的程序需要动一番脑筋。

如下代码即为Python的quine：

```python
s='s=%s;print(s%%repr(s))';print(s%repr(s))
```

<!--more-->

Berkeley的[CS 61A](http://cs61a.org/)课程作业中有一个challenge problem是用Python写一个quine程序，并且，不能使用字符串的`%`运算符，相对而言更困难一些。

答案如下：

```python
s='s="s="+repr(s)+";"+s;print(s)';s="s="+repr(s)+";"+s;print(s)
```
