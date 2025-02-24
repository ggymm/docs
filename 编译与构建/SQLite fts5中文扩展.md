---
date: "2025-02-12"
title: "编译 SQLite fts5 中文扩展"
summary: ""
tags: ["sqlite", "fts5", "全文检索"]
categories: []
---

### 项目地址

[simple](https://github.com/wangfenjin/simple)

### windows环境编译

#### 环境准备

1. 安装cmake
2. 安装msys64，执行 ```pacman -S --needed base-devel mingw-w64-x86_64-toolchain``` 安装编译工具链
3. 安装Git

#### 环境变量

1. 添加cmake的bin目录到path环境变量
2. 添加msys64目录下，mingw64的bin目录到path环境变量

#### 编译安装

```cmd
:: 设置代理
set http_proxy=http://127.0.0.1:9910
set https_proxy=http://127.0.0.1:9910

:: 创建编译目录
mkdir build && cd build

:: 执行cmake
cmake .. -G "Unix Makefiles" -DSIMPLE_WITH_JIEBA=OFF -DCMAKE_INSTALL_PREFIX=release

:: 编译安装
make && make install
```

