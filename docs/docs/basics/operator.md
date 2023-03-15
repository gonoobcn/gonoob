# 运算符

Go 语言的运算符有算术运算符、关系运算符、逻辑运算符、位运算符和赋值运算符。

## 算术运算符

| 运算符 | 说明 |
| --- | -- |
| + | 相加 |
| - | 相减 |
| * | 相乘 |
| / | 相除 |
| % | 取余 |
| ++ | 自增 |
| -- | 自减 |

```go
package main

import "fmt"

func main() {

   var a int = 30
   var b int = 12

   fmt.Printf("a + b 的值为 %d\n", a + b)
   fmt.Printf("a - b 的值为 %d\n", a - b)
   fmt.Printf("a * b 的值为 %d\n", a * b)
   fmt.Printf("a / b 的值为 %d\n", a / b)
   fmt.Printf("a %% b 的值为 %d\n", a % b)

   a++ // 等价于 a = a + 1
   fmt.Printf("a++ 的值为 %d\n", a)
   b-- // 等价于 b = b - 1
   fmt.Printf("a-- 的值为 %d\n", b)
}
```

输出结果：

```
a + b 的值为 42
a - b 的值为 18
a * b 的值为 360
a / b 的值为 2
a % b 的值为 6
a++ 的值为 31
a-- 的值为 11
```

## 关系运算符

| 运算符 | 说明 |
| --- | -- |
| == | 判断两个值是否相等，如果相等返回 true 否则返回 false |
| != | 判断两个值是否不相等，如果不相等返回 true 否则返回 False |
| >	| 判断左边值是否大于右边值，如果是返回 true 否则返回 false |
| <	| 判断左边值是否小于右边值，如果是返回 true 否则返回 false |
| >= | 判断左边值是否大于等于右边值，如果是返回 true 否则返回 false |
| <= | 判断左边值是否小于等于右边值，如果是返回 true 否则返回 false |

```go
package main

import "fmt"

func main() {

   var a int = 30
   var b int = 12

   fmt.Printf("a == b 的值为 %t\n", a == b)
   fmt.Printf("a < b 的值为 %t\n", a < b)
   fmt.Printf("a > b 的值为 %t\n", a > b)
   fmt.Printf("a <= b 的值为 %t\n", a <= b)
   fmt.Printf("b >= a 的值为 %t\n", b >= a)
}
```

输出结果：

```
a == b 的值为 false
a < b 的值为 false
a > b 的值为 true
a <= b 的值为 false
b >= a 的值为 false
```

## 逻辑运算符

| 运算符 | 说明 |
| --- | -- |
| && | 逻辑 AND 运算符，如果两边的操作数都是 true，则条件 true，否则为 false |
| \| | 逻辑 OR 运算符，如果两边的操作数有一个 true，则条件 true，否则为 false |
| !	| 逻辑 NOT 运算符，如果条件为 true，则逻辑 NOT 条件 false，否则为 true |

```go
package main

import "fmt"

func main() {

   var a bool = true
   var b bool = false

   fmt.Printf("a && b 的值为 %t\n", a && b)
   fmt.Printf("a || b 的值为 %t\n", a || b)
   fmt.Printf("!a 的值为 %t\n", !a)
   fmt.Printf("!b 的值为 %t\n", !b)
   fmt.Printf("!(a && b 的值为 %t\n", !(a && b))
}
```

输出结果：

```
a && b 的值为 false
a || b 的值为 true
!a 的值为 false
!b 的值为 true
!(a && b 的值为 true
```

## 位运算符

| 运算符 | 说明 |
| --- | -- |
| & | 参与运算的两数各对应的二进位相与（两位均为1才为1）|
| \| | 参与运算的两数各对应的二进位相或（两位有一个为1就为1）|
| ^ | 参与运算的两数各对应的二进位相异或，当两对应的二进位相异时，结果为1（两位不一样则为1）|
| << | 左移n位就是乘以2的n次方。比如 ```a << b``` 是把 a 的各二进位全部左移 b 位，高位丢弃，低位补0 |
| >> | 右移n位就是除以2的n次方。比如 ```a >> b``` 是把 a 的各二进位全部右移 b 位 |

