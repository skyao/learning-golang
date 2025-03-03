---
title: "continue 语句"
linkTitle: "continue"
weight: 90
date: 2025-02-27
description: >
  Golang 中的 continue 语句
---

## golang语言规范

https://golang.org/ref/spec#Continue_statements

"continue"语句在其post语句处开始最里面的 "for"循环的下一次迭代。"for"循环必须在同一个函数内。

```
ContinueStmt = "continue" [ Label ] .
```

如果有标签，则标签必须包含 "for" 语句。

```go
RowLoop:
	for y, row := range rows {
		for x, data := range row {
			if data == endOfRow {
				continue RowLoop
			}
			row[x] = data + bias(x, y)
		}
	}
```

