---
title: "在 WLS2 Ubuntu20.04 上安装 GCC11"
date: 2021-06-30T13:25:07+08:00
categories: ["Linux"]
tags: ["GCC"]
draft: true
---

clone源码：

```bash
git clone --branch releases/gcc-11.1.0 https://github.com/gcc-mirror/gcc.git --depth 1
```

切换到GCC源码根目录，然后安装依赖库：

```bash
./contrib/download_prerequisites
```

如果出现错误，则换一种方式：

```bash
sudo apt install libmpc-dev flex
```

配置指定安装目录：

```bash
./configure --prefix=/usr/local/gcc-11.1.0 --enable-bootstrap --enable-languages=c,c++ --enable-threads=posix --enable-checking=release --enable-multilib --with-system-zlib
```

然后用`make`命令编译，可以用`-j<num>`选项来并发编译，`<num>`是指定的数字：

```bash
make -j4
```

安装（也可以在root权限下安装，避免权限不足的问题）：

```bash
sudo make install
```

安装完成后查看GCC版本：

```bash
/usr/local/gcc-11.1.0/bin/gcc --version
```

切换GCC版本，最后面的数字表示auto模式选项的默认优先级：

```bash
sudo update-alternatives --install /usr/bin/gcc gcc /usr/bin/x86_64-linux-gnu-gcc-9 90
sudo update-alternatives --install /usr/bin/gcc gcc /usr/local/gcc-11.1.0/bin/gcc 100
```

确认GCC版本选项：

```bash
sudo update-alternatives --config gcc
```

现在用`gcc --version`查看GCC版本应该是11.1.0了。

解决`LD_LIBRARY_PATH`的问题，添加下列语句到`./bashrc`或`./zshrc`：

```bash
export LD_LIBRARY_PATH=$LD_LIBRARY_PATH:/usr/local/gcc-11.1.0/lib64
```

---

VS Code 可能检测到`#include`错误，需要更新头文件位置。`Ctrl+Shift+P`打开控制台，输入`json`，找到 `c_cpp_properties.json`文件，在`"includePath"`中加入：

```json
"/usr/local/gcc-11.1.0/include/c++/11.1.0",
"/usr/local/gcc-11.1.0/include/c++/11.1.0/backward",
"/usr/local/gcc-11.1.0/lib/gcc/x86_64-pc-linux-gnu/11.1.0/include",
"/usr/local/include",
"/usr/local/gcc-11.1.0/include/c++/11.1.0/x86_64-pc-linux-gnu",
"/usr/local/gcc-11.1.0/include",
"/usr/include"
```

---

用旧版本的GDB调试可能报错，因为GCC11.1开始要求支持C++11的编译器来编译，所以需要安装新版本的GDB：

```bash
wget https://ftp.gnu.org/gnu/gdb/gdb-10.2.tar.gz
tar -zxf gdb-10.2.tar.gz
cd gdb-10.2
./configure
make -j4
sudo make install
```

可能报错：

```bash
WARNING: 'makeinfo' is missing on your system.
         You should only need it if you modified a '.texi' file, or
         any other file indirectly affecting the aspect of the manual.
         You might want to install the Texinfo package:
         ......
```

提示安装`Texinfo`包，安装：

```bash
sudo apt install texinfo
```



默认安装在`/usr/local/bin`，不会覆盖原来的GDB（位于`/usr/bin`）。因此使用VSCode或单独运行时，需要手动指定GDB的版本。

检查GDB版本：

```bash
/usr/local/bin/gdb -v
```

更换GDB后可以正常调试GCC11编译的程序了。



参考：

1. https://zhuanlan.zhihu.com/p/373437562
2. https://cloud.tencent.com/developer/article/1635218
