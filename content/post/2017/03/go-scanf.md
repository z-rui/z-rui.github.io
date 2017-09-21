---
date: 2017-03-17T16:50:07+08:00
title: Go的fmt效率问题
---

Go 的 `fmt` 包提供了类似 C 标准库中的 scanf 和 printf 的系列输入输出函数。

```
// 输入 N*N 矩阵
var G [N][N]int
for i := 0; i < N; i++ {
	for j := 0; j < N; j++ {
		fmt.Scan(&G[i][j])
	}
}
```

但是 C 的标准输入输出默认是带缓冲的， 而 Go 的不带。
当有大量 I/O 时， 效率较低。 解决的办法是用 `bufio` 中带缓冲的类型。

```
in := bufio.NewReader(os.Stdin)
for i := 0; i < N; i++ {
	for j := 0; j < N; j++ {
		fmt.Fscan(in, &G[i][j])
	}
}
```

同理， 可用 `bufio.Writer` 对输出进行缓冲，
但是记得在程序退出前调用 `Flush()` 清空缓冲区。

<!--more-->

数据量很大时， 使用 `bufio.Scanner` 配合 `strconv` 中的类型转换函数，
可以跳过 `fmt` 的复杂机制， 直接读取数据。
```
// 读取 N*N 矩阵， 均为整数
var G [N][N]int
in := bufio.NewScanner(os.Stdin)
in.Split(bufio.ScanWords)
for i := 0; i < N; i++ {
	for j := 0; j < N; j++ {
		in.Scan()
		value, _ := strconv.Atoi(in.Text())
		G[i][j] = value
	}
}
```
