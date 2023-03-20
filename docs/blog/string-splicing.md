# 字符串拼接

Go 中的字符串 ```string``` 是不可变的，因此字符串之间的拼接实际上是创建了一个新的字符串。如果频繁的进行字符串拼接，那将会对性能产生严重的影响。

字符串拼接的几种方式：

| 常见的拼接方式 | 性能 |
| ------- | -- |
| + | 低 |
| fmt.Sprintf | 低 |
| strings.Join | 低 |
| strings.Builder | 高 |
| bytes.Buffer | 高 |
| [] byte | 高 |

## 使用 ```+```

```go
// 使用 + 
func UsePlus(str []string) string {
	var s string
	for i := 0; i < len(str); i++ {
		s += str[i]
	}
	return s
}
```

## 使用 ```fmt.Sprintf```

```go
// 使用 fmt.Sprintf
func UseSprintf(str []string) string {
	var s string
	for i := 0; i < len(str); i++ {
		s = fmt.Sprintf("%s%s", s, str[i])
	}
	return s
}
```

## 使用 ```strings.Join```

```go
// 使用 strings.Join
func UseJoin(str []string) string {
	var s string
	for i := 0; i < len(str); i++ {
		s = strings.Join(str, "")
	}
	return s
}
```

## 使用 ```strings.Builder```

```go
// 使用 strings.Builder
func UseBuilder(str []string) string {
	var b strings.Builder
	for i := 0; i < len(str); i++ {
		b.WriteString(str[i])
	}
	return b.String()
}
```

## 使用 ```bytes.Buffer```

```go
// 使用 bytes.Buffer
func UseBuffer(str []string) string {
	var b bytes.Buffer
	for i := 0; i < len(str); i++ {
		b.WriteString(str[i])
	}
	return b.String()
}
```

## 使用 ```[] byte```

```go
// 使用 []byte
func UseByte(str []string) string {
	b := make([]byte, 0)
	for _, v := range str {
		b = append(b, v...)
	}
	return string(b)
}
```

## 性能测试

对以上几个方法进行性能测试：

```go
package gotest

import "testing"

var arr = [...]string{"abc", "b", "c", "d", "ez", "f", "g", "h", "he", "l", "l", "o", "gs", "oo", "n", "o", "o", "b"}
var sss []string = arr[:]

func BenchmarkPlusConcat(b *testing.B) {
	for i := 0; i < b.N; i++ {
		UsePlus(sss)
	}
}
func BenchmarkSprintfConcat(b *testing.B) {
	for i := 0; i < b.N; i++ {
		UseSprintf(sss)
	}
}
func BenchmarkJoinConcat(b *testing.B) {
	for i := 0; i < b.N; i++ {
		UseJoin(sss)
	}
}
func BenchmarkBuilderConcat(b *testing.B) {
	for i := 0; i < b.N; i++ {
		UseBuilder(sss)
	}
}
func BenchmarkBufferConcat(b *testing.B) {
	for i := 0; i < b.N; i++ {
		UseBuffer(sss)
	}
}
func BenchmarkByteConcat(b *testing.B) {
	for i := 0; i < b.N; i++ {
		UseByte(sss)
	}
}
```

测试结果：

```
$ go test -benchmem -bench=.
goos: darwin
goarch: amd64
pkg: gonoob.cn/gotest
cpu: Intel(R) Core(TM) i5-5257U CPU @ 2.70GHz
BenchmarkPlusConcat-4            1430535               776.0 ns/op           288 B/op         17 allocs/op
BenchmarkSprintfConcat-4          299164              3471 ns/op             848 B/op         53 allocs/op
BenchmarkJoinConcat-4             344154              3478 ns/op             432 B/op         18 allocs/op
BenchmarkBuilderConcat-4         6386217               181.8 ns/op            56 B/op          3 allocs/op
BenchmarkBufferConcat-4          6014815               188.9 ns/op            88 B/op          2 allocs/op
BenchmarkByteConcat-4            6709759               170.8 ns/op            56 B/op          3 allocs/op
PASS
ok      gonoob.cn/gotest        9.963s
```

通过对比发现，使用 ```+```、```fmt.sprintf()```、```strings.Join``` 拼接字符串的性能比较低，而使用 ```strings.Builder```、```bytes.Buffer```、```[] byte``` 拼接字符串的性能和占用内存相差不多。







