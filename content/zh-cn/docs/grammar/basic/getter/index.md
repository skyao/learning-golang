---
title: "Getter"
linkTitle: "Getter"
weight: 60
date: 2025-02-27
description: >
  Golang Getter 函数
---


> 备注：摘录自 Effective Go https://golang.org/doc/effective_go.html#Getters

### Getters

Go并没有提供对getter和setter的自动支持。自己提供getter和setter并没有错，而且这样做通常是合适的，但在getter的名称中加上Get既不习惯也没有必要。如果你有一个叫做 owner 的字段（小写，未导出），那么 getter 方法应该叫做 Owner（大写，导出），而不是 GetOwner。导出时使用大写的名称提供了区分字段和方法的钩子。如果需要的话，setter函数可能会被称为SetOwner。这两个名字在实践中都很好读。

```go
owner := obj.Owner()
if owner != user {
    obj.SetOwner(user)
}
```
