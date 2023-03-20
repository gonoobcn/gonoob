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

```log``` 包定义了 ```Logger``` 类型，该类型提供了一些格式化输出的方法。你可以使用 ```log``` 包提供的预定义的 ```logger```，也可以使用 ```log.New``` 函数创建自己的 ```logger``` 对象。

```log``` 包中的 ```Logger``` 提供了 ```Print```、```Printf```、```Println```、```Fatal```、```Fatalf```、```Fatalln```、```Panic```、```Panicf``` 和 ```Panicln``` 等方法来打印不同级别的日志。

使用 ```log``` 包提供的预定义的 ```logger``` 打印日志：

```go
package main

import (
	"log"
)

func main() {
	// Print 系列函数打印普通日志
	log.Print("Hello, world!")
	log.Printf("Hello, %s!", "world")
	log.Println("Hello,", "world!")

	// Fatal 系列函数打印错误日志并退出程序
	log.Fatal("Something went wrong!")
	log.Fatalf("Error code: %d", 1)
	log.Fatalln("Error message:", "fatal error")

	// Panic 系列函数打印错误日志并引发 panic 异常
	log.Panic("Something went wrong!")
	log.Panicf("Error code: %d", 2)
	log.Panicln("Error message:", "panic error")
}
```

其中，```Fatal``` 系列函数会在打印日志后调用 ```os.Exit(1)``` 退出程序，```Panic``` 系列函数会在打印日志后引发 ```panic``` 异常。

```log``` 包中的 ```Logger``` 还支持设置输出目标、输出前缀和输出标志。

```SetOutput``` 函数可以将日志输出到标准输出：

```go
log.SetOutput(os.Stdout)
```

```SetPrefix``` 函数可以在每条日志前加上 ```[INFO]``` 前缀：

```go
log.SetPrefix(“[INFO]”) 
```

```SetFlags``` 函数可以在每条日志前加上日期和时间：

```go
log.SetFlags(log.Ldate | log.Ltime)
```

使用 ```log.New``` 函数创建自己的 ```logger``` 对象，并设置输出目标、输出前缀和输出标志：

```go
package main

import (
	"log"
	"os"
)

func main() {
	// 创建一个新的 logger 对象，将日志输出到文件，设置前缀和标志
	file, err := os.OpenFile("log.txt", os.O_CREATE|os.O_WRONLY|os.O_APPEND, 0666)
	if err != nil {
		log.Fatal(err)
	}
	defer file.Close()
	logger := log.New(file, "[MyLogger]", log.LstdFlags|log.Lshortfile)

	// 使用 logger 打印日志
	logger.Print("Hello, world!")
	logger.Printf("Hello, %s!", "world")
	logger.Println("Hello,", "world!")

	logger.Fatal("Something went wrong!")
	logger.Fatalf("Error code: %d", 1)
	logger.Fatalln("Error message:", "fatal error")

	logger.Panic("Something went wrong!")
	logger.Panicf("Error code: %d", 2)
	logger.Panicln("Error message:", "panic error")
}
```

在实际开发中，你可以根据不同的场景选择合适的日志级别和格式。例如，在开发环境中，你可以使用 ```Print``` 系列函数打印一些调试信息，在生产环境中，你可以使用 ```Fatal``` 系列函数打印一些严重错误信息，并及时退出程序。

你还可以使用一些第三方的 ```log``` 库或框架来增强 ```log``` 包的功能和特性。例如，```zap``` 是一个高性能且易于扩展的结构化日志库。

## strings

Go 语言提供了一个内置的包 ```strings``` 来实现一些常用的字符串操作。

### 判断字符串是否相等

```strings``` 包提供了 ```EqualFold``` 函数和 ```Compare``` 函数来判断两个字符串是否相等。

```go
func EqualFold(s, t string) bool // 判断两个 UTF-8 编码字符串（将 Unicode 大写、小写、标题三种格式字符视为相同）是否相等
func Compare(a, b string) int // 比较两个字符串的字典序，如果 a == b 返回 0，如果 a < b 返回 -1，如果 a > b 返回 1
```

