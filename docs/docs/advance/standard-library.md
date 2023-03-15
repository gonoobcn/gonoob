# 常用标准库

本章节我们将学习 Go 语言的常用标准库。

## fmt

```fmt``` 包提供了类似于 C 的 ```printf``` 和 ```scanf``` 的函数来实现格式化输出和输入。

### 格式化占位符

通用的：

| 占位符 | 说明 |
| --- | -- |
| %v | 表示默认格式的值 |
| %+v | 输出结构体时会添加字段名 |
| %#v | 值的 Go 语法表示 |
| %T | 值类型的 Go 语法表示 |
| %% | 百分号 |

布尔值：

| 占位符 | 说明 |
| --- | -- |
| %t | 表示 true 或 false |

数值：

| 占位符 | 说明 |
| --- | -- |
| %b | 表示二进制 |
| %c | 表示对应的 Unicode 码代表的字符 |
| %d | 表示十进制 |
| %o | 表示八进制 |
| %O | 表示八进制，前缀为 0o |
| %q | 一个用 Go 语法安全转义的单引号字符文字 |
| %x | 十六进制，小写字母表示 af |
| %X | 十六进制，大写字母表示 AF |
| %U | Unicode 格式，U+1234；与“U+%04X”相同 |

浮点和复数：

| 占位符 | 说明 |
| --- | -- |
| %b | 指数为 2 的幂的无小数科学记数法，例如 -123456p-78 |
| %e | 科学计数法，例如 -1.234456e+78 |
| %E | 科学计数法，例如 -1.234456E+78 |
| %f | 小数点但没有指数，例如 123.456 |
| %F | %f 的同义词 |
| %x | 十六进制表示法（十进制的二次幂），例如 -0x1.23abcp+20 |
| %X | 大写十六进制表示法，例如 -0X1.23ABCP+20 |

字符串和字节切片（byte[]）：

| 占位符 | 说明 |
| --- | -- |
| %s | 字符串或切片的未解释字节 |
| %q | 一个用 Go 语法安全转义的双引号字符串 |
| %x | 小写十六进制表示法，每字节两个字符 |
| %X | 大写十六进制表示法，每字节两个字符 |

指针：

| 占位符 | 说明 |
| --- | -- |
| %p | 十六进制表示法，前缀为 0x |

```%v``` 的默认格式是：

```
布尔值：%t
整数类型：%d
无符号的整数类型：%d、%#x（如果使用 %#v 打印）
浮点类型、复数类型：%g
字符串：%s
指针：%p
```

### Print

```Print``` 系列函数会将内容格式化，并输出到系统的标准输出。该系列函数会返回写入的字节数和遇到的任何写入错误。

```go
func Print(a ...any) (n int, err error)
func Printf(format string, a ...any) (n int, err error)
func Println(a ...any) (n int, err error)
```

```Print``` 系列函数及相关说明：

| 函数 | 说明 |
| -- | -- |
| Print | 使用其操作数的默认格式打印内容，如果两个操作数都不是字符串，则在操作数之间添加空格。 |
| Printf | 根据格式占位符格式化内容。 |
| Println | 使用其操作数的默认格式格式化，始终在操作数之间添加空格，并附加换行符。 |

举个例子：

```go
package main

import "fmt"

func main() {
	const name, age = "gonoob", 12
	fmt.Print(name, " is ", age, " years old.\n")
	fmt.Printf("%s is %d years old.\n", name, age)
	fmt.Println(name, "is", age, "years old.")

	n, err := fmt.Print(name, " is ", age, " years old.\n")
	fmt.Println(n, err)
}
```

输出结果：

```
gonoob is 12 years old.
gonoob is 12 years old.
gonoob is 12 years old.
gonoob is 12 years old.
24 <nil>
```

### Sprint

```Sprint``` 系列函数会将内容格式化成一个字符串，并返回该字符串。

```go
func Sprint(a ...any) string
func Sprintf(format string, a ...any) string
func Sprintln(a ...any) string
```

```Print``` 系列函数及相关说明：

| 函数 | 说明 |
| -- | -- |
| Sprint | 使用其操作数的默认格式格式化。如果两个操作数都不是字符串，则在操作数之间添加空格。 |
| Sprintf | 根据格式占位符格式化。 |
| Sprintln | 使用其操作数的默认格式。始终在操作数之间添加空格，并附加换行符。 |

举个例子：

```go
package main

import "fmt"

func main() {
	const name, age = "gonoob", 13
	s1 := fmt.Sprint(name, " is ", age, " years old.")
	s2 := fmt.Sprintf("%s is %d years old.", name, age)
	s3 := fmt.Sprintln(name, "is", age, "years old.")

	fmt.Println(s1)
	fmt.Println(s2)
	fmt.Println(s3)
}
```

输出结果：

```
gonoob is 13 years old.
gonoob is 13 years old.
gonoob is 13 years old.
```

### Fprint

```Fprint``` 系列函数将内容输出到任何实现 ```io.Writer``` 接口的变量 ```w``` 中（比如 File、Stdout 等），并返回写入的字节数和遇到的任何写入错误。

