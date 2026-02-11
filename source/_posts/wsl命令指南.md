---
title: wsl命令指南
date: 2025-12-29 11:05:31
tags:
 - wsl
categories:
 - tutorial
---

#### 0.安装前置设置
1. 需要打开CPU虚拟化
2. 需要打开两个windows功能
	- 适用于windows的linux子系统
	- 虚拟机平台
接着运行`wsl --install --web-download`安装

#### 1.wsl配置相关命令
- `wsl --list --online`:查找可用的linux系统版本
- `wsl --install x --web-download`:下载版本为x的linux系统
- `wsl --list -v`:查找当前电脑安装的linux版本
- `wsl --set-default x`:将x设为默认的操作系统
- `wsl -d x`:cmd命令启动Linux系统
以上是基本操作，更多操作详见[WSL教程](https://www.bilibili.com/video/BV1tW42197za)