举例：

```go
fmt.Println(strings.EqualFold("Go", "go")) // true
fmt.Println(strings.EqualFold("Go", "GO")) // true
fmt.Println(strings.EqualFold("Go", "gO")) // true
fmt.Println(strings.EqualFold("Go", "Golang")) // false
fmt.Println(strings.Compare("apple", "banana")) // -1
fmt.Println(strings.Compare("banana", "apple")) // 1
fmt.Println(strings.Compare("apple", "apple")) // 0
```

### 判断字符串是否有前缀或后缀

```strings``` 包提供了 ```HasPrefix``` 函数和 ```HasSuffix``` 函数来判断字符串是否有指定的前缀或后缀。

```go
func HasPrefix(s, prefix string) bool // 判断 s 是否有前缀 prefix
func HasSuffix(s, suffix string) bool // 判断 s 是否有后缀 suffix
```

举例：

```go
fmt.Println(strings.HasPrefix("gopher", "go")) // true
fmt.Println(strings.HasPrefix("gopher", "ph")) // false
fmt.Println(strings.HasSuffix("gopher", "er")) // true
fmt.Println(strings.HasSuffix("gopher", "ph")) // false
```

### 查找子串在字符串中出现的位置或次数

```strings``` 包提供了 ```Index```、```LastIndex```、```Count``` 函数来查找子串在字符串中出现的位置或次数。

```go
func Index(s, substr string) int // 返回 substr 在 s 中首次出现的位置，如果不存在则返回 -1
func LastIndex(s, substr string) int // 返回 substr 在 s 中最后一次出现的位置，如果不存在则返回 -1
func Count(s, substr string) int // 返回 substr 在 s 中出现的非重叠次数，如果 substr 为空则返回 s 的长度加一
```

举例：

```go
fmt.Println(strings.Index("gopher", "ph")) // 2 
fmt.Println(strings.Index("gopher", "z")) // -1
fmt.Println(strings.LastIndex("gophers are cool gophers are fun", "ph")) // 18 
fmt.Println(strings.LastIndex("gophers are cool gophers are fun", "z")) // -1
fmt.Println(strings.Count("cheeseburger", "e")) // 4 
fmt.Println(strings.Count("cheeseburger", "")) // 13 
```

### 分割和拼接字符串

```strings``` 包提供了 ```Split``` 函数和 ```Join``` 函数来分割和拼接字符串。

```go
func Split(s, sep string) []string // 将 s 按照 sep 分割成一个 []string，并返回该切片，如果 sep 为空，则将 s 分割成每一个 Unicode 字符组成的切片
func Join(a []string, sep string) string // 将一个 []string 按照 sep 拼接成一个字符串，并返回该字符串
```

举例：

```go

// 分割字符串
s := "a,b,c"
ss := strings.Split(s, ",")
for _, v := range ss {
	fmt.Printf("%s\n", v)
}

// 拼接字符串
ss := []string{"a", "b", "c"}
s := strings.Join(ss, ",")
fmt.Println(s) // a,b,c
```

### 替换和重复字符串

```strings``` 包提供了 ```Replace``` 函数和 ```Repeat``` 函数来替换和重复字符串。

```go
func Replace(s, old, new string, n int) string // 将 s 中前 n 个出现的 old 子串替换为 new 子串，并返回新的字符串。如果 n < 0，则替换所有出现的 old 子串
func Repeat(s string, count int) string // 将 s 重复 count 次，并返回新的字符串。如果 count <= 0，则返回空字符串
```

举例：

```go
s := "banana"
fmt.Println(strings.Replace(s, "a", "o", 1)) // bonana 
fmt.Println(strings.Replace(s, "a", "o", -1)) // bonono

s := "ha"
fmt.Println(strings.Repeat(s, 3)) // hahaha 
fmt.Println(strings.Repeat(s, -1)) //
```