```go
func Fprint(w io.Writer, a ...any) (n int, err error)
func Fprintf(w io.Writer, format string, a ...any) (n int, err error)
func Fprintln(w io.Writer, a ...any) (n int, err error)
```

```Fprint``` 系列函数及相关说明：

| 函数 | 说明 |
| -- | -- |
| Fprint | 使用其操作数的默认格式并写入 ```io.Writer```。如果操作数都不是字符串，则在操作数之间添加空格。 |
| Fprintf | 根据格式说明符格式化并写入 ```io.Writer```。 |
| Fprintln | 使用其操作数的默认格式并写入 ```io.Writer```。始终在操作数之间添加空格，并附加换行符。 |

举个例子：

```go
package main

import (
	"fmt"
	"os"
)

func main() {
	const name, age = "gonoob", 15

	// 输出到系统的标准输出
	n, err := fmt.Fprint(os.Stdout, name, " is ", age, " years old.\n")
	if err != nil {
		fmt.Fprintf(os.Stderr, "Fprint: %v\n", err)
	}
	fmt.Print(n, " bytes written.\n")

	// 写入到文件中
	f, err := os.OpenFile("./hello.txt", os.O_CREATE|os.O_WRONLY|os.O_APPEND, 0644)
	if err != nil {
		fmt.Println("open file err:", err)
		return
	}
	fmt.Fprintf(f, "Hello GoNoob！")
}
```

输出结果：

```
gonoob is 15 years old.
24 bytes written.
```

```hello.txt``` 文件内容：

```
Hello GoNoob！
```

### Errorf

```Errorf``` 函数会根据格式占位符格式化，并将字符串作为满足错误的值返回。

```go
func Errorf(format string, a ...any) error
```

举个例子：

```go
package main

import "fmt"

func main() {
	err := fmt.Errorf("this is error")
	fmt.Println(err)
}
```

输出结果：

```
this is error
```

### Sscanf

```Sscanf``` 函数会扫描参数字符串，根据格式将连续的空格分隔值存储为连续的参数。它返回成功解析的项目数。输入中的新行必须与格式中的换行符匹配。

```go
func Sscanf(str string, format string, a ...any) (n int, err error)
```

举个例子：

```go
package main

import "fmt"

func main() {
	var name string
	var age int
	n, err := fmt.Sscanf("Gonoob is 18 years old", "%s is %d years old", &name, &age)
	if err != nil {
		panic(err)
	}
	fmt.Printf("%d: %s, %d\n", n, name, age)
}
```

输出结果：

```
2: Gonoob, 18
```


### Fscan

```Fscan``` 系列函数会扫描从 ```r``` 读取的文本，将连续的空格分隔值存储为连续的参数。换行符算作空格。它返回成功扫描的项目数。如果这小于参数的数量，```err``` 将报告原因。

```go
func Fscan(r io.Reader, a ...any) (n int, err error)
func Fscanf(r io.Reader, format string, a ...any) (n int, err error)
func Fscanln(r io.Reader, a ...any) (n int, err error)
```

举个例子：

```go
package main

import (
	"fmt"
	"os"
	"strings"
)

func main() {
	var (
		i int
		b bool
		s string
	)
	r := strings.NewReader("5 true gonoob")
	n, err := fmt.Fscanf(r, "%d %t %s", &i, &b, &s)
	if err != nil {
		fmt.Fprintf(os.Stderr, "Fscanf: %v\n", err)
	}
	fmt.Println(i, b, s)
	fmt.Println(n)
}
```

输出结果：

```
5 true gonoob
3
```

## time

```time``` 包提供了测量和显示时间的函数，日历计算始终采用公历。

### 时间对象

```Now``` 函数用于获取当前系统的时间对象，通过该对象可以获取当前系统的日期和时间。

```go
package main

import (
	"fmt"
	"time"
)

func main() {
	now := time.Now() // 获取当前系统的时间
	fmt.Println(now)

	year := now.Year()     // 年
	month := now.Month()   // 月
	day := now.Day()       // 日
	hour := now.Hour()     // 小时
	minute := now.Minute() // 分钟
	second := now.Second() // 秒
	fmt.Printf("%d-%02d-%02d %02d:%02d:%02d\n", year, month, day, hour, minute, second)
}
```

输出结果：

```
2023-03-05 10:23:35.668317 +0800 CST m=+0.000312787
2023-03-05 10:23:35
```

### 时间戳

时间戳是自1970年1月1日（08:00:00GMT）至当前时间的总秒数。

获取当前时间的时间戳：

```go
package main

import (
	"fmt"
	"time"
)

func main() {
	now := time.Now() // 获取当前时间

	t1 := now.Unix()      // 时间戳（单位：秒）
	t2 := now.UnixMilli() // 毫秒时间戳
	t3 := now.UnixMicro() // 微秒时间戳
	t4 := now.UnixNano()  // 纳秒时间戳

	fmt.Printf("current t1:%v\n", t1)
	fmt.Printf("current t2:%v\n", t2)
	fmt.Printf("current t3:%v\n", t3)
	fmt.Printf("current t4:%v\n", t4)
}
```

