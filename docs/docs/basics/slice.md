# 切片

数组的大小都是固定的，而切片的长度是动态可变的。在实际开发中，切片比数组更常用。类型 ```[]T``` 表示一个元素类型为 ```T``` 的切片。

切片通过两个下标来界定，即一个上界和一个下界，二者以冒号分隔：

```go
a[low : high]
```

它会选择一个半开区间，包括第一个元素，但排除最后一个元素。

以下表达式创建了一个切片，它包含 ```a``` 中下标从 1 到 3 的元素：

```go
a[1:4]
```

```go
package main

import "fmt"

func main() {
	arr := [6]int{2, 3, 5, 7, 11, 13}

	var s []int = arr[1:4]
	fmt.Println(s)	// 3, 5, 7
}
```

## 切片是数组的引用

切片并不存储任何数据，它只是描述了底层数组中的一段。更改切片的元素会修改其底层数组中对应的元素。与它共享底层数组的切片都会观测到这些修改。

```go
package main

import "fmt"

func main() {
	names := [4]string{
		"Rose",
		"Jack",
		"GoNoob",
		"Mara",
	}
	fmt.Println(names)	// [Rose Jack GoNoob Mara]

	a := names[0:2]
	b := names[1:3]
	fmt.Println(a, b)	// [Rose Jack] [Jack GoNoob]

	b[0] = "GGG"
	fmt.Println(a, b)	// [Rose GGG] [GGG GoNoob]
	fmt.Println(names)	// [Rose GGG GoNoob Mara]
}
```

## 切片的初始化

数组的初始化：

```go
[3]bool{true, true, false}
```

下面这样则会创建一个和上面相同的数组，然后构建一个引用了它的切片：

```go
[]bool{true, true, false}
```

```go
package main

import "fmt"

func main() {
	q := []int{2, 3, 5, 7, 11, 13}
	fmt.Println(q)

	r := []bool{true, false, true, true, false, true}
	fmt.Println(r)

	s := []struct {
		i int
		b bool
	}{
		{2, true},
		{3, false},
		{5, true},
		{7, true},
		{11, false},
		{13, true},
	}
	fmt.Println(s)
}
```

## 切片的默认行为

在进行切片时，你可以利用它的默认行为来忽略上下界。

切片下界的默认值为 0，上界则是该切片的长度。

对于数组

```go
var a [10]int
```

来说，以下切片是等价的：

```go
a[0:10]
a[:10]
a[0:]
a[:]
```

```go
package main

import "fmt"

func main() {
	s := []int{2, 5, 8, 7, 33, 26}

	s = s[1:4]
	fmt.Println(s)	// [5 8 7]

	s = s[:2]
	fmt.Println(s)	// [5 8]

	s = s[1:]
	fmt.Println(s)	// [8]
}
```

## 切片的长度与容量

切片拥有 ```长度``` 和 ```容量```。

切片的长度就是它所包含的元素个数。

切片的容量是从它的第一个元素开始数，到其底层数组元素末尾的个数。

切片 ```s``` 的长度和容量可通过表达式 ```len(s)``` 和 ```cap(s)``` 来获取。

你可以通过重新切片来扩展一个切片，给它提供足够的容量。

```go
package main

import "fmt"

func main() {
	s := []int{2, 3, 5, 7, 11, 13}
	printSlice(s)

	// 截取切片使其长度为 0
	s = s[:0]
	printSlice(s)

	// 拓展其长度
	s = s[:4]
	printSlice(s)

	// 舍弃前两个值
	s = s[2:]
	printSlice(s)
}

func printSlice(s []int) {
	fmt.Printf("len=%d cap=%d %v\n", len(s), cap(s), s)
}
```

## nil 切片

切片的零值是 ```nil```。

```nil``` 切片的长度和容量为 0 且没有底层数组。

```go
package main

import "fmt"

func main() {
	var s []int
	fmt.Println(s, len(s), cap(s))
	if s == nil {
		fmt.Println("nil!")
	}
}
```

## 用 make 创建切片

切片可以用内置函数 ```make``` 来创建，这也是创建动态数组的常用方式。

```make``` 函数会分配一个元素为零值的数组并返回一个引用了它的切片：

```go
a := make([]int, 5)  // len(a)=5
```

要指定它的容量，需向 ```make``` 传入第三个参数：

```go
b := make([]int, 0, 5) // len(b)=0, cap(b)=5
```

```go
b = b[:cap(b)] // len(b)=5, cap(b)=5
b = b[1:]      // len(b)=4, cap(b)=4
```

```go
package main

import "fmt"

func main() {
	a := make([]int, 5)
	printSlice("a", a)

	b := make([]int, 0, 5)
	printSlice("b", b)

	c := b[:2]
	printSlice("c", c)

	d := c[2:5]
	printSlice("d", d)
}

func printSlice(s string, x []int) {
	fmt.Printf("%s len=%d cap=%d %v\n",
		s, len(x), cap(x), x)
}
```

## 切片的切片

切片可包含任何类型，甚至包括其它的切片。

```go
package main

import (
	"fmt"
	"strings"
)

func main() {
	// 创建一个井字板（经典游戏）
	board := [][]string{
		[]string{"_", "_", "_"},
		[]string{"_", "_", "_"},
		[]string{"_", "_", "_"},
	}

	// 两个玩家轮流打上 X 和 O
	board[0][0] = "X"
	board[2][2] = "O"
	board[1][2] = "X"
	board[1][0] = "O"
	board[0][2] = "X"

	for i := 0; i < len(board); i++ {
		fmt.Printf("%s\n", strings.Join(board[i], " "))
	}
}
```

## 向切片追加元素

为切片追加新的元素是种常用的操作，为此 Go 提供了内置的 ```append``` 函数。

```go
func append(s []T, vs ...T) []T
```

```append``` 的第一个参数 ```s``` 是一个元素类型为 ```T``` 的切片，其余类型为 ```T``` 的值将会追加到该切片的末尾。

```append``` 的结果是一个包含原切片所有元素加上新添加元素的切片。

当 ```s``` 的底层数组太小，不足以容纳所有给定的值时，它就会分配一个更大的数组。返回的切片会指向这个新分配的数组。

```go
package main

import "fmt"

func main() {
	var s []int
	printSlice(s)

	// 添加一个空切片
	s = append(s, 0)
	printSlice(s)

	// 这个切片会按需增长
	s = append(s, 1)
	printSlice(s)

	// 可以一次性添加多个元素
	s = append(s, 2, 3, 4)
	printSlice(s)
}

func printSlice(s []int) {
	fmt.Printf("len=%d cap=%d %v\n", len(s), cap(s), s)
}
```

## Range

```for``` 循环的 ```range``` 形式可遍历切片或map。

当使用 ```for``` 循环遍历切片时，每次迭代都会返回两个值。第一个值为当前元素的下标，第二个值为该下标所对应元素的一份副本。

```go
package main

import "fmt"

var pow = []int{1, 6, 8, 10, 16, 32, 55, 128}

func main() {
	for i, v := range pow {
		fmt.Printf("2**%d = %d\n", i, v)
	}
}
```

可以将下标或值赋予 ```_``` 来忽略它。

```go
for i, _ := range pow
for _, value := range pow
```

若你只需要索引，忽略第二个变量即可。

```go
for i := range pow
```

```go
package main

import "fmt"

func main() {
	pow := make([]int, 10)
	for i := range pow {
		pow[i] = 1 << uint(i) // == 2**i
	}
	for _, value := range pow {
		fmt.Printf("%d\n", value)
	}
}
```