### 转换字符串的大小写

strings 包提供了多个函数来转换字符串的大小写。

```go
func ToUpper(s string) string // 将 s 中的所有 Unicode 字符转换为大写，并返回新的字符串
func ToLower(s string) string // 将 s 中的所有 Unicode 字符转换为小写，并返回新的字符串
func Title(s string) string // 将 s 中每个单词（以空白字符分隔）的首字母转换为大写，并返回新的字符串
```

举例：

```go
s := "Hello World"
fmt.Println(strings.ToUpper(s)) // HELLO WORLD 

s := "Hello World"
fmt.Println(strings.ToLower(s)) // hello world 

s := "hello world"
fmt.Println(strings.Title(s)) // Hello World 
```

## io

Go 语言的 io 包提供了一些基本的接口，用于数据的输入和输出。最重要的两个接口是 io.Reader 和 io.Writer，它们分别表示可以读取或写入字节流的类型。io 包还提供了一些辅助函数和类型，例如 io.Copy，io.MultiReader，io.Pipe 等，可以方便地对不同来源的数据进行操作。

### io.Reader 接口

```io.Reader``` 接口定义了一个方法：

```go
Read(p []byte) (n int, err error)
```

它将 ```len(p)``` 个字节读取到 ```p``` 中，并返回实际读取的字节数 ```n``` 和可能发生的错误 ```err```。如果没有更多的数据可读，```err``` 将返回 ```io.EOF```。

```io.Reader``` 接口是很多其他接口的基础，例如 ```io.ReadCloser```、```io.ReadSeeker``` 等。它也被很多标准库中的类型实现，例如 ```os.File```、```bytes.Buffer```、```strings.Reader``` 等。

### Read 方法

```Read``` 方法是 ```io.Reader``` 接口唯一必须实现的方法。它会尝试从数据源中读取最多 ```len(p)``` 个字节，并填充到 ```p``` 中。

它返回实际读取到的字节数 ```n``` 和可能发生的错误 ```err```。如果 ```n < len(p)```，说明数据源已经没有更多数据可读或者发生了部分读取错误。如果 ```err == nil```，则表示成功读取 ```n``` 个字节；如果 ```err == io.EOF```，则表示数据源已经结束；如果 ```err != nil && err != io.EOF```，则表示发生了其他错误。

使用 ```Read``` 方法从一个字符串中读取数据：

```go
package main

import (
	"fmt"
	"io"
	"strings"
)

func main() {
	r := strings.NewReader("Hello, world!")
	buf := make([]byte, 4)
	for {
		n, err := r.Read(buf)
		fmt.Printf("n = %v, err = %v\n", n, err)
		fmt.Printf("buf[:n] = %q\n", buf[:n])
		if err == io.EOF {
			break
		}
	}
}
```

运行结果：

```
n = 4, err = <nil>
buf[:n] = "Hell"
n = 4, err = <nil>
buf[:n] = "o, w"
n = 4, err = <nil>
buf[:n] = "orld"
n = 1, err = <nil>
buf[:n] = "!"
n = 0, err = EOF
buf[:n] = ""
```

从结果可以看出，每次调用 ```Read``` 方法，都会从字符串中读取最多 4 个字节，并返回实际读取的字节数和错误。当读取到最后一个字节时，返回 ```n=1``` 和 ```err=nil```；当再次调用 ```Read``` 方法时，返回 ```n=0``` 和 ```err=io.EOF```，表示数据源已经结束。

### ReadFull 函数

```ReadFull``` 函数是一个辅助函数，它封装了对 ```Read``` 方法的多次调用，直到填满指定的字节切片或者发生错误为止。它的定义如下：

```go
func ReadFull(r Reader, buf []byte) (n int, err error)
```

它返回实际读取的字节数 ```n``` 和可能发生的错误 ```err```。如果 ```n == len(buf)```，则说明成功读取了 ```len(buf)``` 个字节；如果 ```n < len(buf)```，则说明数据源已经没有更多数据可读或者发生了部分读取错误。

