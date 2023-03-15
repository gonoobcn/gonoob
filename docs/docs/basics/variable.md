# 变量

本章节我们将学习 Go 语言的变量、变量的初始化、短变量声明和变量的零值。

## 声明变量

Go 中 使用 ```var``` 来声明一个变量，变量名在前面，类型在后面。

声明变量的基本格式：

```go
var 变量名 变量类型
var 变量名1, 变量名2, 变量名3 变量类型	// 声明多个变量为同一类型
```

```go
package main

import "fmt"

var a, b, gonoob bool

func main() {
	var i int
	fmt.Println(i, a, b, gonoob)
}
```

## 变量的初始化

变量初始化的基本格式：

```go
var 变量名 变量类型 = 值
var 变量名1, 变量名2 变量类型 = 值1, 值2	// 初始化多个变量
```

在变量声明时，可以包含初始值，每个变量对应一个。如果初始化值已存在，则可以省略类型；变量会从初始值中获得类型。

```go
package main

import "fmt"

var a string = "gonoob"
var i, j int = 1, 2

func main() {
	var c, d, str = true, false, "Hello GoNoob!"
	fmt.Println(a, i, j, c, d, str)
}
```

在以上代码中，Go 会自动识别变量 ```c```、```d```、```str``` 的类型。

## 短变量声明

在函数中，简洁赋值语句 ```:=``` 可在类型明确的地方代替 ```var``` 声明，```:=``` 表示变量的声明与初始化，只能在函数内部使用。

在函数外，只能使用 ```var``` 来声明变量，```:=``` 结构不能在函数外使用。

```go
package main

import "fmt"

str := "hello"	// 这是错误的用法

func say() {
	s := "gonoob"	// 这是正确的
	fmt.Println(s)
}

func main() {
	var i, j int = 1, 2
	k := 3
	fmt.Println(i, j, k)
}
```

## 变量的零值

没有明确初始值的变量声明会被赋予它们的零值。

零值是：

- 数值类型为 ```0```
- 布尔类型为 ```false```
- 字符串为 ```""```（空字符串）
- 引用类型为 ```nil```

```go
package main

import "fmt"

func main() {
	var a int
	var b float64
	var c bool
	var d string
    var e chan int
	fmt.Printf("%v %v %v %v %v\n", a, b, c, d, e)
}
```

输出结果：

```
0 0 false "" <nil>
```