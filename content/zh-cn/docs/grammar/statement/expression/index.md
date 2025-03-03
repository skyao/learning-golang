---
title: "表达式语句"
linkTitle: "表达式"
weight: 20
date: 2025-02-27
description: >
  Golang 的表达式语句
---


### 表达式语句

https://golang.org/ref/spec#Expression_statements

除了特定的内置函数外，函数和方法的调用以及接收操作都可以在语句上下文中出现。这种语句可以用括号。

```
ExpressionStmt = Expression .
```

语句上下文中不允许使用以下内置函数：

```go
append cap complex imag len make new real
unsafe.Alignof unsafe.Offsetof unsafe.Sizeof
```

```go
h(x+y)
f.Close()
<-ch
(<-ch)
len("foo")  // illegal if len is the built-in function
```








