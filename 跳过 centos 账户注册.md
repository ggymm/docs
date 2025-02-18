---
date: "2025-02-12"
title: "跳过 centos 账户注册"
summary: ""
tags: ["linux", "centos"]
categories: []
---

### 跳过注册账户

首先使用 ctrl+alt+f2 进入命令行

使用 root 账户登录

输入以下命令

```sh
yum remove -y gnome-initial-setup

init 3

init 5
```