---
title: "常量"
linkTitle: "常量"
weight: 20
date: 2025-02-27
description: >
  Golang 常量
---


### 常量的定义

常量的定义与变量类似，只不过使用 `const` 关键字。

```go
const Pi = 3.14

func main() {
	const World = "world"
	fmt.Println("Hello", World)
	fmt.Println("Happy", Pi, "Day")

	const Truth = true
	fmt.Println("Go rules?", Truth)
}
```

常量可以是字符、字符串、布尔或数字类型的值。

注意：常量不能使用 `:=` 语法定义。

### 数值常量

数值常量是高精度的 **值**。

```go
const (
	Big   = 1 << 100
	Small = Big >> 99
)
```

一个未指定类型的常量由上下文来决定其类型。

## go语言规范

https://golang.org/ref/spec#Constants

https://moego.me/golang_spec.html#id271

常量有 布尔值常量 、 rune 常量 、 整数常量 、 浮点数常量 、 复数常量 和 字符串常量 。 Rune、整数、浮点数和复数常量统称为数值常量。

常量的值是由如下所表示的： rune，整数，浮点数，虚数，字符串字面值，表示常量的标识符，常量表达式，结果为常量的变量转换，或者一些内置函数所生成的值，这些内置函数比如应用于任意值的 unsafe.Sizeof ，应用于一些表达式 的 cap 或 len ，应用于复数常量的 real 和 imag 以及应用于数值常量的 complex 。布尔值是由预先声明的常量 true 和 false 所代表的。预先声明的标识符 iota 表示一个整数常量。

通常，复数常量是 常量表达式 的一种形式，会在该节讨论。

数值常量代表任意精度的确切值，而且不会溢出。因此，没有常量表示 IEEE-754 负零，无穷，以及非数字值集。

常量可以是有类型的也可以是无类型的。字面值常量， true , false , iota 以及一些仅包含无类型的恒定操作数的 常量表达式 是无类型的。

常量可以通过 常量声明 或 变量转换 被显示地赋予一个类型，也可以在 变量声明 或 赋值 中，或作为一个操作数在 表达式 中使用时隐式地被赋予一个类型。如果常量的值不能按照所对应的类型来表示的话，就会出错。「前一版的内容： 比如， 3.0 可以作为任何整数类型或任何浮点数类型，而 2147483648.0 （相当于 1<<31 ）可以作为 float32 , float64 或 uint32 类型，但不能是 int32 或 string 。」

一个无类型的常量有一个 默认类型 ，当在上下文中需要请求该常量为一个带类型的值时，这个 默认类型 便指向该常量隐式转换后的类型，比如像 i := 0 这样子的 短变量声明 就没有显示的类型。无类型常量的默认类型分别是 bool , rune , int , float64 , complex128 或 string ，取决于它是否是一个布尔值、 rune、整数、浮点数、复数或字符串常量。

实现限制：虽然数值常量在这个语言中可以是任意精度的，但编译器可能会使用精度受限的内部表示法来实现它。也就是说，每一种实现必须：

- 使用最少 256 位来表示整数。
- 使用最少 256 位来表示浮点数常量（包括复数常量的对应部分）的小数部分，使用最少 16 位表示其带符号的二进制指数部分。
- 当无法表示一个整数常量的精度时，需要给出错误。
- 当因为溢出而无法表示一个浮点数或复数常量时，需要给出错误。
- 当因为精度限制而无法表示一个浮点数或复数常量时，约到最接近的可表示的常量。

这些要求也适用于字面值常量，以及 常量表达式 的求值结果。

### Effective Go

https://golang.org/doc/effective_go.html#constants

Go中的常量仅仅是常量。它们是在编译时创建的，即使是在函数中定义为locals，也只能是数字、字符（符文）、字符串或布尔值。由于编译时的限制，定义它们的表达式必须是常量表达式，可被编译器评估。例如，1<<3是一个常量表达式，而math.Sin(math.Pi/4)不是，因为对math.Sin的函数调用需要在运行时发生。

在Go中，使用iota枚举器创建枚举常量。由于 iota 可以成为表达式的一部分，而且表达式可以隐式重复，因此很容易建立复杂的值集。

```go
type ByteSize float64

const (
    _           = iota // ignore first value by assigning to blank identifier
    KB ByteSize = 1 << (10 * iota)
    MB
    GB
    TB
    PB
    EB
    ZB
    YB
)
```

将String这样的方法附加到任何用户定义的类型上的能力，使得任意值在打印时自动格式化成为可能。虽然你最常看到的是它应用于结构体，但这种技术对于标量类型也很有用，比如浮点类型（如ByteSize）。

```go
func (b ByteSize) String() string {
    switch {
    case b >= YB:
        return fmt.Sprintf("%.2fYB", b/YB)
    case b >= ZB:
        return fmt.Sprintf("%.2fZB", b/ZB)
    case b >= EB:
        return fmt.Sprintf("%.2fEB", b/EB)
    case b >= PB:
        return fmt.Sprintf("%.2fPB", b/PB)
    case b >= TB:
        return fmt.Sprintf("%.2fTB", b/TB)
    case b >= GB:
        return fmt.Sprintf("%.2fGB", b/GB)
    case b >= MB:
        return fmt.Sprintf("%.2fMB", b/MB)
    case b >= KB:
        return fmt.Sprintf("%.2fKB", b/KB)
    }
    return fmt.Sprintf("%.2fB", b)
}
```

表达式YB打印为1.00YB，而ByteSize(1e13)打印为9.09TB。

这里使用Sprintf实现ByteSize的String方法是安全的（避免无限期重复），不是因为转换，而是因为它用%f调用Sprintf，而%f不是字符串格式。Sprintf只有在需要字符串时才会调用String方法，而%f需要的是浮点值。

