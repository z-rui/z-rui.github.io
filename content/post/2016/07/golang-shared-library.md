---
date: 2016-07-23T20:45:00+08:00
title: Go与动态链接库
---

Go从1.5版本开始支持动态链接库。目前官方工具链仅在linux-amd64平台支持动态链接；gccgo则支持更多的平台。

在此之前，Go的所有程序都采用静态链接。一个很简单的"hello, world"程序，因为引入了`fmt`库(这个库进一步依赖其他代码)，所以最终得到的可执行文件也比较大。如果将Go的标准库编译为动态链接库，就可以减小Go生成的可执行文件的大小。

我在Gentoo上安装了Go 1.6.3，要将标准库编译为动态链接库，需要以root权限执行

```sh
go build -buildmode=shared -linkshared std
```

这样就会产生`/usr/lib/go/pkg/linux_amd64/dynlink/libstd.so`文件。这个文件在我的机器上有32MB大，包含了Go标准库的所有内容。

<!--more-->

Go的标准库涵盖的面其实是相当广泛的，包括归档文件格式(zip, tar)、压缩算法、图像文件格式(png等)、高精度运算、http协议等等。这些在C++的标准库中都是不包含的，需要用户安装第三方的程序库。

C++的标准库中很大一部分是算法和数据结构，例如排序、查找、堆栈、队列、搜索树、散列表等。这些是计算机科学的经典内容，C++的标准库实现得相当全面，而且基于模板的实现方式(STL)也非常精致。而Go在这些内容的设计比较粗糙，而更加关注应用层面的内容，如上一段所述。

将Go的标准库编译为动态链接库后，如果要编译其他的程序，比如

```go
package main

import "fmt"

func main() {
	fmt.Println("hello, 世界!")
}
```

只需运行

```sh
go build -linkshared hello.go
```

即可生成可执行文件`hello`，它的大小在我的机器上只有16KB。如果使用静态链接，则可执行文件的大小为2.2MB，因为它包含了标准库中的目标代码。

还可以把自己编写package编译为动态链接库。不过这样做的话就会带来和目前C语言程序同样面对的相依性问题，而且操作起来有些复杂，所以并不太推荐这样做。

要编写自己的package，首先要设置`GOPATH`环境变量。然后创建`$GOPATH/src/<pkg_name>`文件夹，在其中编写package的代码。例如

```go
package hello

import (
	"fmt"
	"io"
)

func SayHello(w io.Writer) {
	fmt.Fprintln(w, "hello, 世界!")
}
```

然后运行

```sh
go install -buildmode=shared -linkshared
```

就会把这个package安装为`$GOPATH/pkg/linux_amd64_dynlink/libhello.so`。编写其他依赖此库的程序时，如

```go
package main

import (
	"hello"
	"os"
)

func main() {
	hello.SayHello(os.Stdout)
}
```

运行

```sh
go build -linkshared
```

即可动态链接刚才生成的`libhello.so`（以及标准库）。Linux中有一个好处，即可以在可执行文件中指定所链接的动态链接库路径（而不仅是名称），这样就不需要把各个`.so`文件所在的目录加入`LD_LIBRARY_PATH`中了。Windows中的程序，也许出于安装路径可以随意更改的原因，不支持这个功能，所加载的dll文件必须在当前目录或者`PATH`目录中。
