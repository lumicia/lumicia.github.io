---
title: "exa 基础"
date: 2021-05-27T22:35:43+08:00
categories: ["Linux"]
tags: ["exa"]
draft: false
---

`exa` 是一个 Rust 编写的命令行程序，是 `ls` 的现代化替代。官网：https://the.exa.website。

与 `ls` 相比，`exa` 能提供更多的信息，并且拥有丰富的颜色，将文件和数据按类型或含义高亮。

<!--more-->

## 选项参数

1、显示选项

| 选项参数                    | 描述                                                         |
| --------------------------- | ------------------------------------------------------------ |
| `-1` / `--oneline`          | 每行显示一个文件 ( 注意看清楚简写是数字 1 ) |
| `-G` / `--grid`             | 以网格来显示文件，是默认行为                                 |
| `-l` / `--long`             | 在表中显示文件和文件的元数据                                 |
| `-x` / `--across`           | 交叉排序网格，而非降序                                       |
| `-R` / `--recurse`          | 递归进入目录                                                 |
| `-T` / `--tree`             | 递归进入作为树的目录                                         |
| `-L` / `--level=DEPTH`      | 限制递归深度为给定数字                                       |
| `-F` / `--classify`         | 通过文件名来显示文件类型指示符                               |
| `--color` / `--colour=WHEN` | 配置何时使用终端颜色，默认值是 `automatic` / `auto`，其他的值有 `always` 和 `never` |
| `--icons`                   | 通过文件名来显示 Unicode 图标                                |
| `--no-icons`                | 不显示图标                                                   |

2、过滤和排序选项

| 选项参数                              | 描述                                                     |
| ------------------------------------- | -------------------------------------------------------- |
| `-a` / `--all`                        | 显示隐藏文件和 `.` 开头的文件，也会显示 `.` 和 `..` 目录 |
| `-d` / `--no-recurse` / `--list-dirs` | 像常规文件那样列出目录，不递归进入目录                   |
| `-r` / `--reverse`                    | 翻转排序顺序                                             |
| `-s` / `--sort=SORT_FIELD`            | 配置要排序的字段                                         |
| `-I` / `-ignore-glob=GLOBS`           | 忽略文件的全局模式，以 `|` 分隔多个文件                  |
| `--git-ignore`                        | 忽略 `.gitignore` 中的文件                               |
| `--group-directories-first`           | 排序时先列出目录再列出文件                               |

3、长视图选项 ( 和 `-l` 或 `--long` 一起用 )

| 选项参数                           | 描述                                                         |
| ---------------------------------- | ------------------------------------------------------------ |
| `-b` / `--binary`                  | 列出二进制前缀的文件大小                                     |
| `-B` / `--bytes`                   | 列出没有前缀的文件大小，单位是字节                           |
| `-g` / `--group`                   | 列出每个文件的群组                                           |
| `-h` / `--header`                  | 给表中的每一列添加一个首部行                                 |
| `-H` / `--links`                   | 列出每个文件的硬链接编号                                     |
| `-i` / `--inode`                   | 列出每个文件的 inode 编号                                    |
| `-m` / `--modified`                | 使用修改的时间戳字段                                         |
| `-S` / `--blocks`                  | 列出每个文件的文件系统块计数                                 |
| `-t` / `--time=WORD`               | 配置要列出的时间戳字段 ( `modified`、`accesssed`、`created` ) |
| `--time-style=STYLE`               | 配置时间戳格式                                               |
| `-u` / `--accessed`                | 使用 `accessed` 时间戳字段                                   |
| `-U` / `--created`                 | 使用 `created` 时间戳字段                                    |
| `-@` / `--extended`                | 列出每个文件的扩展属性和大小                                 |
| `--git`                            | 列出每个已追踪文件的 Git 状态                                |
| `--color-scale` / `--colour-scale` | 以不同颜色高亮文件大小的级别                                 |
| `--no-permissions`                 | 隐藏权限列                                                   |
| `--no-filesize`                    | 隐藏文件大小列                                               |
| `--no-user`                        | 隐藏用户名列                                                 |
| `--no-time`                        | 隐藏时间列                                                   |
