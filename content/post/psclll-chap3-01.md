---
title: "程序员的自我修养 Chap3 01：目标文件"
date: 2021-05-17T22:04:06+08:00
categories: ["Compiler Principle"]
tags: ["程序员的自我修养：装载、链接与库"]
draft: false
---

目标文件从结构上讲，是已经编译后的可执行文件格式，只是还没有经过链接的过程，其中可能有些符号或有些地址还没有调整。Windows 下的扩展名 `.obj`，Linux 下的扩展名 `.o`。

Windows 的可执行文件格式是 PE ( Portable Executable ) ，Linux 的是 ELF ( Executable Linkable Format ) ，它们都是 COFF ( Common file format ) 的变种。

目标文件跟可执行文件的内容与结构很相似，所以一般跟可执行文件格式一起采用一种格式存储。在 Windows 下可以统称为 PE-COFF 文件格式。在 Linux 下，可以统称为 ELF 文件。

<!--more-->

除此以外，动态链接库和静态链接库也按照可执行文件格式存储。在 Windows 下按 PE-COFF 格式存储，Linux 下按 ELF 格式存储。

动态链接库 ( DLL, Dynamic Linking Library ) 在 Windows 下是 `.dll` 文件，Linux 下是 `.so` 文件。

静态链接库 ( Static Linking Library ) 在 Windows 下是 `.lib` 文件，Linux 下是 `.a` 文件。静态链接库稍有不同，把很多目标文件捆绑在一起形成一个文件，再加上一些索引。可以理解为一个包含很多目标文件的文件包。

ELF 文件标准中采用 EFL 格式的文件可分为四类：

| ELF 文件类型                       | 说明                                                         | 实例                                                   |
| ---------------------------------- | ------------------------------------------------------------ | ------------------------------------------------------ |
| 可重定位文件 ( Relocatable File ) | 包含代码和数据，可用来链接成可执行文件或共享目标文件，静态链接库也可归为这一类。 | Linux  的 `.o`，Windows 的 `.obj`                      |
| 可执行文件 ( Executable File ) | 包含可直接运行的程序，代表就是 ELF 可执行文件，它们一般没有扩展名 | 如 `/bin/bash` 文件，Windows 的 `.exe`                 |
| 共享目标文件 ( Shared Object File ) | 包含代码和数据，可以在两个情况下使用：1、链接器可以使用这种文件跟其他的可重定位文件和共享目标文件链接，产生新的目标文件。2、动态链接器可以将几个这种共享目标文件与可执行文件结合，作为进程映像的一部分来运行 | Linux 的 `.so`，如 `/lib/glibc-2.5.so`，Windows 的 DLL |
| 核心转储文件 ( Core Dump File ) | 当进程意外终止时，系统可以将该进程的地址空间的内容及终止时的一些其他信息转储到核心转储文件 | Linux 的 core dump                                     |

可以用 Linux 命令 `file` 查看相应的文件格式。

## 目标文件内容

目标文件中的内容除了编译后的机器指令代码、数据外，还包括链接时所需的一些信息，如符号表、调试信息、字符串等。一般将这些信息按不同的属性，以「节」 ( Section ) 的形式存储，也称为「段」 ( Segment ) ，表示一个一定长度的区域。段与段之间仅在 ELF 的链接视图和装载视图的时候有区别。

程序源代码编译后的机器指令通常放在代码段 ( Code Section ) 中，代码段常见的名字是 `.code` 或 `.text`。全局变量和局部静态变量的数据通常放在数据段 ( Data Section ) 中，名字一般是 `.data`。

<img src="https://i.loli.net/2021/05/18/Y3HPVOfedQvxZ7y.png" alt="程序源码到目标文件" style="zoom:33%;" />

假设上图为一个 ELF 格式的可执行文件。ELF 文件的开头是一个「文件头」，描述了整个文件的文件属性，包含文件是否可执行，是静态链接还是动态链接及入口地址 ( 如果是可执行文件 ) 、目标硬件、目标操作系统等信息。文件头还包含一个段表 ( Section Table ) ，是一个描述文件中各个段的数组。段表描述了文件中各个段在文件中的偏移位置及段的属性等，从段表中可以得到每个段的所有信息。文件头后面是各个段的内容。

