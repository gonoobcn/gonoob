# 函数

Go 中使用 ```func``` 来声明函数。

函数的基本结构：

```go
func 函数名(参数名1 参数类型1, 参数名2 参数类型2, ... ) 返回值类型 {
	return 返回值
}
```

## 声明函数

函数可以没有参数或接受多个参数，参数的类型在参数名之后：

```go
package main

import "fmt"

func add(x int, y int) int {
	return x + y
}

func main() {
	v := add(12, 13)
	fmt.Println(v)
}
```

输出结果：

```
25
```

## 可省略的参数类型

当连续两个或多个函数的参数类型相同时，除最后一个类型以外，其它都可以省略：

```go
func add(x, y int) int {
	return x + y
}
```

在以上代码中，参数 ```x``` 和 参数 ```y``` 都是 ```int``` 类型。

## 多返回值

函数可以返回任意数量的返回值：

```go
package main

import "fmt"

func swap(x, y string) (string, string) {
	return y, x
}

func main() {
	a, b := swap("gonoob", "hello")
	fmt.Println(a, b)
}
```

输出结果：

```
hello gonoob
```

在以上代码中，```swap``` 函数返回了两个字符串。

## 命名返回值

Go 函数的返回值可被命名，它们会被视作定义在函数顶部的变量。如果函数中的 ```return``` 语句没有返回值，那么默认会返回已命名的返回值。

```go
package main

import "fmt"

func split(sum int) (x, y int) {
	x = sum * 4 / 9
	y = sum - x
	return
}

func main() {
	fmt.Println(split(10)) // 4 6
}
```

在以上代码中，```split``` 函数中的 ```return``` 相当于 ```return x, y```。

## 函数值

函数也是值，它们可以像其它值一样传递。函数值可以用作函数的参数或返回值：

```go
package main

import (
	"fmt"
	"math"
)

func compute(fn func(float64, float64) float64) float64 {
	return fn(3, 4)
}

func main() {
	hypot := func(x, y float64) float64 {
		return math.Sqrt(x*x + y*y)
	}
	fmt.Println(hypot(5, 12)) // 13

	fmt.Println(compute(hypot)) // 5
	fmt.Println(compute(math.Pow)) // 81
}
```

## 函数的闭包

Go 函数可以是一个闭包。闭包是一个函数值，它引用了其函数体之外的变量。该函数可以访问并赋予其引用的变量的值，换句话说，该函数被这些变量绑定在一起。

函数 ```adder``` 返回一个闭包，每个闭包都被绑定在其各自的 ```sum``` 变量上：

```go
package main

import "fmt"

func adder() func(int) int {
	sum := 0
	return func(x int) int {
		sum += x
		return sum
	}
}

func main() {
	pos, neg := adder(), adder()
	for i := 0; i < 10; i++ {
		fmt.Println(
			pos(i),
			neg(-2*i),
		)
	}
}
```

## ```init``` 函数

在 Go 语言中，```init``` 函数是一个特殊的函数，该函数用于包的初始化操作，```init``` 函数会在 ```main``` 函数执行前执行。

声明一个 ```init``` 函数：

```go
func init() {

}
```

```init``` 函数的特点：

- ```init``` 函数可以用于初始化包中的变量或其他初始化相关的操作
- 每个 ```package``` 中可以有一个或多个 ```init``` 函数
- 每个 ```.go``` 文件中也可以有一个或多个 ```init``` 函数
- ```init``` 函数没有参数和返回值，无法直接调用
- ```init``` 函数会在 ```main``` 函数执行前执行

### ```init``` 函数的调用顺序

如果在多个 ```package``` 中声明 ```init``` 函数，则 ```init``` 函数会按照包的导入顺序来执行。

在以下代码中，分别在 ```package a```、```package b```、```package c``` 三个包中声明 ```init``` 函数，然后在 ```main``` 包中导入这三个包。

::: code-group

```go [a.go]
package a

import "fmt"

func init() {
	fmt.Println("init package a")
}
```

```go [b.go]
package b

import "fmt"

func init()  {
	fmt.Println("init package b1")
}

func init()  {
	fmt.Println("init package b2")
}
```

```go [c.go]
package c

import "fmt"

func init() {
	fmt.Println("init package c")
}
```

```go [main.go]
package main

import (
	"fmt"
    _ "gonoob.cn/a"
    _ "gonoob.cn/b"
    _ "gonoob.cn/c"
)

func main() {
    fmt.Println("package main")
}
```

:::

输出结果：

```
init package a
init package b1
init package b2
init package c
package main
```

如果多个 ```package``` 相互嵌套，比如 ```main``` 包导入 ```a``` 包，```a``` 包导入 ```b``` 包，```b``` 包导入 ```c``` 包，则 ```init``` 函数的执行顺序为 ```c``` -> ```b``` -> ```a```。

::: code-group

```go [main.go]
package main

import (
	"fmt"
    _ "gonoob.cn/a"
)

func main() {
    fmt.Println("package main")
}
```

```go [a.go]
package a

import (
	"fmt"
	_ "gonoob.cn/b"
)

func init()  {
	fmt.Println("init package a")
}
```

```go [b.go]
package b

import (
	"fmt"
    _ "gonoob.cn/c"
)

func init()  {
	fmt.Println("init package b1")
}

func init()  {
	fmt.Println("init package b2")
}
```

```go [c.go]
package c

import "fmt"

func init()  {
	fmt.Println("init package c")
}
```

:::

运行结果：

```
init package c
init package b1
init package b2
init package a
package main
```

如果在同一个 ```package``` 的多个 ```.go``` 文件中声明 ```init``` 函数，则根据文件名字符串来比较，按 ```从小到大``` 的顺序调用各文件中的 ```init``` 函数。

::: code-group

```go [a.go]
package hello

import (
	"fmt"
)

func init()  {
	fmt.Println("init a.go")
}
```

```go [aaa.go]
package hello

import (
	"fmt"
)

func init()  {
	fmt.Println("init aaa.go")
}
```

```go [bc.go]
package hello

import (
	"fmt"
)

func init()  {
	fmt.Println("init bc.go")
}
```

```go [bz.go]
package hello

import (
	"fmt"
)

func init()  {
	fmt.Println("init bz.go")
}
```

```go [cc.go]
package hello

import "fmt"

func init()  {
	fmt.Println("init cc.go")
}
```

```go [main.go]
package main

import (
	"fmt"
    _ "gonoob.cn/hello"
)

func main() {
    fmt.Println("package main")
}
```

:::

运行结果：

```
init a.go
init aaa.go
init bc.go
init bz.go
init cc.go
package main
```



