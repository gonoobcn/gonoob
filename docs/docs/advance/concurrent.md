# 并发编程

Go 语言是一种支持并发的编程语言，它可以让我们通过简单的语法和机制来实现高效的并发程序。本章节将介绍 Go 语言的并发基础知识和实践方法，帮助你掌握 Go 语言的并发编程技巧。

## 并发基础

Go 语言的并发是基于 ```goroutine``` 和 ```channel``` 的。

```goroutine``` 是一种用户态的轻量级线程，它由 Go 语言的运行时创建和管理，可以在一个进程中同时运行多个 ```goroutine```，并实现并发效果。

```channel``` 是一种特殊的类型，它提供了一种通信机制，可以让多个 ```goroutine``` 在不需要共享内存或者使用锁机制的情况下，通过 ```channel``` 传递数据和同步状态。

要创建一个 ```goroutine```，只需要在函数调用前加上 ```go``` 关键字即可。例如：

```go
func main() {
    go say("Hello")
    say("World")
}

func say(s string) {
    for i := 0; i < 5; i++ {
        fmt.Println(s)
        time.Sleep(100 * time.Millisecond)
    }
}
```

这段代码会创建两个 ```goroutine```，在主函数中执行 ```say(“World”)```，在另一个 ```goroutine``` 中执行 ```say(“Hello”)```。两个 ```goroutine``` 并发地运行，并输出结果到标准输出。

要创建一个 ```channel```，可以使用 ```make``` 函数指定 ```channel``` 的类型和容量（可选）。例如：

```go
ch := make(chan int) // 创建一个无缓冲的整型 channel
ch := make(chan int, 10) // 创建一个有缓冲（容量为10）的整型 channel
```

要向 ```channel``` 发送数据，可以使用 ```<-``` 运算符。例如：

```go
ch <- x // 将 x 的值发送到 ch 中
```

要从 ```channel``` 接收数据，也可以使用 ```<-``` 运算符。例如：

```go
x := <- ch // 将 ch 中的值赋给 x
<- ch // 只接收 ch 中的值，不赋给任何变量
```

如果 ```channel``` 是无缓冲的，那么发送操作会阻塞直到有接收方准备好接收数据；同样地，接收操作也会阻塞直到有发送方准备好发送数据。如果 ```channel``` 是有缓冲的，那么发送操作只会在缓冲区满时阻塞；接收操作只会在缓冲区空时阻塞。

```channel``` 可以用来在多个 ```goroutine``` 之间传递数据和信号。例如：

```go
func main() {
    ch := make(chan string)
    go func() {
        fmt.Println("Hello")
        ch <- "Done" // 发送信号表示完成
    }()
    msg := <- ch // 等待接收信号
    fmt.Println(msg)
}
```

这段代码会创建一个无缓冲的字符串类型的 ```channel```，在主函数中等待从该 ```channel``` 接收信号，在另一个匿名函数中打印 ```“Hello”``` 并向该 ```channel``` 发送 ```“Done”``` 表示完成。

## 并发模式

Go 语言提供了一些常用的并发模式，可以帮助我们更好地组织和管理并发程序。以下是一些常见的并发模式：

| 模式 | 说明 |
| -- | -- |
| WaitGroup | ```sync.WaitGroup``` 类型可以用来等待一组 ```goroutine``` 完成，并获取它们的返回值。|
| Mutex | ```sync.Mutex``` 类型可以用来保护临界区资源，在多个 ```goroutine``` 访问同一份数据时避免竞态条件。|
| Once | ```sync.Once``` 类型可以用来保证某个操作只执行一次，在初始化或延迟加载场景中很有用。|


## sync.WaitGroup

```WaitGroup``` 可以用来等待一组 ```goroutine``` 完成，并获取它们的返回值。要使用 ```WaitGroup```，我们需要先创建一个 ```WaitGroup``` 的实例，并为每个 ```goroutine``` 调用 ```Add(1)``` 方法来增加计数器。然后，在每个 ```goroutine``` 的函数中，调用 ```Done()``` 方法来减少计数器。最后，在主函数中，调用 ```Wait()``` 方法来阻塞等待所有的 ```goroutine``` 完成。

