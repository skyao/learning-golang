---
title: "语句概述"
linkTitle: "概述"
weight: 1
date: 2025-02-27
description: >
  Golang 语法中的语句概述
---


https://golang.org/ref/spec#Statements

语句控制执行。

```
Statement =
	Declaration | LabeledStmt | SimpleStmt |
	GoStmt | ReturnStmt | BreakStmt | ContinueStmt | GotoStmt |
	FallthroughStmt | Block | IfStmt | SwitchStmt | SelectStmt | ForStmt |
	DeferStmt .

SimpleStmt = EmptyStmt | ExpressionStmt | SendStmt | IncDecStmt | Assignment | ShortVarDecl .
```