C 语言编译后执行语句一般都编译成机器代码，保存在 `.text` 段。已初始化的全局变量和局部静态变量都保存在 `.data` 段。未初始化的全局变量和局部静态变量一般放在 `.bss` 段 ( bss 是 Block Started by Symbol 的缩写 ) 。

虽然未初始化的全局变量和局部静态变量默认值都是 `0`，可以放在 `.data` 段，但因为都是 `0`，为它们在 `.data` 段分配空间并存放数据 `0` 是没有必要的。程序运行的时候它们的确要占据内存空间，并且可执行文件必须记录所有未初始化的全局变量和局部静态变量的大小总和。所以 `.bss` 段只是为未初始化的全局变量和局部静态变量预留位置而已，它没有内容，所以它在文件中也不占据空间。

总体来说，程序源代码在编译之后主要分为两种段：程序指令和程序数据。代码段属于程序指令，数据段和 `.bss` 段属于程序数据。

将数据和指令分段的好处：

-   程序装载后，数据和指令分别映射到两个虚存区域，由于数据区域对于进程来说是可读写的，而指令区域对于进程来说是只读的，所以这两个虚存区域的权限可以分别设置成可读写和只读。这样可以防止程序的指令被有意或无意地改写。
-   现代 CPU 有极为强大的缓存 ( Cache ) 体系，缓存对于现代的计算机非常重要，所以程序必须尽量提供缓存的命中率。指令区和数据区的分离有利于提高程序的局部性。现代 CPU 的缓存一般都设计为数据缓存和指令缓存分离，所以程序的指令和数据分开存放对 CPU 的缓存命中率提高有好处。
-   当系统中运行多个该程序的副本时，它们的指令都是一样的，所以内存中只需要保存一份该程序的指令部分。对于其他只读数据也是这样。系统可以通过共享指令来节省大量空间。

## 目标文件分析

将上图中的 C 代码保存为 `SimpleSeciton.c`，然后用 GCC 来编译。`-c` 参数表示只编译不连接。

```bash
$ gcc -c SimpleSection.c
```

得到一个 `SimpleSection.o` 目标文件。文件都是 64 位的。下面的命令是我的笔记本的输出，系统为 WSL2 Ubuntu 20.04, CPU 为 i5-7200U, gcc 版本 9.3.0。

查看文件的字节大小：

```bash
$ exa -lB
.rw-r--r--  311 18 May 11:17 SimpleSection.c
.rw-r--r-- 2088 18 May 11:18 SimpleSection.o
```

`SimpleSection.o` 文件大小为 2088 字节。

可以用 `binutils` 中的工具 `objdump` 来查看目标文件内部的结构。

```bash
$ objdump -h SimpleSection.o

SimpleSection.o: file format elf64-x86-64

Sections:
Idx Name          Size      VMA               LMA               File off  Algn
  0 .text         00000061  0000000000000000  0000000000000000  00000040  2**0
                  CONTENTS, ALLOC, LOAD, RELOC, READONLY, CODE
  1 .data         00000008  0000000000000000  0000000000000000  000000a4  2**2
                  CONTENTS, ALLOC, LOAD, DATA
  2 .bss          00000004  0000000000000000  0000000000000000  000000ac  2**2
                  ALLOC
  3 .rodata       00000004  0000000000000000  0000000000000000  000000ac  2**0
                  CONTENTS, ALLOC, LOAD, READONLY, DATA
  4 .comment      0000002b  0000000000000000  0000000000000000  000000b0  2**0
                  CONTENTS, READONLY
  5 .note.GNU-stack 00000000  0000000000000000  0000000000000000  000000db  2**0
                  CONTENTS, READONLY
  6 .note.gnu.property 00000020  0000000000000000  0000000000000000  000000e0  2**3
                  CONTENTS, ALLOC, LOAD, READONLY, DATA
  7 .eh_frame     00000058  0000000000000000  0000000000000000  00000100  2**3
                  CONTENTS, ALLOC, LOAD, RELOC, READONLY, DATA
```

`-h` 参数将 ELF 文件的各个段的基本信息打印除了。也可以用 `-x` 参数打印更多信息。

除了基本的代码段、数据段和 BSS 段，还有只读数据段 ( `.rodate` ) 、注释信息段 ( `.comment` ) 、堆栈提示段 ( `.note.GNU-stack` ) 、堆栈属性段 ( `.note.gnu.property` ) 和异常处理段 ( `.eh_frame` ) 。

