# 结构体

一个结构体 ```struct``` 就是一组字段 ```field``` 的集合。

声明结构体的基本格式：

```go
type 结构体名称 struct {
	字段名称1 字段类型1 字段标签1
	字段名称2 字段类型2 字段标签2
	...
}
```

结构体初始化：

```go
type Vertex struct {
	X int
	Y int
}

func main() {
	v1 = Vertex{1, 2}  // 创建一个 Vertex 类型的结构体
	v2 = Vertex{X: 1}  // Y:0 被隐式地赋予
	v3 = Vertex{}      // X:0 Y:0
	p  = &Vertex{1, 2} // 创建一个 *Vertex 类型的结构体（指针）
}
```

举个例子：

```go
package main

import "fmt"

type Vertex struct {
	X, Y int
}

var (
	v1 = Vertex{1, 2}  // 创建一个 Vertex 类型的结构体
	v2 = Vertex{X: 1}  // Y:0 被隐式地赋予
	v3 = Vertex{}      // X:0 Y:0
	p  = &Vertex{1, 2} // 创建一个 *Vertex 类型的结构体（指针）
)

func main() {
	fmt.Println(v1, p, v2, v3)
}
```

输出结果：

```
{1 2} &{1 2} {1 0} {0 0}
```

## 结构体字段

结使用 ```.``` 号来访问构体字段：

```go
package main

import "fmt"

type Vertex struct {
	X int
	Y int
}

func main() {
	v := Vertex{1, 2}
	v.X = 4
	fmt.Println(v.X)
}
```

## 结构体标签

带字段标签的结构体：

```go
type Vertex struct {
	X int `json:"x"`
	Y int `json:"y"`
}
```

## 结构体指针

结构体字段可以通过结构体指针来访问。

如果我们有一个指向结构体的指针 ```p```，那么可以通过 ```(*p).X``` 来访问其字段 ```X```。Go 也允许我们使用隐式间接引用，直接写 ```p.X``` 就可以。

```go
package main

import "fmt"

type Vertex struct {
	X int
	Y int
}

func main() {
	v := Vertex{1, 2}
	p := &v
	p.X = 6
	fmt.Println(v)
}
```