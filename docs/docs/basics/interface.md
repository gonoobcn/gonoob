# 接口

在 Go 中，接口也是一种类型，接口是由一组方法签名定义的集合。接口类型的变量可以保存任何实现了这些方法的值。

声明接口的基本格式：

```go
type 接口名称 interface {
    方法名1( 参数列表1 ) 返回值列表1
    方法名2( 参数列表2 ) 返回值列表2
	方法名3( 参数列表3 ) 返回值列表3
    …
} 
```

## 接口与隐式实现

类型通过实现一个接口的所有方法来实现该接口。隐式接口从接口的实现中解耦了定义，这样接口的实现可以出现在任何包中，无需提前准备。

因此，也就无需在每一个实现上增加新的接口名称，这样同时也鼓励了明确的接口定义。

举个例子：

```go
package main

import "fmt"

type I interface {
	M()
}

type T struct {
	S string
}

// 此方法表示类型 T 实现了接口 I，我们无需显式声明。
func (t T) M() {
	fmt.Println(t.S)
}

func main() {
	var i I = T{"gonoob"}
	i.M()
}
```

## 接口值

接口也是值。它们可以像其它值一样传递，接口值可以用作函数的参数或返回值。

在内部，接口值可以看做包含值和具体类型的元组：

```go
(value, type)
```

接口值保存了一个具体底层类型的具体值。

接口值调用方法时会执行其底层类型的同名方法。

```go
package main

import (
	"fmt"
	"math"
)

type I interface {
	M()
}

type T struct {
	S string
}

func (t *T) M() {
	fmt.Println(t.S)
}

type F float64

func (f F) M() {
	fmt.Println(f)
}

func main() {
	var i I

	i = &T{"Hello"}
	describe(i)
	i.M()

	i = F(math.Pi)
	describe(i)
	i.M()
}

func describe(i I) {
	fmt.Printf("(%v, %T)\n", i, i)
}
```

## ```nil``` 接口值

```nil``` 接口值既不保存值也不保存具体类型。

为 ```nil``` 接口调用方法会产生运行时错误，因为接口的元组内并未包含能够指明该调用哪个具体方法的类型。

```go
package main

import "fmt"

type I interface {
	M()
}

func main() {
	var i I
	describe(i)
	i.M()
}

func describe(i I) {
	fmt.Printf("(%v, %T)\n", i, i)
}
```

## 空接口

指定了零个方法的接口值被称为空接口：

```go
interface{}
```

空接口可保存任何类型的值。（因为每个类型都至少实现了零个方法。）

空接口被用来处理未知类型的值。例如，```fmt.Print``` 可接受类型为 ```interface{}``` 的任意数量的参数。

```go
package main

import "fmt"

func main() {
	var i interface{}
	describe(i)

	i = 42
	describe(i)

	i = "hello"
	describe(i)
}

func describe(i interface{}) {
	fmt.Printf("(%v, %T)\n", i, i)
}
```

## 类型断言

类型断言提供了访问接口值底层具体值的方式。

```go
t := i.(T)
```

该语句断言接口值 ```i``` 保存了具体类型 ```T```，并将其底层类型为 ```T``` 的值赋予变量 ```t```。

若 ```i``` 并未保存 ```T``` 类型的值，该语句就会触发一个恐慌。

为了判断一个接口值是否保存了一个特定的类型，类型断言可返回两个值：其底层值以及一个报告断言是否成功的布尔值。

```go
t, ok := i.(T)
```

若 ``i`` 保存了一个 ```T```，那么 ```t``` 将会是其底层值，而 ok 为 true。

否则，ok 将为 false 而 ```t``` 将为 ```T``` 类型的零值，程序并不会产生恐慌。

请注意这种语法和读取一个映射时的相同之处。

```go
package main

import "fmt"

func main() {
	var i interface{} = "hello"

	s := i.(string)
	fmt.Println(s)

	s, ok := i.(string)
	fmt.Println(s, ok)

	f, ok := i.(float64)
	fmt.Println(f, ok)

	f = i.(float64) // 报错(panic)
	fmt.Println(f)
}
```

## 类型选择

类型选择是一种按顺序从几个类型断言中选择分支的结构。

类型选择与一般的 ```switch``` 语句相似，不过类型选择中的 ```case``` 为类型（而非值）， 它们针对给定接口值所存储的值的类型进行比较。

```go
switch v := i.(type) {
case T:
    // v 的类型为 T
case S:
    // v 的类型为 S
default:
    // 没有匹配，v 与 i 的类型相同
}
```

类型选择中的声明与类型断言 ```i.(T)``` 的语法相同，只是具体类型 ```T``` 被替换成了关键字 ``type``。

此选择语句判断接口值 ```i``` 保存的值类型是 ```T``` 还是 ```S```。在 ```T``` 或 ```S``` 的情况下，变量 ```v``` 会分别按 ```T``` 或 ```S``` 类型保存 ```i``` 拥有的值。在默认（即没有匹配）的情况下，变量 ```v``` 与 ```i``` 的接口类型和值相同。

```go
package main

import "fmt"

func do(i interface{}) {
	switch v := i.(type) {
	case int:
		fmt.Printf("Twice %v is %v\n", v, v*2)
	case string:
		fmt.Printf("%q is %v bytes long\n", v, len(v))
	default:
		fmt.Printf("I don't know about type %T!\n", v)
	}
}

func main() {
	do(21)
	do("hello")
	do(true)
}
```