段属性：长度 ( `Size` ) 、段的位置 ( `File offset` ) 。属性值中的 `CONTENTS` 表示该段在文件中存在。BSS 段没有 `CONTENTS`，表示它实际在 ELF 文件中不存在内容。`.note.GNU-stack` 段虽然有 `CONTENTS`，但它的长度为 0，可以认为它在 ELF 文件中也不存在。

`size` 命令可以查看 ELF 文件的代码段、数据段和 BSS 段的长度。输出内容其的 `dec` 和 `hex` 分别表示 3 个段的长度的和的十进制、十六进制。

```bash
$ size SimpleSection.o
   text    data     bss     dec     hex filename
    221       8       4     233      e9 SimpleSection.o
```

### 代码段

`objdump` 的 `-s` 参数可以将所有段的内容以十六进制的方式打印出来，`-d` 参数可以将所有包含指令的段反汇编。下面只分析代码段的内容，其他段的内容省略。

```bash
$ objdump -s -d SimpleSection.o
......
Contents of section .text:
 0000 f30f1efa 554889e5 4883ec10 897dfc8b  ....UH..H....}..
 0010 45fc89c6 488d3d00 000000b8 00000000  E...H.=.........
 0020 e8000000 0090c9c3 f30f1efa 554889e5  ............UH..
 0030 4883ec10 c745f801 0000008b 15000000  H....E..........
 0040 008b0500 00000001 c28b45f8 01c28b45  ..........E....E
 0050 fc01d089 c7e80000 0000b800 000000c9  ................
 0060 c3
......
0000000000000000 <func1>:
   0: f3 0f 1e fa             endbr64
   4:55                      push   %rbp
   5:48 89 e5                mov    %rsp,%rbp
   8:48 83 ec 10             sub    $0x10,%rsp
   c:   89 7d fc                mov    %edi,-0x4 ( %rbp )
   f:   8b 45 fc                mov    -0x4 ( %rbp ) ,%eax
  12:89 c6                   mov    %eax,%esi
  14:48 8d 3d 00 00 00 00    lea    0x0 ( %rip ) ,%rdi        # 1b <func1+0x1b>
  1b: b8 00 00 00 00          mov    $0x0,%eax
  20: e8 00 00 00 00          callq  25 <func1+0x25>
  25:90                      nop
  26: c9                      leaveq
  27: c3                      retq

0000000000000028 <main>:
  28: f3 0f 1e fa             endbr64
  2c:   55                      push   %rbp
  2d:   48 89 e5                mov    %rsp,%rbp
  30:48 83 ec 10             sub    $0x10,%rsp
  34: c7 45 f8 01 00 00 00    movl   $0x1,-0x8 ( %rbp )
  3b:   8b 15 00 00 00 00       mov    0x0 ( %rip ) ,%edx        # 41 <main+0x19>
  41:8b 05 00 00 00 00       mov    0x0 ( %rip ) ,%eax        # 47 <main+0x1f>
  47:01 c2                   add    %eax,%edx
  49:8b 45 f8                mov    -0x8 ( %rbp ) ,%eax
  4c:   01 c2                   add    %eax,%edx
  4e:   8b 45 fc                mov    -0x4 ( %rbp ) ,%eax
  51:01 d0                   add    %edx,%eax
  53:89 c7                   mov    %eax,%edi
  55: e8 00 00 00 00          callq  5a <main+0x32>
  5a: b8 00 00 00 00          mov    $0x0,%eax
  5f: c9                      leaveq
  60: c3                      retq
```

`Contents of section .text` 后面的内容就是 `.text` 的数据以十六进制打印出来的内容，总共 `0x61` 字节。最左一列是偏移量，中间 4 列是十六进制内容，最右一列是 `.text` 段的 ASCII 码形式。

对照下面的反汇编结果，可以看到 `.text` 段中所包含的正是 `SimpleSection.c` 中两个函数 `func1 ( ) ` 和 `main ( ) ` 的指令。`.text` 段的第二个字节 `0x55` 是 `func1 ( ) ` 函数的第一条 `push %rbp` 指令，而最后一个字节 `0xc3` 是 `main ( )` 函数的最后一条指令 `retq`。

> : large_orange_diamond: DIFF
>
> 书中 `0x55` 是第一个字节，指令为 `push %ebp`。`0xc3` 的指令是 `ret`。

### 数据段和只读数据段