例如，我们想要并发地计算 1 到 100 的和，可以这样写：

```go
package main

import (
    "fmt"
    "sync"
)

func main() {
    var wg sync.WaitGroup // 创建一个 WaitGroup 实例
    sum := 0              // 存储结果的变量
    for i := 1; i <= 100; i++ {
        wg.Add(1) // 为每个 goroutine 增加计数器
        go func(n int) {
            defer wg.Done() // 在函数结束时减少计数器
            sum += n        // 累加数字
        }(i)
    }
    wg.Wait()       // 等待所有的 goroutine 完成
    fmt.Println(sum) // 打印结果
}
```

这段代码会创建 100 个 ```goroutine```，每个 ```goroutine``` 负责累加一个数字到 ```sum``` 变量中。主函数会等待所有的 ```goroutine``` 完成后打印出最终的结果。

## sync.Mutex

```Mutex``` 可以用来保护临界区资源，在多个 ```goroutine``` 访问同一份数据时避免竞态条件。要使用 Mutex，我们需要先创建一个 ```sync.Mutex``` 的实例，并在临界区代码前调用 ```Lock()``` 方法来加锁，在临界区代码后调用 ```Unlock()``` 方法来解锁。

例如，我们想要并发地向一个切片中添加元素，可以这样写：

```go
package main

import (
    "fmt"
    "sync"
)

func main() {
    var mu sync.Mutex   // 创建一个 Mutex 实例
    slice := make([]int, 0, 10) // 创建一个切片
    for i := 0; i < 10; i++ {
        go func(n int) {
            mu.Lock()         // 在修改切片前加锁
            slice = append(slice, n) // 向切片中添加元素
            mu.Unlock()       // 在修改切片后解锁
        }(i)
    }
    
}
```

## sync.Once

```Once``` 可以用来保证某个操作只执行一次，在初始化或延迟加载场景中很有用。要使用 ```Once```，我们需要先创建一个 ```sync.Once``` 的实例，并在需要执行一次的操作前调用 ```Do()``` 方法，并传入一个函数作为参数。

例如，我们想要在程序中只初始化一次一个全局变量，可以这样写：

```go
package main

import (
    "fmt"
    "sync"
)

var once sync.Once // 创建一个 Once 实例
var num int        // 全局变量

func main() {
    for i := 0; i < 10; i++ {
        go func() {
            once.Do(initNum) // 只执行一次 initNum 函数
            fmt.Println(num) // 打印全局变量
        }()
    }
}

func initNum() {
    num = 42 // 初始化全局变量
}
```

这段代码会创建 10 个 ```goroutine```，每个 ```goroutine``` 都会尝试调用 ```once.Do(initNum)``` 来初始化全局变量 ```num```。但是只有第一个 ```goroutine``` 调用时，```initNum``` 函数才会真正执行，后面的 ```goroutine``` 调用时都会忽略该函数。因此，所有的 ```goroutine``` 都会打印出相同的 ```num``` 值。

## select

```select``` 可以用来同时处理多个 ```channel``` 的发送和接收操作，并根据情况选择其中一个分支执行。要使用 ```select```，我们需要先创建一个 ```select``` 语句，并在其中列出多个 ```case``` 分支，每个分支对应一个 ```channel``` 的发送或接收操作。```select``` 语句会阻塞等待任意一个分支就绪，并随机选择其中一个执行。如果有 ```default``` 分支，则在没有其他分支就绪时执行 ```default``` 分支。

例如，我们想要从两个不同的 ```channel``` 中接收数据，并打印出最先到达的数据，可以这样写：

