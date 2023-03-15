# 常量

常量的声明与变量类似，只不过是使用 ```const``` 关键字。常量的值可以是字符、字符串、布尔值或数值。

注意：常量不能用 ```:=``` 语法声明。

声明并初始化常量：

```go
const s = "gonoob"
const z = 200
```

声明并初始化一组常量：

```go
const (
	a string = "gonoob"
	b        = 100
	c        = true
)

const (
	// 将 1 左移 100 位来创建一个非常大的数字
	// 即这个数的二进制是 1 后面跟着 100 个 0
	Big = 1 << 100
	// 再往右移 99 位，即 Small = 1 << 1，或者说 Small = 2
	Small = Big >> 99
)
```

常量声明后必须初始化，常量的类型可以不指定，一个未指定类型的常量由上下文来决定其类型（Go 会根据常量的值自动推断其类型）。

```go
package main

import "fmt"

const Pi = 3.14

func main() {
	fmt.Printf("Pi = %v\n", Pi)

	const Str = "GoNoob"
	fmt.Println("Hello", Str)

	const Truth = true
	fmt.Println(Truth)
}
```

输出结果：

```
Pi = 3.14
Hello GoNoob
true
```

同时声明多个常量时，如果省略了值则表示和上面一行的值相同：

```go
package main

import "fmt"

const (
	a = 100
	b
	c
)

func main() {
	fmt.Println(a, b, c) // 100 100 100
}
```

## iota

```iota``` 用于表示常量的索引，只能在常量的表达式中使用。```iota``` 在 ```const``` 关键字出现时将被重置为0。

```go
package main

import "fmt"

const a = iota // 0

const (
	b = iota // 0
	c = iota // 1
)

const (
	d = iota // 0
	e        // 1
	f        // 2
	g        // 3
)

func main() {
	fmt.Println(a, b, c, d, e, f, g) // 0 0 1 0 1 2 3
}
```