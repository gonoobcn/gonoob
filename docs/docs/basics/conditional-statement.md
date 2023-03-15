# 条件语句

Go 中的条件语句有 ```if``` 语句和 ```switch``` 语句。

## if 语句

```if``` 语句的基本格式：

```go
if 表达式 {
	// ...
}

if 初始化语句; 表达式 {
	// ...
}
```

```if else``` 语句的基本格式：

```go
if 表达式 {
	// ...
} else {
	// ...
}

if 初始化语句; 表达式 {
	// ...
} else if {
	// ...
} else {
	// ...
}
```
```if``` 语句与 ```if else``` 语句的特点：

- ```if``` 语句可以在条件表达式前执行一个简单的语句，该语句声明的变量作用域仅在 ```if``` 之内。
- 在 ```if``` 的简短语句中声明的变量同样可以在任何对应的 ```else``` 块中使用。
- ```if``` 语句的初始化语句和表达式外无需小括号 ```( )``` ，初始化语句和表达式之间使用 ```;``` 隔开。

举个例子：

```go
package main

import (
	"fmt"
	"math"
)

func pow(x, y, z float64) float64 {
	if x < 0 {
		return x
	}

	if a := 2.5; x < 0 {
		return a
	}

	if v := math.Pow(x, y); v < z {
		return v
	}

	if v := math.Pow(x, y); v < z {
		return v
	} else {
		fmt.Printf("%g >= %g\n", v, z)
	}
	// 这里开始就不能使用 v 了
	return z
}

func main() {
	fmt.Println(pow(3, 4, 12), pow(3, 3, 20))
}
```

输出结果：

```
81 >= 12
27 >= 20
12 20
```

## switch 语句

```switch``` 是编写一连串 ```if - else``` 语句的简便方法。它会运行第一个值等于条件表达式的 ```case``` 语句，只运行选定的 ```case```，而非之后所有的 ```case```。 

Go 默认会在每个 ```case``` 后面提供了所需的 ```break``` 语句。除非以 ```fallthrough``` 语句结束，否则分支就会自动终止。```switch``` 的 ```case``` 无需为常量，且取值不必为整数。

```switch``` 语句的基本格式：

```go
switch 初始化语句（可选）; 变量 {
case 常量或表达式:
	// ...
case 常量或表达式:
	// ...
default:
	// ...
}
```

举个例子：

```go
package main

import "fmt"

func main() {
	switch ch := "a"; ch {
	case "a":
		fmt.Println("a")
	case "b":
		fmt.Println("b")
	default:
		fmt.Printf("%s.\n", ch)
	}
}
```

输出结果：

```
a
```

### switch 的求值顺序

switch 的 ```case``` 语句从上到下顺次执行，直到匹配成功时停止。

举例：

```go
switch i {
case 0:
case f():
}
```

在以上代码中，当 ```i==0``` 时 f 函数不会被调用。

举个例子：

```go
package main

import (
	"fmt"
	"time"
)

func main() {
	fmt.Println("When's Saturday?")
	today := time.Now().Weekday()
	switch time.Saturday {
	case today + 0:
		fmt.Println("Today.")
	case today + 1:
		fmt.Println("Tomorrow.")
	case today + 2:
		fmt.Println("In two days.")
	default:
		fmt.Println("Too far away.")
	}
}
```

输出结果：

```
When's Saturday?
Tomorrow.
```

### 没有条件的 switch

没有条件的 ```switch``` 同 ```switch true``` 一样。

这种形式能将一长串 ```if-then-else``` 写得更加清晰。

```go
package main

import (
	"fmt"
	"time"
)

func main() {
	t := time.Now()
	switch {
	case t.Hour() < 12:
		fmt.Println("Good morning!")
	case t.Hour() < 17:
		fmt.Println("Good afternoon.")
	default:
		fmt.Println("Good evening.")
	}
}
```

输出结果：

```
Good morning!
```

