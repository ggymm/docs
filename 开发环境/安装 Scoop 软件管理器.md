---
date: "2025-02-18"
title: "安装 Scoop 软件管理器"
summary: ""
tags: ["scoop", "windows", "proxy"]
categories: []
---

### 文档

[Scoop 官网](https://scoop.sh/)

[Scoop 安装文档](https://github.com/ScoopInstaller/Install)

[Scoop 配置文档](https://github.com/ScoopInstaller/Scoop/wiki)

### 安装 Scoop

```bash
# 打开 powershell
Set-ExecutionPolicy -ExecutionPolicy RemoteSigned -Scope CurrentUser

irm get.scoop.sh | iex

# 如果无法下载，可以添加代理配置
irm get.scoop.sh -Proxy 'http://127.0.0.1:9910' | iex
```

### 配置 Scoop 代理

```bash
scoop config proxy '127.0.0.1:9910'
```

### 常用命令

```bash
# 更新
scoop update

# 检查软件是否有更新
scoop status

# 安装软件
scoop install git

# 卸载软件
scoop uninstall git

# 添加 bucket
scoop bucket add extras

```