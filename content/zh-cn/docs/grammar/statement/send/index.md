---
title: "Send语句"
linkTitle: "Send"
weight: 30
date: 2025-02-27
description: >
  Golang 的 Send 语句
---


### Send语句

https://golang.org/ref/spec#Send_statements

发送语句在通道上发送一个值。通道表达式必须是通道类型，通道方向必须允许发送操作，要发送的值的类型必须可以分配给通道的元素类型。

```
SendStmt = Channel "<-" Expression .
Channel  = Expression .
```

在通信开始之前，通道和值表达式都会被评估。通信会被阻塞，直到发送可以继续进行。如果接收者准备好了，在无缓冲通道上的发送就可以进行。在缓冲通道上的发送，如果缓冲区有空间，就可以进行。在已关闭通道上的发送会引起运行时恐慌。在nil通道上的发送会永远阻塞。

```go
ch <- 3  // send value 3 to channel ch
```










