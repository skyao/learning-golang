---
title: "windows安装"
linkTitle: "windows"
weight: 20
date: 2025-02-27
description: >
  在 windows 上安装 golang
---

### 卸载旧版本

如果发现安装有旧版本，会要求先卸载旧版本再继续安装。

> 备注：不知道为什么，删除旧版本文件的过程非常的慢。

更新：遇到安装完成后，执行 go install 命令时报错：

```bash
\xxx\map.go xxxxx mapiterinit redeclared in this block
```

后来发现先在 windows 的 app 列表中卸载 golang 后，重新安装 golang，问题解决。

因此，强烈建议先手工卸载旧版本，再安装新版本。

### 安装

运行下载下来的 `go1.24.0.windows-amd64.msi` 安装文件。

安装过程中需要设置安装路径，我一般不喜欢用默认的 `Program Files` 路径，通常设置为：`D:\sky\work\soft\golang\`

安装完成后，打开 cmd 验证一下：

```bash
$ go version

go version go1.24.0 windows/amd64
```

打开 gitbash，检查：

```bash
$ go version
go version go1.24.0 windows/amd64

$ which go
/d/sky/work/soft/golang/bin/go
```

设置环境变量：

- GOPATH=`D:\sky\work\soft\gopath\`

- GOROOT=`D:\sky\work\soft\golang\`

修改环境变量（用户变量）Path，增加内容 `%GOPATH%\bin` 和 `%GOROOT%\bin` 。

验证环境变量是否生效：

```bash
$ env | grep -E 'GOPATH|GOROOT'
GOROOT=D:\sky\work\soft\golang\
GOPATH=D:\sky\work\soft\gopath\

$ go env GOPATH
D:\sky\work\soft\gopath\

$ go env GOROOT
D:\sky\work\soft\golang\

$ echo $PATH
```


