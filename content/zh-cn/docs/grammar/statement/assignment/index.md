---
title: "赋值语句"
linkTitle: "赋值"
weight: 50
date: 2025-02-27
description: >
  Golang 的赋值语句
---

### Assignment/赋值

https://golang.org/ref/spec#Assignments

```
Assignment = ExpressionList assign_op ExpressionList .

assign_op = [ add_op | mul_op ] "=" .
```

每个左操作数必须是可寻址的（addressable），是一个映射索引表达式，或者是（仅对于=赋值）空白标识符。操作数可以加括号。

```go
x = 1
*p = f()
a[i] = 23
(k) = <-ch  // same as: k = <-ch
```

赋值操作 `x op= y`，其中op是一个二进制算术运算符，相当于 `x = x op (y)`，但只对x进行一次评估。`op=` 结构是一个单一的Token。在赋值操作中，左手和右手的表达式列表必须正好包含一个单值表达式，而且左手表达式不能是空白标识符。

```go
a[i] <<= 2
i &^= 1<<n
```

元组（tuple）赋值将多值操作的各个元素赋值到变量列表中。有两种形式。在第一种形式中，右手的操作数是一个单一的多值表达式，如函数调用、通道或map操作、或类型断言。左手操作数的数量必须与值的数量相匹配。例如，如果f是一个返回两个值的函数。

```go
x, y = f()
```

将第一个值赋给x，第二个值赋给y，在第二种形式中，左边的操作数必须等于右边的表达式数量，每个表达式必须是单值，右边的第n个表达式被赋给左边的第n个操作数。

```
one, two, three = '一', '二', '三'
```

空白标识符提供了一种在赋值中忽略右侧值的方法。

```go
_ = x       // evaluate x but ignore it
x, _ = f()  // evaluate f() but ignore second result value
```

赋值分两个阶段进行。首先，左边的索引表达式的操作数和指针直指（包括选择器中的隐式指针直指）以及右边的表达式都按照通常的顺序进行评估。第二，按照从左到右的顺序进行赋值。

```go
a, b = b, a  // exchange a and b

x := []int{1, 2, 3}
i := 0
i, x[i] = 1, 2  // set i = 1, x[0] = 2

i = 0
x[i], i = 2, 1  // set x[0] = 2, i = 1

x[0], x[0] = 1, 2  // set x[0] = 1, then x[0] = 2 (so x[0] == 2 at end)

x[1], x[3] = 4, 5  // set x[1] = 4, then panic setting x[3] = 5.

type Point struct { x, y int }
var p *Point
x[2], p.x = 6, 7  // set x[2] = 6, then panic setting p.x = 7

i = 2
x = []int{3, 5, 7}
for i, x[i] = range x {  // set i, x[2] = 0, x[0]
	break
}
// after this loop, i == 0 and x == []int{3, 5, 3}
```

在赋值中，每个值都必须可以赋给被赋值的操作数的类型，但有以下特殊情况：

1. 任何类型的值都可以分配给空白标识符。
2. 如果一个非类型的常量被赋值给接口类型的变量或空白标识符，那么该常量首先被隐式转换为其默认类型。
3. 如果一个非类型的布尔值被分配给接口类型的变量或空白标识符，它首先被隐式转换为布尔类型。










