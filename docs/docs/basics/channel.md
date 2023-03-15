# 通道

Go 中使用 ```chan``` 来声明一个通道（channel），通道是带有类型的管道，你可以通过它用通道操作符 ```<-``` 来发送或者接收值。通道具有先进先出（FIFO）的特点，先发送到通道的值会被优先接收。

通道可用于实现 ```goroutine``` 之间的通信。

创建通道：

```go
c1 := make(chan int)    // 创建一个 int 类型的无缓冲通道
c2 := make(chan string) // 创建一个 string 类型的无缓冲通道
c3 := make(chan<- int)  // 创建一个 int 类型的无缓冲单向通道（只能发送不能接收）
c4 := make(<-chan int)  // 创建一个 int 类型的无缓冲单向通道（只能接收不能发送）

c5 := make(chan int, 20) // 创建一个 int 类型的有缓冲通道（可缓冲20个元素）
```

通道的发送、接收与关闭：

```go
ch <- v   // 将 v 发送至通道 ch。
v := <-ch // 从 ch 接收值并赋予 v。
close(ch) // 关闭通道 ch
```

举个例子：

```go
package main

import "fmt"

func main() {
	ch := make(chan int) // 创建一个 int 类型的无缓冲通道
	go func() {
		v := <-ch      // 从 ch 接收值并赋予 v。
		fmt.Println(v) // 输出 10
	}()
	ch <- 10  // 将 10 发送至通道 ch。
	close(ch) // 关闭通道 ch
}
```

输出结果：

```
10
```

在以上代码中，创建一个 ```int``` 类型的无缓冲通道，要向一个无缓冲通道发送值，则必须要有一个 ```goroutine``` 用于接收值，否者，进行发送操作的 ```goroutine``` 会阻塞。

## 无缓冲通道

默认创建的是无缓冲通道：

```go
ch := make(chan int)
```

无缓冲通道的特点：

- 当一个发送者 ```goroutine``` 向无缓冲通道发送值时，会阻塞，直到有一个接收者 ```goroutine``` 用于接收操作。
- 当一个接收者 ```goroutine``` 向无缓冲通道接收值时，也会阻塞，直到有一个发送者 ```goroutine``` 向通道中发送值。

## 有缓冲通道

通道可以设置缓冲区，通过 ```make``` 函数的第二个参数指定缓冲区大小：

```go
ch := make(chan int, 30)
```

通道 ```ch``` 是一个有缓冲通道，可缓冲 30 个 ```int``` 类型的值。

有缓冲通道的特点：

- 当一个发送者 ```goroutine``` 向有缓冲通道发送值时，如果缓冲区未满，则继续发送值；如果缓冲区已满，则会阻塞。

- 当一个接收者 ```goroutine``` 向有缓冲通道接收值时，如果缓冲区有值，则继续接收值；如果缓冲区没有值，则会阻塞。

## 遍历通道

通过 ```for range``` 循环语句遍历通道，它会不断从通道中接收值，直到通道被关闭：

```go
package main

import "fmt"

func send(c chan int) {
	for i := 0; i < 10; i++ {
		c <- i
	}
	close(c)
}

func main() {
	ch := make(chan int, 10)
	go send(ch)
	for v := range ch {
		fmt.Println(v)
	}
}
```

输出结果：

```
0
1
2
3
4
5
6
7
8
9
```

在以上代码中，```send``` 函数发送完 10 个数据之后关闭了通道，因此 ```for range``` 遍历完 10 次之后就结束了。如果没有关闭通道，则 ```for range``` 就不会结束，当接收不到数据时，就会一直阻塞。

## 关闭通道

使用 Go 内置的 ```close``` 函数来关闭一个通道：

```go
ch := make(chan int)
close(ch)
```

接收者可以通过为接收表达式分配第二个参数来判断通道是否被关闭：

```go
v, ok := <-ch // ok = true 表示关闭，ok = false 表示未关闭
```

关闭后的通道有以下特点：

- 对一个关闭的通道再发送值就会导致 panic。
- 对一个关闭的通道进行接收会一直获取值直到通道为空。
- 对一个关闭的并且没有值的通道执行接收操作会得到对应类型的零值。
- 关闭一个已经关闭的通道会导致 panic。

当发送者不再向通道中发送数据时，通常需要关闭这个通道。需要注意的是，只有发送者才能关闭通道。

## select 语句

```select``` 语句使一个 Go 协程可以等待多个通信操作。

```select``` 语句的基本格式：

```go
select {
case <- ch:
    // 如果成功从通道 ch 读到数据，则执行 case 语句
case ch <- 1:
    // 如果成功向通道 ch 写入数据，则执行 case 语句
default:
    // 如果上面都没有成功，则进入 default 处理流程
}
```

每个 ```case``` 对应一个通道的通信操作（接收或发送），```select``` 会阻塞直到某个分支可以继续执行为止，这时就会执行该分支。当多个分支都准备好时会随机选择一个执行。

当 ```select``` 中的其它分支都没有准备好时，如果未声明 ```default``` 分支，则 ```select``` 会阻塞，直到有一个 ```case``` 可以执行；如果声明了 ```default``` 分支，则会执行该分支。

举个例子：

```go
package main

import "fmt"

func fibonacci(c, quit chan int) {
	x, y := 0, 1
	for {
		select {
		case c <- x:
			x, y = y, x+y
		case <-quit:
			fmt.Println("quit")
			return
		default:
			fmt.Println("default")
		}
	}
}

func main() {
	c := make(chan int)
	quit := make(chan int)
	go func() {
		for i := 0; i < 10; i++ {
			fmt.Println(<-c)
		}
		quit <- 0
	}()
	fibonacci(c, quit)
}
```


