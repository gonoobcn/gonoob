# 包

每个 Go 程序都是由包构成的，每个源文件（ ```.go``` 文件）必须属于一个包，一个包可以包含多个源文件。

在学习包之前，我们先新建一个 ```gonoob``` 目录，使用 ```go mod init 模块名称``` 将该目录初始化成一个 Go 模块：

```bash
$ mkdir gonoob
$ cd gonoob
$ go mod init gonoob.cn
```

模块初始化完成后，会在 ```gonoob``` 目录下生成 ```go.mod``` 文件：

```
gonoob
 |-- go.mod
```

在 ```go.mod``` 文件中，声明了模块名称和 Go 版本：

```
module gonoob.cn

go 1.20
```

在 ```gonoob``` 目录下新建一个 ```hello``` 目录和一个 ```main.go``` 文件，在 ``hello`` 目录下新建一个 ```hello.go``` 文件：

```
gonoob
 |-- hello
   |-- hello.go
 |-- go.mod
 |-- main.go
```

## 声明包

每个源文件中非注释的第一行必须声明包名：

```go
package 包名
```

在 ```hello.go``` 文件中编写如下代码：

```go
// hello.go
package hello

import "fmt"

func Say() {
    fmt.Println("Hello, GoNoob!")
}
```

在以上代码中，```package hello``` 声明了 ```hello.go``` 文件属于 ```hello``` 包。

在 ```main.go``` 文件中编写如下代码：

```go
package main

import "fmt"

func main() {
    fmt.Println("Hello, GoNoob!")
}
```

Go 语言规定，```main``` 函数所在的包必须是 ```main``` 包。

## 导入包

使用 ```import``` 导入其他包：

```go
package main

import "fmt"
import "math/rand"

func main() {
	fmt.Println("rand number is", rand.Intn(20))
}
```

在以上代码中，我们导入了 ```fmt```、```math/rand```包。包名与导入路径的最后一个名称一致。例如，```math/rand``` 包中的源码均以 ```package rand``` 语句开始；```fmt``` 包中的源码均以 ```package fmt``` 语句开始。

通过 ```包名.函数名``` 的方式来调用包中的函数，导入的包必须被使用，如未被使用则会报错。在 ```main``` 函数中，调用了 ```fmt``` 包的 ```Println``` 函数和 ```rand``` 包的 ```Intn``` 函数。

使用分组的形式导入，会让代码看起来更简洁一些：

```go
package main

import (
	"fmt"
	"math/rand"
    "time"
)

func main() {
	fmt.Println("rand number is", rand.Intn(20))
}
```

导入自定义的包，需要以 ```模块名称 + 路径名（包名）``` 的形式导入：

```go
package main

import "gonoob.cn/hello"

func main() {
    hello.Say()
}
```

Go 会从 ```go.mod``` 文件（已声明了模块名称）所在的目录中查找自定义的包文件。

导入的包必须被使用，如未被使用，则会报错。如果想导入包，但是不使用包中的函数，可以使用 ```_``` 来忽略导包：

```go
package main

import _ "gonoob.cn/hello"

func main() {
    
}
```

在以上代码中，使用 ```_``` 忽略了导包，表示该包可以不被使用，程序不会报错。此时程序会执行包中的 ```init``` 函数，```init``` 函数会在 ```main``` 函数执行之前执行。

## 导出包

在 Go 中，如果一个名称以大写字母开头，那么它就是已导出的，包外可见的，其他包可以直接使用；如果一个名称以小写字母开头的，那么它就是未导出的，只能在包内使用，其他包无法直接使用。

```go
package hello

import "fmt"

const Pi = 3.1415926  // 常量 Pi 是已导出的，可以被其他包使用
var a int = 666  // 变量 a 是未导出的，只能在 hello 包内使用

// 该函数是包外可见的，可以被其他包调用
func Say() {
    fmt.Println("Hello, GoNoob!")
}

// 该函数是包外不可见的，只能在 hello 包内使用
func print() {
	fmt.Println(a, Pi)
}
```

在 ```main``` 包中使用 ```hello``` 包：

```go
package main

import (
	"fmt"

	"gonoob.cn/hello"
)

func main() {
   hello.Say()
   fmt.Println(hello.Pi)
   hello.print()	// 错误，a not exported by package hello
   fmt.Println(hello.a) // 错误，a not exported by package hello
}
```