# 流程控制语句

Go 使用条件、循环、分支和推迟语句来控制代码的流程。

## defer 语句

```defer``` 语句会将函数推迟到外层函数返回之后执行。

推迟调用的函数其参数会立即求值，但直到外层函数返回前该函数都不会被调用。

```go
package main

import "fmt"

func main() {
	defer fmt.Println("gonoob")
	fmt.Println("hello")
}
```

控制台输出：

```
hello
gonoob
```

### defer 栈

推迟的函数调用会被压入一个栈中。当外层函数返回时，被推迟调用的函数会按照后进先出、先进后出的顺序调用。

```go
package main

import "fmt"

func main() {
	fmt.Println("counting")

	for i := 0; i < 10; i++ {
		defer fmt.Println(i)
	}

	fmt.Println("done")
}
```

控制台输出：

```
counting
done
9
8
7
6
5
4
3
2
1
0
```


