---
title: "Archlinux+wsl2+latex"
date: 2022-11-14T16:14:37+08:00
categories: 
tags: 
draft: true
---

安装 LaTeX 套件：

```bash
sudo pacman -S texlive-most texlive-langchinese
```

安装相关 VS Code 插件：

- LaTex Workshop

- LaTeX Utilities

- HyperSnipts for math

随便写一个 TeX 文件，编译通过。然后发现缺少相关的包，TeX 文件无法格式化，查看 [LaTeX Workshop 文档]([Format · James-Yu/LaTeX-Workshop Wiki (github.com)](https://github.com/James-Yu/LaTeX-Workshop/wiki/Format) 发现需要 `latexindent`。按照 [latexindent 的文档]([12. Appendices — latexindent.pl 3.19 documentation (latexindentpl.readthedocs.io)](https://latexindentpl.readthedocs.io/en/latest/sec-appendices.html#required-perl-modules) 进行安装：

```bash
sudo pacman -S perl cpanminus perl-file-homedir
```

配置路径和缩进格式：

```bash
"latex-workshop.latexindent.path": "/usr/bin/latexindent",
"latex-workshop.latexindent.args": [
    "-c",
    "%DIR%/",
    "%TMPFILE%",
    "-y=defaultIndent: '%INDENT%'"
]
```

缩进默认是 `\t`。如果想改成两个空格，就把 `args` 最后一行的 `'%INDENT%'`  改为 `'  '` 即可。我个人改成了四个空格。

安装 `latexindent` 后尝试对 TeX 文件格式化，结果报错：

```bash
Can't locate YAML/Tiny.pm in @INC (you may need to install the YAML::Tiny module) (@INC contains:
...
```

根据 https://bugs.archlinux.org/task/60210[FS#60210 : [texlive-core] latexindent is missing perl dependencies](https://bugs.archlinux.org/task/60210) 中的评论，尝试安装

```bash
sudo pacman -S perl-yaml-tiny
```

OK，再就没有问题了。
