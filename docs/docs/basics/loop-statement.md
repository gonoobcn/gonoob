# 循环语句

Go 中的循环语句有 ```for``` 语句和 ```for range``` 语句。

## for 语句

Go 只有 ```for``` 循环。没有 ```while``` 循环，```for``` 循环中包含了 ```while``` 循环。

```for``` 循环的基本格式：

```go
for 初始化语句（可选）; 条件表达式; 后置语句（可选） {
	// ...
}

// 相当于 while 循环
for 条件表达式 {
	// ...
}

// 无限循环
for {
}
```

基本的 ```for``` 循环由三部分组成，它们用分号隔开：

- 初始化语句：在第一次迭代前执行
- 条件表达式：在每次迭代前求值
- 后置语句：在每次迭代的结尾执行

```for``` 语句后面的三个构成部分外没有小括号，而大括号 ```{ }``` 则是必须的。初始化语句通常为一句短变量声明，该变量声明仅在 ```for``` 语句的作用域中可见。一旦条件表达式的布尔值为 ```false```，循环迭代就会终止。

举个例子：

```go
package main

import "fmt"

func main() {
	sum := 0
	for i := 0; i < 5; i++ {
		sum += i
	}
	fmt.Println(sum)
}
```

输出结果：

```
10
```

初始化语句和后置语句是可选的：

```go
package main

import "fmt"

func main() {
	sum := 1
	for sum < 100 {
		sum += sum
	}
	fmt.Println(sum)
}
```

输出结果：

```
128
```

在以上代码中，我们省略了初始化语句和后置语句，只保留了条件表达式部分，此时的 ```for``` 循环就相当于 ```C``` 语言中的 ```while``` 循环。

### 无限循环

如果省略循环条件，该循环就不会结束。

```go
package main

func main() {
	for {
	}
}
```

## for range 语句

```for range``` 语句经常被用来遍历字符串、数组、切片、Map 和通道。

```for range``` 语句的基本格式：

```go
for 返回值 := range 字符串/数组/切片/Map/通道 {
	// ...
}
```

遍历不同类型对应的返回值：

| 遍历类型 | 返回值1 | 返回值2 |
| ---- | ---- | ---- |
| string | index | s[index] |
| array | index | a[index] |
| slice	| index | s[index] |
| map | key | m[key] |
| channel | element | |

当使用 ```for range``` 循环遍历切片时，每次迭代都会返回两个值。第一个值为当前元素的下标，第二个值为该下标所对应元素的一份副本。

举个例子：

```go
package main

import "fmt"

var pow = []int{1, 3, 8, 10, 18, 32, 50, 118}

func main() {
	for i, v := range pow {
		fmt.Printf("index: %d, value: %d\n", i, v)
	}
}
```

输出结果：

```
index: 0, value: 1
index: 1, value: 3
index: 2, value: 8
index: 3, value: 10
index: 4, value: 18
index: 5, value: 32
index: 6, value: 50
index: 7, value: 118
```

可以将下标或值赋予 ```_``` 来忽略它：

```go
for i, _ := range pow
for _, value := range pow
```

若你只需要索引，直接忽略第二个变量即可：

```go
for i := range pow
```

举个例子：

```go
package main

import "fmt"

func main() {
	pow := make([]int, 5)
	for i := range pow {
		pow[i] = 1 << uint(i)
	}
	for _, value := range pow {
		fmt.Printf("%d\n", value)
	}
}
```

输出结果：

```
1
2
4
8
16
```