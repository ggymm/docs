---
date: "2025-02-12"
title: "编译打包 Chromium"
summary: ""
tags: ["chrome", "编译"]
categories: []
---

### 参考文档

[在Windows检查和构建文档](https://chromium.googlesource.com/chromium/src/+/main/docs/windows_build_instructions.md)

### 配置Git代理

编辑C:\Users\19679\.gitconfig文件

添加如下内容
```gitconfig
[http "https://github.com"]
    proxy = socks5://127.0.0.1:9909
		
[http "https://chromium.googlesource.com"]
    proxy = socks5://127.0.0.1:9909
```

### Visual Studio环境准备

下载安装Visual Studio 2019，按照如图所示安装

![image](https://user-images.githubusercontent.com/20694755/149737181-0c5de1ff-c1ab-4973-9d95-b034755aa5c2.png)

需要注意的是Windows 10 SDK（10.0.19041）需要安装SDK Debugging Tools

可以选择重新下载安装[Windows 10 SDK](https://developer.microsoft.com/en-us/windows/downloads/sdk-archive/)，勾选如图所示

![image](https://user-images.githubusercontent.com/20694755/149737667-e8a5f9d3-33e3-4469-bd88-8fde71057fb7.png)

安装完成后查看是否有C:\Program Files (x86)\Windows Kits\10\Debuggers目录存在

### 安装depot_tools

下载[depot_tools](https://storage.googleapis.com/chrome-infra/depot_tools.zip)

解压到D:\Code\chromium\depot_tools目录下

### 配置环境变量

添加：D:\Code\chromium\depot_tools到系统环境变量Path中，并使其位置最前

添加环境变量：DEPOT_TOOLS_WIN_TOOLCHAIN=0，使用本地已经安装的Visual Studio

添加环境变量：vs2019_install=C:\Program Files (x86)\Microsoft Visual Studio\2019\Community，最好不要修改默认地址

### 初始化depot_tools

执行命令，安装更新python，cipd等工具

```bash
gclient
```

### 下载代码

设置代理

```bash
set http_proxy=http://127.0.0.1:9910
set https_proxy=http://127.0.0.1:9910
```

创建目录并下载chromium代码

```bash
mkdir chromium && cd chromium
fetch --no-history chromium
```

此过程使用git下载代码库中的代码，执行时间比较漫长，需要耐心等待

### 构建代码

下载代码之后会在chromium目录下创建src目录，进入到src目录执行下面操作

生成编译配置

```bash
gn gen out/Default
```

编译代码

```bash
autoninja -C out\Default chrome
```

### 生成Visual Studio工程

在chromium\src目录下执行命令

```bash
gn gen --ide=vs out\Default
```

生成all.sln，包含所有项目和依赖库

可以使用如下命令生成部分依赖的工程

```bash
gn gen --ide=vs --filters=//chrome --no-deps out\Default
gn gen --ide=vs --filters=//chrome;//third_party/WebKit/*;//gpu/* --no-deps out\Default
```

### 打包配置

找到chromium\src\out\Release目录下的args.gn

[配置项参考网址](https://www.chromium.org/developers/gn-build-configuration/)

### 生成编译配置

在chromium\src目录下执行命令

```bash
gn gen out/Release 
```

### 修改配置如下

```gn
# Build arguments go here.
# See "gn args <out_dir> --list" for available build arguments.
target_cpu = "x86"
is_debug = false
symbol_level = 0
enable_nacl = false
remove_webcore_debug_symbols = true
```

### 打包

在chromium\src目录下执行命令

```bash
autoninja -C out\Release chrome
```

打最小安装包

```bash
autoninja -C out\Release mini_installer
```

### 更新代码

在chromium目录下执行命令

```bash
gclient sync --force
```