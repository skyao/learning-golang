---
title: "设置"
linkTitle: "设置"
weight: 90
date: 2025-02-27
description: >
  golang 安装完成后的设置
---

### 设置 GOPROXY

查看默认的 goproxy 设置：

```bash
$ go env GOPROXY
https://proxy.golang.org,direct
```

设置环境变量 GOPROXY 来设置 go module 公共代理仓库，代理并缓存go模块，以加速构建。

```bash
# golang
......
export GOPROXY="https://goproxy.cn,direct"
```

可用的 goproxy 有：

- 七牛："https://goproxy.cn,direct"
- 阿里云："https://mirrors.aliyun.com/goproxy/,direct"
- 官方（有全球 CDN 加速）："https://goproxy.io,direct"

对于通过 nexus 建立了本地代理仓库的情况，设置为本地代理仓库的地址，如：

```bash
export GOPROXY="http://192.168.0.246:8081/repository/go-proxy-all/,direct"
```

### 设置私有模块

查看默认的 GOPRIVATE 设置，默认为空：

```bash
$ go env GOPRIVATE

```

可以通过设置 GOPRIVATE 环境变量来控制私有仓库、依赖等 (如公司内部仓库) 不通过 goproxy 拉取，而是走本地：

```bash
# 设置不走 goproxy 的私有仓库
# 如果有多个则用逗号分隔
export GOPRIVATE=*.someone.com
```

### 设置 GOSUMDB

查看默认的 GOSUMDB 设置：

```bash
$ go env GOSUMDB
sum.golang.org
```

如果遇到 GOSUMDB sum.golang.org 连接超时，则需要设置 GOSUMDB：

```bash
# 设置不走 goproxy 的私有仓库
# 如果有多个则用逗号分隔
export GOSUMDB=sum.golang.google.cn
```

参考：https://learnku.com/go/wikis/66836