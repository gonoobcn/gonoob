# 泛型

Go 1.18 及以上版本新增了泛型。

在函数中声明泛型，使得这个函数能够成为一个通用的函数。简单来说，原本同样的代码逻辑，你需要编写多个函数，使用了泛型之后，你只需要编写一个函数就行了。

举个例子，如果没有使用泛型，你定义的函数可能是这样的：

```go
func SumInt(a, b int) int {
	return a + b
}

func SumFloat(a, b float64) float64 {
	return a + b
}

func main() {
	si := SumInt(1, 2)
	sf := SumFloat(1.5, 3.5)
	fmt.Printf("s1 = %v, s2 = %v", si, sf)  // s1 = 3, s2 = 5
}
```

使用了泛型之后，你定义的函数是这样的：

```go
func Sum[T int | float64](a, b T) T {
	return a + b
}

func main() {
	si := Sum(1, 2)
	sf := Sum(1.5, 3.5)
	fmt.Printf("s1 = %v, s2 = %v", si, sf)  // s1 = 3, s2 = 5
}
```

很明显，如果没有使用泛型，相同功能的函数你可能要编写两个；使用了泛型之后，你只需要编写一个函数而不是两个。

再看一个例子，来自官方文档中提供的例子。

没有使用泛型：

```go
// SumInts adds together the values of m.
func SumInts(m map[string]int64) int64 {
	var s int64
	for _, v := range m {
		s += v
	}
	return s
}

// SumFloats adds together the values of m.
func SumFloats(m map[string]float64) float64 {
	var s float64
	for _, v := range m {
		s += v
	}
	return s
}

func main() {
	// Initialize a map for the integer values
	ints := map[string]int64{
		"first":  34,
		"second": 12,
	}

	// Initialize a map for the float values
	floats := map[string]float64{
		"first":  35.98,
		"second": 26.99,
	}

	fmt.Printf("Non-Generic Sums: %v and %v\n",
		SumInts(ints),
		SumFloats(floats))  // Non-Generic Sums: 46 and 62.97
}
```

使用了泛型：
```go
// SumIntsOrFloats sums the values of map m. It supports both int64 and float64
// as types for map values.
func SumIntsOrFloats[K comparable, V int64 | float64](m map[K]V) V {
    var s V
    for _, v := range m {
        s += v
    }
    return s
}

func main() {
	// Initialize a map for the integer values
	ints := map[string]int64{
		"first":  34,
		"second": 12,
	}

	// Initialize a map for the float values
	floats := map[string]float64{
		"first":  35.98,
		"second": 26.99,
	}

	fmt.Printf("Generic Sums: %v and %v\n",
    SumIntsOrFloats[string, int64](ints),
    SumIntsOrFloats[string, float64](floats))   // Non-Generic Sums: 46 and 62.97
}
```

在以上代码中，声明一个 ```SumIntsOrFloats``` 具有两个泛型参数（在方括号内）```K``` 和 ```V```，以及一个函数参数 ```m``` ，类型为 ```map[K]V``` ，该函数返回值为 ```V```。

泛型参数 ```K``` 指定类型约束为 ```comparable```。```comparable``` 是 Go 中预先声明的约束，它允许其值可用作比较运算符 ```==``` 的任何类型（比如，numbers、strings、pointers、booleans 等等)。

```comparable``` 在源码中的定义：

```go
// comparable is an interface that is implemented by all comparable types
// (booleans, numbers, strings, pointers, channels, arrays of comparable types,
// structs whose fields are all comparable types).
// The comparable interface may only be used as a type parameter constraint,
// not as the type of a variable.
type comparable interface{ comparable }
```

把注释翻译过来就是：```compatible``` 是由所有可比较类型实现的接口（布尔值、数字、字符串、指针、通道、可比较类型的数组、字段都是可比较类型的结构），```compatible``` 只能用作泛型参数的约束。

同时，Go 要求 ```map``` 的 ```key``` 也是可比较的。所以声明 ```comparable``` 是必要的，这样 K 在 map 变量中就可以用作键。它还确保调用代码使用映射键的允许类型。

为 ```V``` 泛型参数指定一个约束，它是两种类型的联合：int64 和 float64。你可以使用 ```|``` 指定两种或多种类型的联合，这意味着此约束允许任何一种类型。编译器将允许任何一种类型作为调用代码中的参数。

指定 ```m``` 参数的类型 ```map[K]V```，其中 ```K``` 和 ```V``` 是已经为类型参数指定的类型。