`.data` 段保存已经初始化了的全局静态变量和局部静态变量。在 `SimpleSection.c` 中分别是 `global_init_var` 和 `static_var` 变量。这两个变量每个 4 字节，一共正好 8 字节，所以 `.data` 段大小为 8 字节。

调用 `printf` 函数时，使用了字符串常量 `%d\n`，它是一种只读数据，所以放到 `.rodata` 段。`.rodata` 段的 4 个字节正好是这个字符串常量的 ASCII 字节序，最后以 `\0` 结尾。

`.rodata` 段存放只读数据，一般是程序中的只读变量 ( 如 `const` 修饰的变量 ) 和字符串常量。单独设立一个 `.rodata` 段不仅在语义上支持了 C++ 的 `const` 关键字，而且操作系统在装载的时候可以将 `.rodata` 段的属性映射成只读，这样对于这个段的任何修改都会作为非法操作处理，保证了程序的安全性。另外在某些嵌入式平台下，有些存储区域是采用只读存储器的，如 ROM，这样将 `.rodata` 段放在该存储区域中就可以保证程序访问存储器的正确性。

另外，有时编译器会将字符串常量放到 `.data` 段，而不会单独放在 `.rodata` 段。

```bash
$ objdump -x -s -d SimpleSection.o
......
Sections:
Idx Name          Size      VMA               LMA               File off  Algn
  ......
  1 .data         00000008  0000000000000000  0000000000000000  000000a4  2**2
                  CONTENTS, ALLOC, LOAD, DATA
  ......
  3 .rodata       00000004  0000000000000000  0000000000000000  000000ac  2**0
                  CONTENTS, ALLOC, LOAD, READONLY, DATA
......
Contents of section .data:
 0000 54000000 55000000                    T...U...
Contents of section .rodata:
 0000 25640a00%d..
......
```

`.data` 段前 4 个字节，从低到高分别为 `0x54`、`0x00`、`0x00`、`0x00`，刚好是 `global_init_var` 的值，即十进制的 `84`。后 4 个字节是 `static_init_var` 的值，即 `85`。`global_init_var` 是个 4 字节长度的 `int` 类型，由于 Intel CPU 的字节序 ( Byte Order ) 是小端 ( Little-endian ) ，在低地址存放最低有效字节，因此存放的次序从 `0x54` 开始，而不是从 `0x00` 开始。

### BSS 段

`.bss` 段存放未初始化的全局静态变量和局部静态变量。`global_uninit_var` 和 `static_var2` 存放在 `.bss` 段，更准确地说是 `.bss` 段为它们预留了空间。但该段只有 4 字节，与 `global_uninit_var` 和 `static_var2` 的总共 8 字节大小不符。

可以通过符号表 ( Symbol Table ) 看到，`.bss` 段只存放了 `static_var2`，而 `global_uninit_var` 未存放到任何段中，只是一个未定义的 `COMMON` 符号。这与不同的语言和不同的编译器实现有关。有些编译器会将全局的未初始化变量存放在目标文件的 `.bss` 段；有些则不存放，只预留一个未定义的全局变量符号，等到最终链接成可执行文件时再在 `.bss` 段分配空间。值得一提的是，编译单元内部可见的静态变量 ( 如给 `global_uninit_var` 加上 `static` 修饰 ) 的确是存放在 `.bss` 段的。

```bash
$ objdump -x -s -d SimpleSection.o
......
Sections:
Idx Name          Size      VMA               LMA               File off  Algn
  ......
  2 .bss          00000004  0000000000000000  0000000000000000  000000ac  2**2
                  ALLOC
  ......
```

如果有如下代码：

```c
static int x1 = 0;
static int x2 = 1;
```

变量 `x1` 会存放到 `.bss` 段，变量 `x2` 存放到 `.data` 段。因为 `x1` 为 `0`，可以认为是未初始化的，因为未初始化的都是 `0`，所以优化后放在 `.bss` 段来节省磁盘空间。而 `x2` 初始化值为 `1`，故存放到 `.data` 段中。

### 其他段

`.comment` 段存放编译器版本信息。

---

如果要将二进制文件，如图片、MP3 文件、词典文件等作为目标文件中的一个段，可以使用 `objcopy` 工具。

在全局变量或函数之前加上 `__attribute__ (( section ( "name" )) )` 可以把相应的变量或函数放在 `name` 自定义段中。
