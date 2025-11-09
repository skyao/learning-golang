---
title: "linux安装"
linkTitle: "linux"
weight: 30
date: 2025-11-09
description: >
  在 linux 上安装 golang
---

首先删除之前已经安装的版本 ：

```bash
sudo rm -rf /usr/local/go
```

解压缩下载下来的 go1.xx.x.linux-amd64.tar.gz 文件：

```bash
sudo tar -C /usr/local -xzf go1.25.4.linux-amd64.tar.gz
```

设置 GOROOT / GOPATH 然后将 GOROOT/bin 和 GOPATH/bin 加入到 PATH：

```bash
vi ~/.zshrc
```

增加内容:

```bash
# golang
export GOROOT=/usr/local/go
export GOPATH=/home/sky/work/soft/gopath
export PATH=$GOROOT/bin:$GOPATH/bin:$PATH
```

重新装载:

```bash
source ~/.zshrc
```

执行 go version / go env 等检验。

```bash
$ go version
go version go1.25.4 linux/amd64

$ env | grep GO
GOROOT=/usr/local/go
GOPATH=/home/sky/work/soft/gopath
```


