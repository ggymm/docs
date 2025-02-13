---
date: "2025-02-12"
title: "创建 swap 交换分区"
summary: ""
tags: ["linux", "ubuntu", "swap"]
categories: []
---


### 创建交换分区

```sh

# 查看磁盘空间
df -h

# 创建交换分区
sudo fallocate -l 64G /home/webapp/swapfile

# 检查交换分区
ls -lh /home/webapp/swapfile

# 设置交换分区权限
sudo chmod 600 /home/webapp/swapfile
sudo mkswap /home/webapp/swapfile
sudo swapon /home/webapp/swapfile
sudo swapon --show

# 查看交换分区
free -h

# 永久设置交换分区
sudo cp /etc/fstab /etc/fstab.bak
echo '/home/webapp/swapfile none swap sw 0 0' | sudo tee -a /etc/fstab

```