---
title: "变量"
linkTitle: "变量"
weight: 30
date: 2025-02-27
description: >
  Golang 变量
---


### 变量定义

`var` 语句定义一个变量的列表；跟函数的参数列表一样，类型在后面。

`var` 语句可以定义在包或函数级别。

```go
var c, python, java bool

func main() {
	var i int
	fmt.Println(i, c, python, java)
}
```

### 变量初始化

```go
// 变量定义可以包含初始值，每个变量对应一个。
var i, j int = 1, 2

func main() {
    // 如果初始化是使用表达式，则可以省略类型；
    // 变量从初始值中获得类型。
	var c, python, java = true, false, "no!"
	fmt.Println(i, j, c, python, java)
}
```

也可以用这个方式初始化多个变量，每个变量一行代码：

```go
var (
	ToBe   bool       = false
	MaxInt uint64     = 1<<64 - 1
	z      complex128 = cmplx.Sqrt(-5 + 12i)
)
```

如果变量在定义时没有明确的初始化，则会赋值为该类型对应的**零值**：

- 数值类型为 `0`，
- 布尔类型为 `false`，
- 字符串为 `""`（空字符串）

### 短声明变量

在函数中，`:=` 简洁赋值语句在明确类型的地方，可以用于替代 `var` 定义。

```go
func main() {
	var i, j int = 1, 2
	k := 3
	c, python, java := true, false, "no!"
}
```

函数外的每个语句都必须以关键字开始（`var`、`func`、等等），`:=` 结构不能使用在函数外。

### 多赋值模式

上面同时赋值多个变量的模式，可以用于if, for 等语句，实现在if, for 语句中的多赋值模式：

```go
for i, j, s := 0, 5, "a"; i < 3 && j < 100 && s != "aaaaa"; i, j, s = i+1, j+1, s + "a"  {
    fmt.Println("Value of i, j, s:", i, j, s)
}
```

## go语言规范

https://golang.org/ref/spec#Variables

变量是用来存放数值的存储位置。允许的值集由变量的类型决定。

变量声明，或者对于函数参数和结果，函数声明或函数字面量的签名为命名的变量保留了存储空间。调用内置函数new或取复合字面的地址，在运行时为变量分配存储空间。这样的匿名变量是通过（可能是隐含的）指针间接引用的。

数组、切片和结构体类型的结构化变量具有可以单独寻址的元素和字段。每个这样的元素都像一个变量一样。

变量的静态类型（或者说只是类型）是在它的声明中给出的类型，新调用或复合文字中提供的类型，或者是结构变量的元素的类型。接口类型的变量也有一个独特的动态类型，它是在运行时分配给变量的值的具体类型（除非该值是预先声明的标识符nil，它没有类型）。动态类型在执行过程中可能会发生变化，但存储在接口变量中的值总是可以分配给变量的静态类型。

```go
var x interface{}  // x 是 nil，它有一个静态类型 interface{}
var v *T           // v 的值为 nil，静态类型为 *T
x = 42             // x 的值为 42，动态类型为 int
x = v              // x 的值为 (*T)(nil)，动态类型为 *T
```

变量的值通过在表达式中引用变量来检索；它是分配给变量的最新值。如果一个变量还没有被赋值，它的值就是它的类型的零值。

## Effective Go

https://golang.org/doc/effective_go.html#variables

变量可以像常量一样被初始化，但初始化器可以是一个在运行时计算的通用表达式。

```go
var (
    home   = os.Getenv("HOME")
    user   = os.Getenv("USER")
    gopath = os.Getenv("GOPATH")
)
```
