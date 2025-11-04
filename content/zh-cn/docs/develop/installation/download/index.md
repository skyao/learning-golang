---
title: "下载golang"
linkTitle: "下载"
weight: 10
date: 2025-02-27
description: >
  下载 golang 的安装文件
---

## 下载

下载地址： 

https://go.dev/dl/

windows 选择 msi 安装文件:

https://go.dev/dl/go1.25.3.windows-amd64.msi

linux 下选择 tar.gz 压缩文件:

```bash
wget https://go.dev/dl/go1.25.3.linux-amd64.tar.gz
```

版本一般选最新。

## 准备gopath

gopath 是 golang 的 workspace，用于存放项目代码和依赖。

```bash
mkdir -p $HOME/work/soft/gopath/
```