使用 ```ReadFull``` 函数从一个字符串中读取数据：

```go
package main

import (
	"fmt"
	"io"
	"strings"
)

func main() {
	r := strings.NewReader("Hello, world!")
	buf := make([]byte, 8)
	n, err := io.ReadFull(r, buf)
	fmt.Printf("n = %v, err = %v\n", n, err)
	fmt.Printf("buf[:n] = %q\n", buf[:n])
	n, err = io.ReadFull(r,buf)
	fmt.Printf("n=%v,err=%v\n", n,err)
	fmt.Printf("buf[:n]=%q\n", buf[:n])
}
```

从结果可以看出，第一次调用 ```ReadFull``` 函数，成功读取了 8 个字节，并返回 ```n=8``` 和 ```err=nil```；第二次调用 ```ReadFull``` 函数，只能读取剩余的 5 个字节，并返回 ```n=5``` 和 ```err=io.ErrUnexpectedEOF```，表示数据源已经结束。

### io.Writer 接口

```o.Writer``` 接口定义了一个方法：

```go
Write(p []byte) (n int, err error)
```

它将 ```p``` 中的 ```len(p)``` 个字节写入到数据目标中，并返回实际写入的字节数 ```n``` 和可能发生的错误 ```err```。如果 ```n < len(p)```，说明数据目标已经没有更多空间可写或者发生了部分写入错误。

```io.Writer``` 接口是很多其他接口的基础，例如 ```io.WriteCloser```、```io.WriteSeeker``` 等。它也被很多标准库中的类型实现，例如 ```os.File```、```bytes.Buffer```、```strings.Builder``` 等。

### Write 方法

```Write``` 方法是 ```io.Writer``` 接口唯一必须实现的方法。它会尝试将 ```p``` 中的所有字节写入到数据目标中，并返回实际写入的字节数 ```n``` 和可能发生的错误 ```err```。如果 ```n == len(p)```，则表示成功写入了 ```len(p)``` 个字节；如果 ```n < len(p)```，则表示数据目标已经没有更多空间可写或者发生了部分写入错误。

使用 Write 方法向一个字符串构建器中写入数据：

```go
package main

import (
	"fmt"
	"strings"
)

func main() {
	var b strings.Builder
	n, err := b.Write([]byte("Hello"))
	fmt.Printf("n = %v, err = %v\n", n, err)
	fmt.Printf("b.String() = %q\n", b.String())
	n, err = b.Write([]byte(", world!"))
	fmt.Printf("n = %v, err = %v\n", n,err)
	fmt.Printf("b.String()=%q\n", b.String())
}
```

运行结果：

```
n = 5, err = <nil>
b.String() = "Hello"
n=7,err=<nil>
b.String()="Hello, world!"
```

从结果可以看出，每次调用 ```Write``` 方法，都会向字符串构建器中追加指定的字节，并返回实际写入的字节数和错误。由于字符串构建器有足够的空间可写，所以不会发生错误。

## strconv

```strconv``` 包用于实现基本数据类型和字符串之间的转换。```strconv``` 包中包含了很多有用的函数，例如 ```Atoi```，```Itoa```，```ParseBool```，```FormatBool``` 等。

### Atoi 和 Itoa 函数

```Atoi``` 函数用于将一个字符串转换为一个 ```int``` 类型，它的定义如下：

```go
func Atoi(s string) (int, error)
```

它返回转换后的 ```int``` 值和可能发生的错误。如果 ```s``` 是一个有效的十进制整数表示，那么 ```Atoi``` 函数会成功返回对应的 ```int``` 值；如果 ```s``` 是一个空字符串或者包含非数字字符，那么 ```Atoi``` 函数会返回 0 和一个错误。

举例：