输出结果：

```
current t1:1677983954
current t2:1677983954005
current t3:1677983954005668
current t4:1677983954005668000
```

### 时间常量

```time``` 预定义了一些时间常量，如时间格式、时间间隔等常量。

时间格式常量：

```go
const (
	Layout      = "01/02 03:04:05PM '06 -0700" // The reference time, in numerical order.
	ANSIC       = "Mon Jan _2 15:04:05 2006"
	UnixDate    = "Mon Jan _2 15:04:05 MST 2006"
	RubyDate    = "Mon Jan 02 15:04:05 -0700 2006"
	RFC822      = "02 Jan 06 15:04 MST"
	RFC822Z     = "02 Jan 06 15:04 -0700" // RFC822 with numeric zone
	RFC850      = "Monday, 02-Jan-06 15:04:05 MST"
	RFC1123     = "Mon, 02 Jan 2006 15:04:05 MST"
	RFC1123Z    = "Mon, 02 Jan 2006 15:04:05 -0700" // RFC1123 with numeric zone
	RFC3339     = "2006-01-02T15:04:05Z07:00"
	RFC3339Nano = "2006-01-02T15:04:05.999999999Z07:00"
	Kitchen     = "3:04PM"
	// Handy time stamps.
	Stamp      = "Jan _2 15:04:05"
	StampMilli = "Jan _2 15:04:05.000"
	StampMicro = "Jan _2 15:04:05.000000"
	StampNano  = "Jan _2 15:04:05.000000000"
)
```

这些常量是用于 ```time.Format``` 和 ```time.Parse``` 的预定义格式常量。

时间间隔常量：

```go
const (
	Nanosecond  Duration = 1
	Microsecond          = 1000 * Nanosecond
	Millisecond          = 1000 * Microsecond
	Second               = 1000 * Millisecond
	Minute               = 60 * Second
	Hour                 = 60 * Minute
)
```

获取一段时间间隔，可直接使用这些常量：

```go
package main

import (
	"fmt"
	"time"
)

func main() {
	fmt.Println(time.Second) // 获取1秒的时间间隔
	fmt.Println(time.Hour)   // 获取1小时的时间间隔
}
```

输出结果：

```
1s
1h0m0s
```

时间间隔类型 ```Duration``` 与 ```int``` 类型的转换：

```go
// 时间间隔类型转换成整型
second := time.Second 
fmt.Print(int64(second)) // 10

// 整型转换成时间间隔类型
seconds := 10 
fmt.Print(time.Duration(seconds)*time.Second) // 10s
```

### 时间格式化

```Format``` 函数用于时间格式化。格式化时间模板必须是 Go 的诞生时间2006年1月2号15点04分，你也可以选择预定义的时间格式常量作为格式化模版使用。

```go
package main

import (
	"fmt"
	"time"
)

func main() {
	now := time.Now()

    // 24小时制
    fmt.Println(now.Format("2006-01-02 15:04:05.000 Mon Jan"))
	fmt.Println(now.Format("2006/01/02 15:04"))
	fmt.Println(now.Format(time.RFC3339))

    // 12小时制
    fmt.Println(now.Format("2006-01-02 03:04:05.000 PM Mon Jan"))
    fmt.Println(now.Format("15:04 2006/01/02"))
    fmt.Println(now.Format("2006/01/02"))
}
```

输出结果：

```
2023-03-05 11:28:04.991 Sun Mar
2023/03/05 11:28
2023-03-05T11:28:04+08:00
2023-03-05 11:28:04.991 AM Sun Mar
11:28 2023/03/05
2023/03/05
```

### 时间操作

获取时间间隔 d 之后的时间，返回时间为 ```t+d```：

```go
func (t Time) Add(d Duration) Time
```

时间 ```t``` 与 时间 ```u``` 的差值：

```go
func (t Time) Sub(u Time) Duration
```

判断时刻 ```t``` 是否在 ```u``` 之后：

```go
func (t Time ) After(u Time ) bool
```

判断时刻 ```t``` 是否在 ```u``` 之前：

```go
func (t Time) Before(u Time) bool
```


### After

```After``` 函数会在等待时间间隔 d 过去后，然后在返回的通道上发送当前时间。

```go
func After(d Duration) <-chan Time
```

举个例子：

```go
package main

import (
	"fmt"
	"time"
)

var c chan int

func handle(int) {}

func main() {
	select {
	case m := <-c:
		handle(m)
	case <-time.After(10 * time.Second):
		fmt.Println("timed out")
	}
}
```

10秒钟之后，输出结果：

```go
timed out
```

### Sleep

```Sleep``` 函数会暂停当前的 ```goroutine``` 至少持续时间 ```d```。当持续时间为负或零时，会导致 ```Sleep``` 立即返回。

举个例子：

```go
package main

import (
	"time"
)

func main() {
	time.Sleep(5 * time.Second)
}
```

5秒之后会退出 ```main``` 协程（```main``` 函数将结束运行）。

## log

```log``` 包实现了简单的日志输出。

## strings

## io

## strconv

## flag

## reflect

## http