请注意，我们知道 ```map[K]V``` 是一个有效的映射类型，因为 ```K``` 它是一个可比较的类型。如果我们没有声明 ```K comparable```，编译器将会报错。

## 声明泛型

在函数、切片、map、通道中声明泛型：

```go
// 泛型函数
func Sum[T int | float64 | string](s []T) T {
	var sum T
	for _, v := range s {
		sum += v
	}
	return sum
}

// any 表示任意类型
// 在源码中的定义：type any = interface{}
func Print[T any](v T) {
	fmt.Println(v)
}

// 泛型切片
type sss[T any] []T

// 泛型map
type mmm[K string, V int64] map[K]V

// 泛型通道
type ccc[T any] chan T

func main() {

	s1 := Sum([]int{1, 2, 3, 5, 8})
	s2 := Sum([]float64{1.2, 3.2, 5.8})
	s3 := Sum([]string{"Hello", "GoNoob"})
	fmt.Printf("%v, %v, %s", s1, s2, s3) // 19, HelloGoNoob

	Print(2.55)           // 2.55
	Print([]int{1, 2, 3}) // [1 2 3]

	Print(struct {
		Name string
		Age  int
	}{"GoNoob", 1}) // {GoNoob 1}
	Print("Hello GoNoob!") // Hello GoNoob!

	fmt.Println(sss[int]{1, 3, 6})                          // [1 3 6]
	fmt.Println(mmm[string, int64]{"a": 1, "b": 3, "c": 6}) // map[a:1 b:3 c:6]

	c := make(ccc[int], 5)
	c <- 2
	fmt.Println(<-c) // 2
}
```

## 泛型类型约束

### 使用 ````interface```` 中规定的类型来约束泛型的参数

将类型约束声明为接口：

```go
type Number interface {
    int64 | float64
}
```

在以上代码中，声明 ```Number``` 接口类型作为类型约束，在接口内部声明 int64 和 float64 的联合。

本质上，您是将联合从函数声明移动到新的类型约束中。

然后你就可以这样声明泛型的类型约束：

```go
func SumNumbers[K comparable, V Number](m map[K]V) V {
    var s V
    for _, v := range m {
        s += v
    }
    return s
}
```

如果你是这样声明类型约束：

```go
type Number interface {
    ~int64 | ~float64 | ~string
}
```

除了 ```int64```、```float64```、```string``` 这三种类型能够被允许，它还会允许这三种类型的自定义类型，比如：

```go
type myInt64 int64
type myFloat64 float64
type myString string
```

### 使用 ```interface``` 中规定的方法来约束泛型的参数

```go
// 定义泛型类型约束
type printString interface {
	String() string
	~ int | ~float64
}

// 自定义类型
type numberInt int
type numberFloat64 float64

// 实现 printString 接口的的 String 方法
func (i numberInt) String() string {
	return strconv.Itoa(int(i))
}
func (f numberFloat64) String() string {
	return strconv.FormatFloat(float64(f), 'g', 5 ,32)
}

// 将数值转成字符串
func NumberToString[T printString](n T) string {
	return n.String()
}

func main() {

	fmt.Println(NumberToString[numberInt](12))	// 12
	fmt.Println(NumberToString[numberFloat64](23.1293018290892))	// 23.129

	fmt.Println(NumberToString[int](12))	// error: int does not implement printString (missing method String)
}
```

在以上代码中，根据泛型类型约束的不同，会调用不同的 ```String()``` 方法。

泛型类型约束 ```printString``` 接口中规定，泛型参数必须是实现了 ```String()``` 方法的类型，所以泛型参数只能是 ```numberInt``` 和 ```numberFloat64```。

### 使用预定义的接口来约束泛型的参数

在 Go 的标准库中，有几个预定的泛型类型约束接口：

```go
// 任何类型
type any = interface{}

// 所有可以比较的类型（布尔值、数字、字符串、指针、通道、可比较类型的数组、字段都是可比较类型的结构）
type comparable interface{ comparable }
```

你可以直接拿来用：

```go
func GetSum[K comparable, V int | float64](m map[K]V) V {
	var s V
	for _, v := range m {
		s += v
	}
	return s
}

func PrinfSum[T any](s T) {
	fmt.Printf("%v", s)
}

func main() {
	PrinfSum(GetSum(map[string]int{"a": 100, "b": 200, "c": 300})) // 600
}
```











