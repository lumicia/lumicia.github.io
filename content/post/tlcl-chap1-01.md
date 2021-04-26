---
title: "Linux 命令行 Chap1 shell"
date: 2021-04-24T20:16:50+08:00
categories: ["Linux"]
tags: ["command line"]
draft: false
---

命令行在 shell 中编写。shell 是一个将键盘命令传递给操作系统的程序。`bash` 是 Linux 中自带的 shell 程序，是 `sh` 的增强版本。

使用终端模拟器 GUI 程序来使用 shell。

<!--more-->

终端模拟器一行开头机器名后的 `$` 符号称为 shell 提示符，用于提示输入。如果出现的是 `#` 符号则表示终端会话具有超级用户权限。

如果输入无意义的命令，`bash` 将提示 `command not found`。

按 `up` 键可以快速输入上次输入的命令，按 `down` 键取消。按 `up` 键 n 次前进到第 n 次前输入的命令，按 `down` 键后退。

按 `left` 和 `right` 键移动光标。

关闭终端会话的方法：

- 关闭窗口；
- 输入 `exit` 命令退出；
- 按 `Ctrl-D`。