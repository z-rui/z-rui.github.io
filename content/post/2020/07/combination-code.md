---
date: 2020-07-24T21:41:11-07:00
title: 组合的编码/解码
---

n 元集合 {0,1,...,n-1} 的 k 元子集共有 \\(n\choose k\\) 个。
设某个子集的元素为 \\(c\_1\<c\_2\<\\cdots\<c\_k\\)， 则该子集可以[编码][1]为
\\[{c\_k\\choose k} + \\cdots + {c\_2\\choose 2} + {c\_1\\choose 1}.\\]

此编码最小值为 0， 最大值为
\\[{n-1\\choose k} + \\cdots + {n-k+1\\choose 2} + {n-k\\choose 1} = {n\\choose k}-1.\\]
上式的证明： 在两边加上 \\(n-k\\choose 0\\) 并使用恒等式 \\({n\\choose k}={n-1\\choose k-1}+{n-1\\choose k}\\)。

<!--more-->

{{<gist "z-rui" "305b0bf6d0be773a3a05b330d34c9238">}}

[1]: https://en.wikipedia.org/wiki/Combinatorial_number_system
