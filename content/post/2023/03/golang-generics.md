---
date: 2023-03-03T21:07:48-08:00
title: Go语言的泛型
---

长期以来， Go 语言缺乏泛型 (Generics) 的支持。曾经有人认为在 Go 2.0 问世前都不会支持泛型。我很久没有关注 Go 语言的动态了（最后一次写的 Go 项目还没有用 module 机制），今天才发现一年前的 Go 1.18 就已经增加了泛型。

简单地说，泛型可以将同一段代码作用在多种数据类型上。C++ 里的模板 (template) 就是一种泛型编程的方式。下面的 `max` 函数返回两个参数中较大的那个。不论 `a` `b` 是什么类型，只要支持 `<` 运算符，就可以用这个函数。

```cpp
template <typename T>
T max(T a, T b) {
    return a < b ? b : a;
}
// max(1, 2) == 2
// max(2.5, 3.1) == 3.1
```

在 Go 中，一直没有比较好的方法实现这个效果。像 `max` 这种简单的函数，一般可以用的时候现场写一个，只是不太方便。

现在，可以在 Go 写一个泛型函数来实现：

```go
import "golang.org/x/exp/constraints"

func max[T constraints.Ordered] (a, b T) T {
    if a < b {
        return b
    }
    return a
}
// max(1, 2) == 2
// max(2.5, 3.1) == 3.1
// max("abc", "def") == "def"
```

`constraints.Ordered` 包括有能比大小的类型（数值类型和字符串）。目前为止，`constraints` 还没有正式加入标准库中，只能从 `golang.org/x/exp` 这个实验性质的 module 中获取。

<!--more-->

## 原生“泛型”

Go 语言的一些原生功能是自带“泛型”的。例如：

- 运算符 `+` 可以用在整数、浮点数、字符串上。但是，这是编译器写死的；用户不能自己定义运算符的新用法。
- 原生复合类型 (`[]T`, `map[K]V`, `chan T`) 的元素类型是任意的。在 Go 1.18 之前，这也是编译器写死的；用户不能自己定义泛型类型，也不能写针对内置复合类型的泛型函数（像 `len` 这样的）。

## OOP、接口和泛型

如果单纯地说“将同一段代码作用在多种数据类型上”，那么 OOP 中的多态 (polymorphism) 也是一种实现方式，前提是这里的数据类型以 OOP 的方式定义了一套接口，即事先约定了一套方法的集合。

```go
fmt.Fprintln(output, "hello, world")
```

- 如果 `output` 的类型是 `os.File`，那么 hello, world 会被写入文件。
- 如果 `output` 的类型是 `net.Conn`，那么 hello, world 会被传送到网络的另一端。
- 如果 `output` 的类型是 `strings.Builder`，那么 hello, world 会被添加到一个字符串里。
- ……

这里，`Fprintln` 的第一个形式参数的类型是 `io.Writer`。

```go
package io

type Writer interface {
    Write([]byte) (int, error)
}
```

因为 `os.File`, `net.Conn`, `strings.Builder` 都实现了 `Write` 这个方法，它们自动满足 `io.Writer` 这个接口，所以可以被用作 `Fprintln` 的第一个参数。

基于接口的多态一般被认为是比泛型更好的方案。由于 Go 中的接口不需要像 C++ 或 Java 那样由类的继承关系确定，因此灵活性更好。可能是因为接口已经“够用了”，Go 一直没有实现泛型。

接口的主要问题是对于原生类型的支持不好。因为原生类型不是 OOP 的，没有定义方法，所以也就不能用接口。

利用接口，Go 用了一些很聪明的方式绕过泛型：

1. 在容器类型上定义接口。典型的例子是 `sort.Sort`。
   - 这种方式绕过了对容器中的成员类型的讨论，非常聪明。
   - 但是缺点也很明显：对于每个容器类型，用户往往需要自己写一遍 `Len`, `Less`, `Swap` 这三个方法，尽管它们的代码几乎永远是一样的。
2. 用空接口 `interface{}` （现己改名为 `any`）配合 type assertion。典型的例子是 `container/ring`。
   - 这种方式适合完全不关心数据类型的代码（ring 不需要对其中的数据做任何操作）。
3. 空接口配合反射 (reflection)。典型的例子是 `sort.Slice`。
   - 配合可变参数表，还可以实现像 `printf` 这样的函数，可以接收任意类型的一系列值。

## 泛型函数

Go 1.18 的泛型函数可以在作用在任意满足条件的类型上。例如，我想从一个 map 中取出所有键。这在 Go 1.18 之前很难封装成一个函数。有了泛型之后，就可以这么写：

```go
func mapKeys[K comparable, V any](m map[K]V) (r []K) {
        for k := range m {
                r = append(r, k)
        }
        return
}
// mapKeys(map[string]int{"a": 1, "b": 2}) 返回 []string，其中包含 "a" 和 "b" (顺序不确定)
```

这里新引入的语法是函数名后用 `[]` 括起的类型参数表。每个类型参数由名称和约束两部分构成。在这个例子中，`K` 和 `V` 是类型参数名，而 `comparable` 和 `any` 是对应的约束。

