# 类型

本章节我们将学习 Go 语言的类型、类型转换和类型推导。

## 基本类型

Go 语言中的基本类型有：

| 类型 | 长度（字节）| 默认值 | 说明 |
| -- | ---- | ---- | -- |
| bool | 1 | false | 布尔 |
| int、uint | 4或8 | 0 | 整型 |
| int8、uint8 | 1 | 0 | 整型 |
| int16、uint16 | 2 | 0 | 整型 |
| int32、uint32 | 4 | 0 | 整型 |
| int64、uint64 | 8 | 0 | 整型 |
| uintptr | 4或8 | 0 | 无符号整型 |
| float32 | 4 | 0.0 | 浮点型 |
| float64 | 8 | 0.0 | 浮点型 |
| byte | 1 | 0 | uint8 |
| rune | 4 | 0 | int32 |
| string |  | “” | 字符串 | 
| complex64 | 8 | | 复数 |
| complex128 | 16 | | 复数 |
| array | | | 数组 |
| struct | | | 结构体 |

其中，```int```、```uint``` 和 ```uintptr``` 在 32 位系统上通常为 32 位，在 64 位系统上则为 64 位。当你需要一个整数值时应当使用 ```int``` 类型，除非你有特殊的理由使用固定大小或无符号的整数类型。

## 引用类型

Go 语言中的引用类型有：

| 类型 | 默认值 | 说明 |
| -- | ---- | -- |
| slice | nil | 切片 |
| map | nil | 字典 |
| chan | nil | 通道 |
| interface | nil | 接口 |
| function | nil | 函数 |

## 类型转换

表达式 ```T(v)``` 将值 ```v``` 转换为类型 ```T```。

一些关于数值的转换：

```go
var i int = 30
var f float64 = float64(i)
var u uint = uint(f)
```

或者，更加简单的形式：

```go
i := 25
f := float64(i)
u := uint(f)
```

与 ```C``` 不同的是，Go 没有隐式转换，当不同类型的变量之间赋值时需要显式转换。

```go
package main

import (
	"fmt"
	"math"
)

func main() {
	var x, y int = 3, 4
	var f float64 = math.Sqrt(float64(x*x + y*y))
	var z uint = uint(f)
	fmt.Println(x, y, z)
}
```

## 类型推导

在声明一个变量而不指定其类型时（即使用不带类型的 ```:=``` 语法或 ```var =``` 表达式语法），变量的类型由右值推导得出。

当 ```:=``` 右边的值声明了类型时，新变量的类型与其相同：

```go
var a int
b := a // b 也是一个 int
```

不过当右边的值包含未指明类型的数值常量时，新变量的类型就可能是 ```int```、```float64``` 或 ```complex128``` 了，这取决于常量的精度：

```go
i := 42           // int
f := 3.142        // float64
g := 0.867 + 0.5i // complex128
```

输出变量 ```v``` 的类型：

```go
package main

import "fmt"

func main() {
	v := 666
	fmt.Printf("v is of type %T\n", v)
}
```