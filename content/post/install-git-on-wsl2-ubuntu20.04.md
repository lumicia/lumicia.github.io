---
title: "在 WSL2 Ubuntu20.04 上安装最新版本的 Git"
date: 2021-06-30T09:26:47+08:00
categories: ["Git", "Linux"]
tags: ["git"]
draft: false
---

首先用官方库下载更新，看 Git 版本：

```bash
sudo apt update
sudo apt install git
git --version
```

Git 版本号显示为`git version 2.25.1`，然而这是 Ubuntu20.04 发布时自带的版本。。。

使用官方 PPA 更新 Git 到最新的版本：

```bash
sudo add-apt-repository ppa:git-core/ppa
sudo apt install git
sudo apt upgrade
```

好了，现在 Git 更新到已发布的最新版本了:wink:。

参考：

https://www.osradar.com/install-git-ubuntu-20-04/
