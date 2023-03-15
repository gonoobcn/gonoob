# 数组

类型 ```[n]T``` 表示拥有 ```n``` 个 ```T``` 类型的值的数组。

声明一个数组变量：

```go
var a [10]int
```

在以上代码中，会将变量 ```a``` 声明为拥有 10 个整数的数组。

数组的长度是其类型的一部分，因此数组不能改变大小。这看起来是个限制，不过没关系，Go 提供了更加便利的方式来使用数组。

```go
package main

import "fmt"

func main() {
	var a [2]string
	a[0] = "hello"
	a[1] = "gonoob"
	fmt.Println(a[0], a[1])
	fmt.Println(a)

	arr := [5]int{2, 3, 6, 15, 50}
	fmt.Println(arr)
}
```

输出结果：

```
hello gonoob
[hello gonoob]
[2 3 6 15 50]
```