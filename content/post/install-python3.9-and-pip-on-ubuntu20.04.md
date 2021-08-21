---
title: "在 Ubuntu20.04 上安装 Python3.9"
date: 2021-08-17T09:34:04+08:00
categories: ["Linux", "Python"]
tags: ["Ubuntu"]
draft: false
---

由于Ubuntu20.04只自带Python3.8，为了使用Python的新特性，需要安装Python3.9：

```bash
sudo apt update
sudo apt install software-properties-common
sudo add-apt-repository ppa:deadsnakes/ppa
sudo apt install python3.9
```

检查安装的Python3.9：

```bash
python3.9 -V
```

接着用`pip`安装其他包。由于自带的Python3.8占用了`pip`和`pip3`，删除`/usr/bin/pip`文件，然后将`pip3.9`软链接到`pip`：

```bash
sudo ln -s ~/.local/bin/pip3.9 /usr/bin/pip
```

查看`pip`：

```bash
pip -V
```

应该输出的是Python3.9中的`pip`包。

`pip`换清华源：

```bash
pip config set global.index-url https://pypi.tuna.tsinghua.edu.cn/simple
```

或新建`~/.pip/pip.config`文件，

```conf
[global]
index-url = https://pypi.tuna.tsinghua.edu.cn/simple

[install]
trusted-host = pypi.tuna.tsinghua.edu.cn
```