```go
package main

import (
    "fmt"
    "time"
)

func main() {
    ch1 := make(chan int)
    ch2 := make(chan int)
    
    go func() {
        time.Sleep(2 * time.Second)
        ch1 <- 1
    }()
    
    go func() {
        time.Sleep(1 * time.Second)
        ch2 <- 2
    }()
    
    select {
    case x := <- ch1:
        fmt.Println("Received", x, "from ch1")
    case y := <- ch2:
        fmt.Println("Received", y, "from ch2")
    }
}
```

这段代码会创建两个无缓冲的整型 ```channel```，并分别在两个 ```goroutine``` 中向它们发送数据。主函数中使用 select 语句等待从两个 ```channel``` 中接收数据，并打印出最先到达的数据。由于 ```ch2``` 比 ```ch1``` 先发送数据，所以 ```select``` 语句会选择第二个 ```case``` 分支执行，并打印出 ```“Received 2 from ch2”```。

## range 和 close

```range``` 和 ```close``` 可以用来在多个 ```goroutine``` 之间传递数据和信号。要使用 ```range``` 和 ```close```，我们需要先创建一个有缓冲的 ```channel```，并在发送方向该 ```channel``` 发送数据，并在发送完毕后调用 ```close()``` 方法来关闭该 ```channel```。然后，在接收方使用 ```for range``` 循环来从该 ```channel``` 接收数据，直到该 ```channel``` 被关闭。

例如，我们想要并发地计算斐波那契数列的前 10 项，并将结果发送到一个 ```channel``` 中，可以这样写：

```go
package main

import "fmt"

func main() {
    ch := make(chan int, 10) // 创建一个有缓冲（容量为10）的整型 channel
    go fibonacci(10, ch)      // 在一个 goroutine 中计算斐波那契数列并发送到 ch 中
    for n := range ch {      // 在主函数中从 ch 中接收数据，直到 ch 被关闭
        fmt.Println(n)       // 打印结果
    }
}

func fibonacci(n int, ch chan int) {
    x, y := 0, 1              // 初始化两个变量
    for i := 0; i < n; i++ {  // 循环 n 次
        ch <- x               // 将 x 的值发送到 ch 中
        x, y = y, x+y         // 更新 x 和 y 的值
    }
    close(ch)                 // 关闭 ch
}
```

这段代码会创建一个有缓冲的整型 ```channel```，并在一个 ```goroutine``` 中计算斐波那契数列的前 10 项，并将结果依次发送到该 ```channel``` 中。主函数中使用 ```for range``` 循环从该 ```channel``` 中接收数据，并打印出结果。当 ```goroutine``` 发送完毕后，会调用 ```close(ch)``` 来关闭该 ```channel```，此时主函数中的 ```for range``` 循环也会结束。

## 超时处理

超时处理可以用来在多个 ```goroutine``` 之间设置时间限制和取消操作。要使用超时处理，我们需要先创建一个 ```time.After``` 函数返回的只读 ```channel```，并将其作为 ```select``` 语句的一个 ```case``` 分支。```time.After``` 函数接受一个时间参数，表示经过多长时间后向返回的 ```channel``` 发送当前时间。如果 ```select``` 语句选择了 ```time.After``` 返回的 ```case``` 分支，则表示已经超过了设定的时间限制。

例如，我们想要从一个无缓冲的整型 ```channel``` 中接收数据，并设置一秒钟的超时限制，可以这样写：

```go
package main

import (
    "fmt"
    "time"
)

func main() {
    ch := make(chan int)     // 创建一个无缓冲的整型 channel
    
    go func() {
        time.Sleep(2 * time.Second)
        ch <- 1              // 在两秒后向 ch 发送数据
    }()
    
    select {
    case x := <-ch:          // 如果从 ch 接收到数据，则执行此分支
        fmt.Println("Received", x)
    case <-time.After(1 * time.Second): // 如果经过一秒没有从任何分支就绪，则执行此分支
        fmt.Println("Timeout")
    }
}
```

这段代码会创建一个无缓冲的整型 ```channel```，并在一个 ```goroutine``` 中延迟两秒后向该 ```channel``` 发送数据。主函数中使用 ```select``` 语句等待从该 ```channel``` 接收数据或者经过一秒钟没有任何响应。由于 ```goroutine``` 延迟了两秒才发送数据，所以 ```select``` 语句会选择第二个 ```case``` 分支执行，并打印出 ```“Timeout”```。