```go
package main

import (
	"fmt"
	"strconv"
)

func main() {
	s1 := "42"
	i1, err1 := strconv.Atoi(s1)
	fmt.Printf("s1 = %q, i1 = %v, err1 = %v\n", s1,i1,err1)
	s2 := "-42"
	i2,err2 := strconv.Atoi(s2)
	fmt.Printf("s2=%q,i2=%v,err2=%v\n", s2,i2,err2)
	s3 := "42.0"
	i3,err3 := strconv.Atoi(s3)
	fmt.Printf("s3=%q,i3=%v,err3=%v\n", s3,i3,err3)
}
```

运行结果：

```
s1 = "42", i1 = 42, err1 = <nil>
s2="-42",i2=-42,err2=<nil>
s3="42.0",i3=0,err3=strconv.Atoi: parsing "42.0": invalid syntax
```

从结果可以看出，当 ```s``` 是一个有效的十进制整数表示时，```Atoi``` 函数会成功返回对应的 ```int``` 值和 ```nil``` 错误；当 ```s``` 是一个空字符串或者包含非数字字符时，```Atoi``` 函数会返回 0 和一个错误。

```Itoa``` 函数用于将一个 ```int``` 类型转换为一个字符串。它的定义如下：

```go
func Itoa(i int) string
```

它返回转换后的字符串。它总是使用十进制格式，并且不会产生错误。

举例：

```go
package main

import (
	"fmt"
	"strconv"
)

func main() {
	i1 := 42
	s1 := strconv.Itoa(i1)
	fmt.Printf("i1 = %v, s1 = %q\n", i1,s1)
	i2 := -42
	s2:=strconv.Itoa(i2)
	fmt.Printf("i2=%v,s2=%q\n", i2,s2)
}
```

运行结果：

```
i1 = 42, s1 = "42"
i2=-42,s2="-42"
```

从结果可以看出，```Itoa``` 函数可以将任意 ```int``` 值转换为对应的十进制字符串。

### ParseBool 和 FormatBool 函数

```ParseBool``` 函数用于将一个字符串转换为一个 ```bool``` 类型 。它的定义如下：

```go
func ParseBool(str string) (bool, error)
```

它返回转换后的 ```bool``` 值和可能发生的错误。如果 ```str``` 是一个有效的布尔值表示，那么 ```ParseBool``` 函数会成功返回对应的 ```bool``` 值；如果 ```str``` 是一个空字符串或者包含非布尔值字符，那么 ```ParseBool``` 函数会返回 false 和一个错误。

举例：

```go
package main

import (
	"fmt"
	"strconv"
)

func main() {
	s1 := "true"
	b1,err1 := strconv.ParseBool(s1)
	fmt.Printf("s1 = %q, b1 = %v, err1 = %v\n", s1,b1,err1)
	s2 := "false"
	b2,err2 := strconv.ParseBool(s2)
	fmt.Printf("s2=%q,b2=%v,err2=%v\n", s2,b2,err2)
	s3 := "TRUE"
	b3,err3 := strconv.ParseBool(s3)
	fmt.Printf("s3=%q,b3=%v,err3=%v\n", s3,b3,err3)
	s4 := "0"
	b4,err4 := strconv.ParseBool(s4)
	fmt.Printf("s4=%q,b4=%v,err4=%v\n", s4,b4,err4)
}
```

运行结果：

```
s1 = "true", b1 = true, err1 = <nil>
s2="false",b2=false,err2=<nil>
s3="TRUE",b3=true,err3=<nil>
s4="0",b4=false,err4=<nil>
```

从结果可以看出，```ParseBool``` 函数可以接受以下字符串作为有效的布尔值表示：```"true"```、```"false"```、```"True"```、```"False"```、```"TRUE"```、```"FALSE"```、```"t"```、```"f"```、```"T"```、```"F"```、```"0"``` 和 ```"1"```。其他字符串都会导致错误。

```FormatBool``` 函数用于将一个 ```bool``` 类型转换为一个字符串 。它的定义如下：

