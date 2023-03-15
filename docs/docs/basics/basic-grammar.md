# 基本语法

本章节我们将学习 Go 语言的基本语法。

## 源文件

Go 程序代码必须写在以 ```.go``` 为后缀的源文件中，如 ```hello.go```、```main.go```。

## 命名规范

Go 的包、变量、常量、自定义类型、函数、方法、接口的命名方式遵循以下规则：

- 首字符可以是任意的 Unicode 字符或者下划线
- 剩余字符可以是 Unicode 字符、下划线、数字
- 字符长度不限
- 不能使用 Go 语言的关键字和保留字命名

```txt
_hello ✅
Hello ✅
12world // ❌ 不能使用数字开头
var // ❌ 不能使用 Go 语言的关键字
```

## 关键字

| 关键字 | 说明 |
| --- | -- |
| package | 声明包文件 |
| import | 导入其它 package |
| var | 声明变量 |
| const | 声明常量 |
| type | 声明类型 |
| struct | 定义结构体类型 |
| interface | 定义接口 |
| func | 声明函数或方法 |
| return | 从函数返回 |
| defer | 声明延迟调用，在函数退出之前执行 |
| go | 创建一个协程（goroutine） |
| break | 退出当前整个循环 |
| continue | 退出循环中的某次循环 |
| break | 退出循环 |
| if、else | 条件语句 |
| switch、case、fallthrough、default | 流程控制 |
| goto | 无条件地转移到指定的行 |
| select | 用于监听 I/O 操作 |
| chan | 用于声明通道（chan）类型 |
| map | 用于声明 map 类型 |
| for、range | 用于遍历array、slice、map、channel数据 |

## 保留字

| 分类 | 保留字 | 说明 |
| -- | --- | -- |
| Constants | true  false  iota  nil | 常量 |
| Types | int、int8、int16、int32、int64、uint、uint8、uint16、uint32、uint64、uintptr、float32、float64、complex128、complex64、bool、byte、rune、string、error | 基本类型 |
| Functions | make、len、cap、new、append、copy、close、delete、complex、real、imag、panic、recover | 内置函数 |

## 可见性

Go 的变量、常量、自定义类型、函数、方法、接口的可见性遵循以下规则：

- 声明在函数内部，只能在函数内部使用
- 声明在函数外部，如果首字母大写，则对所有包可见；如果首字母小写，则只对当前包可见（包内所有 ```.go``` 文件都可见）

## 注释

```txt
// 单行注释

/* 这是一个多行注释
   这是一个多行注释 */
```

## 主函数

在 Go 中，```main``` 函数是程序执行的默认入口函数：

```go
package main

func main() {
    // 函数体
}
```

注意：```main``` 函数所在的包必须是 ```package main```，该函数不能有参数和返回值。

## 程序的基本结构

Go 语言程序的基本结构包含：

- 包声明
- 导入包
- 全局变量或常量
- 函数或方法（包含局部变量、流程控制语句、表达式）
- 代码注释

```go
// 包声明
package main

// 导入包
import "fmt"

// 声明全局常量和变量
const a int = 20
var b string = "gonoob"

// 自定义函数
func Hello() {
   var c int = 30 // 局部变量
   fmt.Printf("a = %v, b = %s, c = %v \n", a, b, c) // 调用 fmt 包中的函数
}

// main 函数，程序执行的入口函数
func main() {
   Hello()
   fmt.Println("Hello, GoNoob!")
}
```