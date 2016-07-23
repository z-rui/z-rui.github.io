---
date: 2016-07-22T23:16:00+08:00
title: Go语言中的生成器
---

生成器(generator)是Python的一个语法特性，例如生成平方数序列的生成器可以写成

```
def squares(n):
	i = 1
	while i <= n:
		yield i * i
		i += 1
```

若要求前10个平方数之和，只需

```
>>> sum(squares(10))
385
```

在Go语言中，使用goroutine配合channel，可以实现相同的功能：

```
func squares(n int) <-chan int {
	ch := make(chan int)
	go func() {
		for i := 1; i <= n; i++ {
			ch <- i * i
		}
		close(ch)
	}()
	return ch
}
```

然后可以用for语句遍历：

```
sum := 0
for term := range squares(10) {
	sum += term
}
```

<!--more-->

偶尔也可以写一些无穷的生成器，例如产生不限长度的斐波纳契数列：

```go
func fibonacci() <-chan *big.Int {
	ch := make(chan *big.Int)
	go func() {
		x := big.NewInt(1)
		y := big.NewInt(0)
		for {
			ch <- x.Add(x, y)
			ch <- y.Add(x, y)
		}
	}()
	return ch
}
```

求其前N项的和，这时候不能用for语句，而要用channel的接收表达式：

```go
sum := big.NewInt(0)
ch := fibonacci()
for i := 1; i <= N; i++ {
	sum.Add(sum, <-ch)
}
```

但是有一个问题，那就是负责生成数据的goroutine永远不会终止，其中分配的资源也不会被释放。所以这种写法只适合运行一次即完毕的程序，或者生成器始终在工作状态的程序。如果要用于长期运行的程序，需要创建和销毁多个这样的生成器，就会引发资源问题。要解决这个问题，或者只使用含有终止条件的生成器，或者增加新的机制使得goroutine可以和外部通信以便知道终止的时机，但是那样写出来的代码就比较复杂，不像Python那样漂亮了。

值得一提的是，用Python写永不终止的生成器，生成器也会在适当的时间终止。

```
def fibonacci():
	x, y = 1, 0
	while True:
		x += y
		y += x
		yield x
		yield y

sum = 0
ch = fibonacci()
for i in range(100):
	sum += next(ch)
del ch # 对应的生成器将终止
```

顺便一说，一个语言提供的语法特性对程序中使用的方法有重大的影响。例如，协程操作比较困难的语言中，用户就很有可能采用回调函数来实现类似的功能。例如标准C语言就不提供任何协程操作（只有一个调用栈），所以广为使用的办法是回调函数。

在C++中，成员函数比较难作为普通的回调函数来使用。但是STL提供了大量好用的容器，所以往往采用这样的办法：先计算出所有的数据，保存在一个容器中，然后由外部函数依次处理这些数据，最后释放容器。这是最传统的做法，它的缺陷也很明显：空间开销大、无法生成无穷序列等。

再例如，Lua的闭包功能非常强大，所以常常用闭包实现“依次处理序列数据”的功能。

```lua
function fibonacci(callback)
	local x, y = 1, 1
	while callback(x) and callback(y) do
		x = x + y
		y = x + y
	end
end

local sum = 0
local n = 5 -- Lua 的数值类型精度有限，只求前5项的和
fibonacci(function(term)
	sum = sum + term
	n = n - 1
	return n > 0
end)
```

虽然Lua也有`coroutine`包用于协程操作，但是一方面写起来麻烦，另一方面运行开销也更大。

另一方面，Python中函数字面量——也就是lambda表达式——的函数体只能是表达式而不能是语句，因此功能很弱。如果要实现功能完整的闭包，则要单独声明而不能嵌入表达式中。所以闭包在Python中的应用相对较少了。但是，Python的生成器非常好用，所以相关的功能全部使用生成器来实现了。在Python 3中，很多内置函数（如`range`）都是生成器。

Go语言的话两方面比较兼顾，goroutine很好用，函数字面量的功能也不输。用回调函数(闭包)来遍历斐波纳契数列，方法如下：

```go
func fibonacci(callback func(*big.Int) bool) {
	x := big.NewInt(1)
	y := big.NewInt(1)
	for callback(x) && callback(y) {
		x.Add(x, y)
		y.Add(x, y)
	}
}
```

求前100项的和：

```
n := 100
sum := big.NewInt(0)
fibonacci(func(term *big.Int) bool {
	sum.Add(sum, term)
	n--
	return n > 0
})
```