```go
func FormatBool(b bool) string
```

它返回转换后的字符串。它总是使用小写字母，并且不会产生错误。

举例：

```go
package main

import (
	"fmt"
	"strconv"
)

func main() {
	b1 := true
	s1:=strconv.FormatBool(b1)
	fmt.Printf("b1 = %v, s1 = %q\n", b1,s1)
	b2:=false
	s2:=strconv.FormatBool(b2)
	fmt.Printf("b2=%v,s2=%q\n", b2,s2)
}
```

运行结果：

```
b1 = true, s1 = "true"
b2=false,s2="false"
```

从结果可以看出，```FormatBool``` 函数可以将任意 ```bool``` 值转换为对应的小写字母字符串。

### ParseInt 和 FormatInt 函数

```ParseInt``` 函数用于将一个字符串转换为一个 ```int64``` 类型 。它的定义如下：

```go
func ParseInt(s string, base int, bitSize int) (i int64, err error)
```

它返回转换后的 ```int64``` 值和可能发生的错误。```s``` 是要转换的字符串，```base``` 是进制数（2 到 ```36），bitSize``` 是结果必须能适应的位大小（0 到 64）。如果 ```s``` 是一个有效的整数表示，那么 ParseInt 函数会成功返回对应的 ```int64``` 值；如果 ```s``` 是一个空字符串或者包含非数字字符，那么 ```ParseInt``` 函数会返回 0 和一个错误。

举例：

```go
package main

import (
	"fmt"
	"strconv"
)

func main() {
	s1 := "42"
	i1,err1 := strconv.ParseInt(s1,10,32)
	fmt.Printf("s1 = %q, i1 = %v, err1 = %v\n", s1,i1,err1)
	s2 := "101010"
	i2,err2 := strconv.ParseInt(s2,2,32)
	fmt.Printf("s2=%q,i2=%v,err2=%v\n", s2,i2,err2)
	s3 := "FF"
	i3,err3 := strconv.ParseInt(s3,16,32)
	fmt.Printf("s3=%q,i3=%v,err3=%v\n", s3,i3,err3)
	s4 := "9223372036854775808"
	i4,err4 := strconv.ParseInt(s4,10,64)
	fmt.Printf("s4=%q,i4=%v,err4=%v\n", s4,i4,err4)
}
```

运行结果：

```
s1 = "42", i1 = 42, err1 = <nil>
s2="101010",i2=42,err2=<nil>
s3="FF",i3=255,err3=<nil>
s4="9223372036854775808",i4=0,err4=strconv.ParseInt: parsing "9223372036854775808": value out of range
```

从结果可以看出，```ParseInt``` 函数可以接受不同进制数的字符串作为有效的整数表示，并根据 ```bitSize``` 参数来检查结果是否溢出。其他字符串都会导致错误。

```FormatInt``` 函数用于将一个 ```int64``` 类型转换为一个字符串 。它的定义如下：

```go
func FormatInt(i int64, base int) string
```

它返回转换后的字符串。```base``` 是进制数（2 到 36）。它总是使用小写字母，并且不会产生错误。

举例：

```go
package main

import (
	"fmt"
	"strconv"
)

func main() {
	i1 := 42
	s1:=strconv.FormatInt(i1,10)
	fmt.Printf("i1 = %v,s1 = %q\n", i1,s1)
	i2:=42
	s2:=strconv.FormatInt(i2,16)
	fmt.Printf("i2=%v,s2=%q\n", i2,s2)
}
```

运行结果：

```
i1 = 42,s1 = "42"
i2=42,s2="29"
```

从结果可以看出，```FormatInt``` 函数可以将任意 ```int64``` 值转换为对应进制数的小写字母字符串。

### ParseFloat 和 FormatFloat 函数

```ParseFloat``` 函数用于将一个字符串转换为一个 ```float64``` 类型 。它的定义如下：

```go
func ParseFloat(s string, bitSize int) (f float64, err error)
```

