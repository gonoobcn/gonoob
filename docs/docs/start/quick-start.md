# 快速上手

我们来编写一个简单的 Go 程序并运行它。

## 第一个 Go 程序

Go 程序代码必须写在以 ```.go``` 为后缀的源文件中。

创建一个 ```hello.go``` 文件，在文件中编写如下代码：

```go
package main

import "fmt"

func main() {
    fmt.Println("Hello, World!")
}
```

要执行 Go 程序可以使用 ```go run``` 命令：

```bash
$ go run hello.go 
Hello, World!
```

你也可以使用 ```go build``` 命令将 ```hello.go``` 文件编译成二进制可执行文件：

```bash
$ go build hello.go 
$ ls
hello    hello.go
$ ./hello 
Hello, World!
```