为了支持泛型，Go 扩展了接口的语义。原来的接口现在被称为基本接口。广义接口还可以包括类型描述：

```go
type Float interface {
    ~float32 | ~float64
}
```

这里定义的 `Float` 是一个广义接口，它描述了一个类型集合，包括所有实际等价于 `float32` 或 `float64` 的类型。这样，我们就可以定义一个适用于任意浮点数的函数：

```go
func Abs[T Float](x T) T {
    if T < 0 {
        return -T
    }
    return T
}
// Abs(-1.2) == 1.2
type myFloat float32
// Abs(myFloat(-4.5)) == myFloat(4.5)
```

很多时候，泛型编程对类型没有任何约束。这时可以使用空接口 `any`。

```go
func Reduce[R, E any](s []E, init R, f func(R, E) R) R {
    r := init
    for _, v := range s {
        r = f(r, v)
    }
    return r
}
// Reduce([]int{1, 2, 3, 4, 5}, 0, func(a, b int) { return a + b }) == 15
```

和 C++ 的模板不同，在 Go 的泛型函数中，对参数类型做的任何操作都必须满足约束。如果没有约束 (`any`)，那就只能做赋值、传参这种基础操作。

- 如果要测试相等 (`==`, `!=`)，或者用作 map 的键，则必须满足 `comparable` 约束（预定义的特殊约束）。
- 如果要比大小 (`<` 等)，则必须满足 `constraints.Ordered` 约束（包括了所有数值类型和字符串）。

## 泛型类型

这和 C++ 的类型模板是类似的，主要区别有：

- 类型参数有约束。
- 方法不能有额外的参数类型。

## 性能

泛型的实现和 C++ 的模板类似，编译器生成多个版本的函数。在每个版本中，函数直接操作实际的类型，这样就比基于接口的方案少了一些运行时开销。因此，理论上泛型的性能可以更好。

我测试了一下新的用泛型实现的排序函数，确实比标准库性能好一些。（假设它们的算法没有太大差别。）

```go
package main

import (
        "fmt"
        "math/rand"
        "sort"
        "time"

        "golang.org/x/exp/constraints"
        "golang.org/x/exp/slices"
)

const N = 1500000

var (
        intData   = genData(N, rand.Int)
        floatData = genData(N, rand.Float64)
)

func genData[T any](n int, f func() T) []T {
        r := make([]T, n)
        for i := range r {
                r[i] = f()
        }
        return r
}

func benchmarkSorting[T any](sortName string, sortFunc func([]T), data []T) {
        s := slices.Clone(data)
        t := time.Now()
        sortFunc(s)
        fmt.Printf("%s: %s\n", sortName, time.Since(t))
}

func sortSlice[T constraints.Ordered](s []T) {
        sort.Slice(s, func(i, j int) bool { return s[i] < s[j] })
}

func main() {
        fmt.Printf("Benchmarking sorting %d elements...\n", N)

        benchmarkSorting("sort.Ints", sort.Ints, intData)
        benchmarkSorting("sort.Slice [int]", sortSlice[int], intData)
        benchmarkSorting("slices.Sort[int]", slices.Sort[int], intData)

        benchmarkSorting("sort.Float64s", sort.Float64s, floatData)
        benchmarkSorting("sort.Slice [float64]", sortSlice[float64], floatData)
        benchmarkSorting("slices.Sort[float64]", slices.Sort[float64], floatData)
}
```
在我的电脑上，速度几乎相差一倍：
```txt
Benchmarking sorting 1500000 elements...
sort.Ints: 214.250113ms
sort.Slice [int]: 212.454287ms
slices.Sort[int]: 117.284737ms
sort.Float64s: 248.716739ms
sort.Slice [float64]: 213.446413ms
slices.Sort[float64]: 127.760644ms
```

## 局限

暂时记录一些我认为的局限。这些局限在实践中未必重要。

1. 难以特殊化。
   - 泛型的数学函数（例如：`func Sin[T float32|float64](x T) T`）可能还是比较困难。因为泛型是严格地将同一段代码应用在不同类型上，而这里 32 位和 64 位浮点数的 `Sin` 运算恐怕需要采用不同的实现方式。
2. 原生类型仍然有其特殊性。
   - 比如，`constraints.Ordered` 只能表示原生类型（Go 语言中，确实只有原生类型可以比大小）。但是，如果我想实现二叉树这样的数据结构，仅仅用 `Ordered` 是不够的，因为我想存的也可能是其他有序的结构。
   - 基于泛型的 `slices.Sort` 现在也针对原生类型和非原生类型分别写了两种实现。这其实和泛型的初衷（减少代码重复）是相违背的。
3. 一些比较生硬的限制。
   - 例如：方法不能有类型参数；广义接口只能用作类型参数的约束而不能用于声明变量。
   - 目前为止，Go 中的泛型似乎不能很好的和所有其他功能自由地组合使用。Go 在这方面的逻辑似乎是暂且先解决一些已知的难题，对于潜在的新可能性则暂时保留，从实践中获取经验。这种做法比较现实主义，可能也是为了向后兼容性而做出的妥协，但给用户的感觉则是这个产品没有经过充分的打磨。