它返回转换后的 ```float64``` 值和可能发生的错误。```s``` 是要转换的字符串，```bitSize``` 是结果必须能适应的位大小（32 或 64）。如果 ```s``` 是一个有效的浮点数表示，那么 ```ParseFloat``` 函数会成功返回对应的 ```float64``` 值；如果 ```s``` 是一个空字符串或者包含非数字字符，那么 ```ParseFloat``` 函数会返回 0 和一个错误。

举例：

```go
package main

import (
	"fmt"
	"strconv"
)

func main() {
	s1 := "3.14"
	f1,err1 := strconv.ParseFloat(s1,32)
	fmt.Printf("s1 = %q,f1 = %v,err1 = %v\n", s1,f1,err1)
	s2 := "3.14159265358979323846264338327950288419716939937510582097494459"
	f2,err2 := strconv.ParseFloat(s2,32)
	fmt.Printf("s2=%q,f2=%v,err2=%v\n", s2,f2,err2)
	s3 := "3.14159265358979323846264338327950288419716939937510582097494459"
	f3,err3 := strconv.ParseFloat(s3,64)
	fmt.Printf("s3=%q,f3=%v,err3=%v\n", s3,f3,err3)
	s4 := "NaN"
	f4,err4 := strconv.ParseFloat(s4,64)
	fmt.Printf("s4=%q,f4=%v,err4=%v\n", s4,f4,err4)
}
```

运行结果：

```
s1 = "3.14",f1 = 3.14,err1 = <nil>
s2="3.14159265358979323846264338327950288419716939937510582097494459",f2=3.1415927,err2=<nil>
s3="3.14159265358979323846264338327950288419716939937510582097494459",f3=3.141592653589793,err3=<nil>
s4="NaN",f4=NaN,err4=<nil>
```

从结果可以看出，```ParseFloat``` 函数可以接受任意格式的浮点数字符串作为有效的浮点数表示，并根据 ```bitSize``` 参数来检查结果是否溢出或丢失精度。其他字符串都会导致错误。

```FormatFloat``` 函数用于将一个 ```float64``` 类型转换为一个字符串。它的定义如下：

```go
func FormatFloat(f float64, fmt byte, prec int, bitSize int) string
```

它返回转换后的字符串。```fmt``` 是格式标记（```‘b’```、```‘e’```、```‘E’```、```‘f’```、```‘g’``` 或 ```‘G’```），```prec``` 是精度（对于 ``‘b’``、```‘e’```、```‘E’```、和 ```‘f’``` 表示小数点后面数字个数；对于 ```‘g’```、```‘G’```, 表示总数字个数），```bitSize``` 是原始值必须能适应的位大小（32 或 64）。它总是使用小写字母，并且不会产生错误。

举例：

```go
package main

import (
	"fmt"
	"strconv"
)

func main() {
	f1 := 1234567890.1234567890
	s1:=strconv.FormatFloat(f1,'e',10,32)
	fmt.Printf("f1 = %v,s1 = %q\n", f1,s1)
	f2:=1234567890.1234567890
	s2:=strconv.FormatFloat(f2,'g',10,32)
	fmt.Printf("f2=%v,s2=%q\n", f2,s2)
}
```

运行结果：

```
f1 = 1234567890.123456
```

## flag

```flag``` 包实现了命令行参数的解析，```flag``` 包使得开发命令行工具更为简单。```flag``` 包支持的命令行参数类型有 ```bool```、```int```、```int64```、```uint```、```uint64```、```float```、```float64```、```string```、```duration```等。

有两种常用的定义命令行 flag 参数的方法：

```go
flag.Type(flag名, 默认值, 帮助信息) *Type
flag.TypeVar(Type指针, flag名, 默认值, 帮助信息)
```

其中 ```Type``` 是指参数类型，如 ``` String```、```Int``` 等。

定义好 ```flag``` 参数后，需要调用 ```flag.Parse()``` 来解析命令行参数，并把结果存入相应的变量中。

