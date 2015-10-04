---
date: 2015-10-04
title: "玩具垃圾收集器"
---

今天我写了一个[玩具垃圾收集器][toygc]，采用_标记-清除([Mark-Sweep][1])_ 算法。

[Wikipedia][1]上的一张图片解释了该过程。{{< figure src="https://upload.wikimedia.org/wikipedia/commons/4/4a/Animation_of_the_Naive_Mark_and_Sweep_Garbage_Collector_Algorithm.gif" >}}

我写的垃圾收集器之所以称作玩具，是因为它采用了朴素实现。每次垃圾收集会打断其他例程的运行，进行一次完整的标记-清除周期。其缺点是容易造成周期性的延迟，影响程序运行的平顺度。

[toygc]: https://github.com/z-rui/toygc
[1]: https://en.wikipedia.org/wiki/Tracing_garbage_collection

<!--more-->

整个实现非常干净，基本上就是标记-清除算法的直接转译。垃圾收集器需要三个外部函数来操作对象：

1. `finalize`是外界对象的终结器(finalizer)，通常它会调用`free`之类的函数来释放对象的内存（垃圾回收器自己不管理内存的分配和释放）。
2. `walk_obj`遍历一个对象的所有直接引用。
3. `walk_root_set`遍历根集对象。

对外暴露两个接口函数：

1. `tgc_collect`执行一次标记-清除。
2. `tgc_add`将新对象加入垃圾收集器的管辖范围。

之后我可能会考虑改进垃圾收集器的算法，例如采用增量收集的方法，减少一次收集过程的延迟。
