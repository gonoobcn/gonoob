# 协程

Go 协程 ```goroutine``` 是由 Go 运行时管理的轻量级线程。

开启一个协程任务：

```go
go f(x, y)
```

会启动一个新的 Go 协程来执行 ```f(x, y)``` 函数。

```x``` 和 ```y``` 的求值发生在当前的 Go 协程中，而 ```f``` 函数的执行发生在新的 Go 协程中。

```go
package main

import (
	"fmt"
	"time"
)

func say(s string) {
	for i := 0; i < 5; i++ {
		time.Sleep(time.Second)
		fmt.Println(s)
	}
}

func main() {
	go say("gonoob")
	say("hello")
}
```

输出结果：

```
gonoob
hello
hello
gonoob
gonoob
hello
hello
gonoob
gonoob
hello
```

在以上代码中，主协程和新建的协程并发执行 ```say``` 函数。