```go
package main

import (
	"flag"
	"fmt"
)

func main() {
	// 定义两个 flag 参数
	name := flag.String("name", "Gonoob", "your name")
	age := flag.Int("age", 18, "your age")

	// 解析命令行参数
	flag.Parse()

	// 打印结果
	fmt.Printf("Hello, %s! You are %d years old.\n", *name, *age)
}
```

以上代码使用了 ```flag.String()``` 和 ```flag.Int()``` 方法来定义两个 ```flag``` 参数，分别是 ```name``` 和 ```age```。然后调用 ```flag.Parse()``` 来解析命令行参数，并打印出结果。

运行结果如下：

```bash
$ go run main.go -h
Usage of /tmp/gonoob/main:
  -age int
    	your age (default 18)
  -name string
    	your name (default "Gonoob")
exit status 2

$ go run main.go 
Hello, Gonoob! You are 18 years old.

$ go run main.go -name Alice -age 25
Hello, Alice! You are 25 years old.
```

## reflect

Go 语言中的反射功能由 ```reflect``` 包提供。反射是指在程序运行时，能够获取和修改未知变量的值、类型和方法的能力。

```reflect``` 包定义了两个重要的类型：```reflect.Type``` 和 ```reflect.Value```。

```reflect.Type``` 是一个接口，它表示一个 Go 类型。它有很多方法可以获取类型的信息，如名称、种类、字段、方法等。```reflect.Value``` 是一个结构体，它表示一个 Go 值。它也有很多方法可以获取和设置值的信息，如类型、地址、长度、元素等。

要获取任意对象的 ```reflect.Type``` 和 ```reflect.Value```，可以使用 ```reflect``` 包提供的两个函数：```reflect.TypeOf``` 和 ```reflect.ValueOf```。

举例：

```go
package main

import (
	"fmt"
	"reflect"
)

func main() {
	// 定义一个字符串变量
	s := "Hello, Gonoob!"

	// 获取变量的类型和值
	t := reflect.TypeOf(s)
	v := reflect.ValueOf(s)

	// 打印结果
	fmt.Println("Type:", t)
	fmt.Println("Value:", v)
}
```

运行结果如下：

```
Type: string
Value: Hello, Gonoob!
```

使用了 ```reflect.TypeOf``` 和 ```reflect.ValueOf``` 函数来获取一个字符串变量的类型和值，并打印出结果。

## http

```http``` 包提供了创建 ```http``` 服务或者访问 ```http``` 服务所需要的能力。

它包含了两个核心接口：```net/http.RoundTripper``` 和 ````net/http.Handler````。```RoundTripper``` 接口定义了如何发送一个 ```HTTP``` 请求并返回一个 ```HTTP``` 响应，它是客户端的抽象。```Handler``` 接口定义了如何处理一个 ```HTTP``` 请求并生成一个 ```HTTP``` 响应，它是服务端的抽象。

### 客户端

如果你想创建一个 ```HTTP``` 客户端，你可以使用 ```http.Get```、```http.Post```、```http.Head``` 等函数发送请求，并获取返回的 ```*http.Response``` 结构体。

举例：

```go
resp, err := http.Get("https://www.baidu.com")
if err != nil {
    // handle error
}
defer resp.Body.Close()
// read resp.Body
```

### 服务端

如果你想创建一个 ```HTTP``` 服务端，你可以使用 ```http.HandleFunc``` 或者 ```http.Handle``` 函数注册处理器函数或者处理器对象，并使用 ```http.ListenAndServe``` 函数启动服务器并监听指定的地址和端口。

举例：

```go
func hello(w http.ResponseWriter, r *http.Request) {
    fmt.Fprintln(w, "Hello, world!")
}

func main() {
    // 注册处理器函数
    http.HandleFunc("/hello", hello)
    // 启动服务器
    err := http.ListenAndServe(":8080", nil)
    if err != nil {
        // handle error
    }
}
```