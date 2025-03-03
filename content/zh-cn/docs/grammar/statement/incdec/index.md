---
title: "自增自减语句"
linkTitle: "自增自减"
weight: 40
date: 2025-02-27
description: >
  Golang 的自增自减语句
---


### IncDec/自增自减语句

"++"和"--"语句以无类型常数1来递增或递减操作数。和赋值一样，操作数必须是可寻址的（addressable），或者是一个映射索引表达式。

```
IncDecStmt = Expression ( "++" | "--" ) .
```

下列赋值语句在语义上是等价的：

```go
IncDec statement    Assignment
x++                 x += 1
x--                 x -= 1
```