位运算符对整数在内存中的二进制位进行操作，位运算符 ```&```、```|```、```^``` 的计算：

| x | y	| x & y | x \| y | x ^ y |
| - | - | --- | ---| --- |
| 0	| 0	| 0	| 0	| 0 |
| 0	| 1	| 0	| 1	| 1 |
| 1	| 1	| 1	| 1	| 0 |
| 1	| 0	| 0	| 1	| 1 |

举个例子，假设 a = 80、b = 24，其二进制数转换为：

```
a = 0101 0000
b = 0001 1000

a&b = 0001 0000 // 从第一位开始比较，两位均为1才为1，否则为0
a|b = 0101 1000 // 从第一位开始比较，两位有一个为1就为1，两位均为0才为0
a^b = 0100 1000 // 从第一位开始比较，两位不一样则为1，否则为0
```

```go
package main

import "fmt"

func main() {

   var a int = 80 // 80 = 01010000
   var b int = 24 // 24 = 00011000

   fmt.Printf("a & b 的值为 %d\n", a & b)  // 16 = 00010000
   fmt.Printf("a | b 的值为 %d\n", a | b)  // 88 = 01011000
   fmt.Printf("a ^ b 的值为 %d\n", a ^ b)  // 72 = 01001000
   fmt.Printf("a << 2 的值为 %d\n", a << 2)  // 320 = 101000000
   fmt.Printf("b >= a 的值为 %d\n", a >> 2)  // 20 = 00010100
}
```

输出结果：

```
a & b 的值为 16
a | b 的值为 88
a ^ b 的值为 72
a << 2 的值为 320
b >= a 的值为 20
```

## 赋值运算符

| 运算符 | 描述 |
| --- | -- |
| = | 简单的赋值运算符，将一个表达式的值赋给一个左值 |
| += | 相加后再赋值 |
| -= | 相减后再赋值 |
| *= | 相乘后再赋值 |
| /= | 相除后再赋值 |
| %= | 求余后再赋值 |
| <<= | 左移后赋值 |
| >>= |	右移后赋值 |
| &= | 按位与后赋值 |
| ^= | 按位异或后赋值 |
| \|= | 按位或后赋值 |

```go
package main

import "fmt"

func main() {

   var a int = 30
   var b int

   b = a // 把变量a赋值给b
   fmt.Printf("b = a, b的值为 %d\n", b)

   b += a // 相当于 b = b + a
   fmt.Printf("b += a, b的值为 %d\n", b)

   b -= a // 相当于 b = b - a
   fmt.Printf("b -= a, b的值为 %d\n", b)

   b *= a // 相当于 b = b * a
   fmt.Printf("b *= a, b的值为 %d\n", b)

   b /= a // 相当于 b = b / a
   fmt.Printf("b /= a, b的值为 %d\n", b)

   a <<= 2 // 相当于 a = (a << 2)
   fmt.Printf("a <<= 2, a的值为 %d\n", a)

   a >>= 2 // 相当于 a = (a >> 2)
   fmt.Printf("a >>= 2, a的值为 %d\n", a)

   a &= 2 // 相当于 a = (a &= 2)
   fmt.Printf("a &= 2, a的值为 %d\n", a)

   a ^= 2 // 相当于 a = (a ^= 2)
   fmt.Printf("a ^= 2, a的值为 %d\n", a)

   a |= 2 // 相当于 a = (a |= 2)
   fmt.Printf("a |= 2, a的值为 %d\n", a) 
}
```

输出结果：

```
b = a, b的值为 30
b += a, b的值为 60
b -= a, b的值为 30
b *= a, b的值为 900
b /= a, b的值为 30
a <<= 2, a的值为 120
a >>= 2, a的值为 30
a &= 2, a的值为 2
a ^= 2, a的值为 0
a |= 2, a的值为 2
```