## worker pool

```worker pool``` 可以用来在多个 ```goroutine``` 之间分配和执行任务，并控制并发数量。要使用 ```worker pool```，我们需要先创建一个有缓冲的 ```channel``` 作为任务队列，并向其中发送任务。然后，创建一定数量的 ```worker goroutine``` 来从任务队列中获取任务并执行。最后，在主函数中等待所有的 ```worker goroutine``` 完成。

例如，我们想要并发地计算 1 到 100 的平方和，并限制最多只有 10 个 ```goroutine``` 同时执行，可以这样写：

```go
package main

import (
    "fmt"
    "sync"
)

func main() {
    tasks := make(chan int, 100) // 创建一个有缓冲（容量为100）的整型 channel 作为任务队列
    for i := 1; i <= 100; i++ {
        tasks <- i // 向任务队列中发送任务（数字）
    }
    close(tasks) // 关闭任务队列
    
    var wg sync.WaitGroup // 创建一个 WaitGroup 实例
    sum := 0              // 存储结果的变量
    for i := 0; i < 10; i++ { // 创建10个 worker goroutine
        wg.Add(1) // 增加计数器
        go func() {
            defer wg.Done() // 减少计数器
            for n := range tasks { // 循环从任务队列中获取任务，直到队列被关闭
                sum += n * n      // 计算平方和
            }
        }()
    }
    
    wg.Wait()       // 等待所有的 worker goroutine 完成
    fmt.Println(sum) // 打印结果
}
```

这段代码会创建一个有缓冲的整型 ```channel```，并向其中发送从 1 到 100 的数字作为任务。然后，创建10个 ```worker goroutine``` 来从该 ```channel``` 中获取数字并计算其平方和，并将结果累加到 sum 变量中。主函数会等待所有的 ```worker goroutine``` 完成后打印出最终的结果。

## context

```context``` 可以用来在多个 ```goroutine``` 之间传递上下文信息和取消信号。要使用 ```context```，我们需要先创建一个 ```context.Context``` 的实例，并在需要传递或取消信息的地方使用该实例。

```context.Context``` 实例可以通过 ```context``` 包提供的函数来生成或派生，例如 ```context.Background()```, ```context.WithValue()```, ```context.WithCancel()```, ```context.WithDeadline()```, ```context.WithTimeout()``` 等。

例如，我们想要在一个 ```HTTP``` 请求处理函数中启动两个子 ```goroutine``` 来分别处理不同的逻辑，并在请求超时或客户端断开连接时取消这两个子 ```goroutine``` 的执行，可以这样写：

```go
package main

import (
	"context"
	"fmt"
	"log"
	"net/http"
	"time"
)

func main() {
	http.HandleFunc("/", handler)
	log.Fatal(http.ListenAndServe(":8080", nil))
}

func handler(w http.ResponseWriter, r *http.Request) {
	ctx := r.Context()                      // 获取请求上下文
	ctx, cancel := context.WithTimeout(ctx, time.Second*5) // 设置请求超时时间为5秒，并返回一个新的上下文和取消函数
	defer cancel()                          // 在函数结束时调用取消函数
	
	done := make(chan struct{})             // 创建一个无缓冲的通道用于同步
	
	go func() {                             // 启动第一个子goroutine来处理逻辑A
		defer close(done)                   // 在goroutine结束时关闭通道
		
		select {
		case <-time.After(time.Second * 3):   // 模拟耗时3秒的操作
			fmt.Fprintln(w, "Logic A done")   // 向响应中写入数据
			
		case <-ctx.Done():                    // 如果上下文被取消，则退出goroutine
			
			err := ctx.Err()
			log.Println("Logic A canceled:", err)
			http.Error(w, err.Error(), http.StatusInternalServerError)
			
		}
		
	}()
}
```
