# 如何使用 log 包进行错误处理？

Go 语言引入了一个关于错误处理的标准模式，即 ```error``` 接口，该接口的定义如下：

```go
type error interface {
	Error() string
}
```

创建 ```error``` 对象通常使用 ```errors``` 包提供的 ```errors.New``` 函数：

```go
var e error = errors.New("some error")
```

对于大多数函数，如果要返回错误，大致上都可以定义为如下模式，将 ```error``` 作为多种返回值中的最后一个：

```go
func doSomething() (result type, err error) {
	// do something
	if someError {
		return nil, errors.New("some error")
	}
	return result, nil
}
```

在调用可能返回错误的函数时，通常使用 ```if``` 语句来检查并处理错误：

```go
result, err := doSomething()
if err != nil {
	// handle error
	log.Println(err)
	return
}
// use result
```

```log``` 包中提供了 ```Fatal``` 系列函数和 ```Panic``` 系列函数来打印错误日志并退出程序或引发 ```panic``` 异常，这些函数一般用于处理严重的或不可恢复的错误情况。例如：

```go
file, err := os.Open("somefile.txt")
if err != nil {
	// handle file open error
	log.Fatal(err) // print error and exit program with code 1
}
defer file.Close()
// use file

data, err := ioutil.ReadAll(file)
if err != nil {
	// handle read file error
	log.Panic(err) // print error and panic 
}
// use data
```