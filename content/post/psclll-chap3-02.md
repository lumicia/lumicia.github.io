---
title: "Psclll Chap3 02: ELF 文件结构描述"
date: 2021-05-19T09:20:31+08:00
categories: ["Compiler Principle"]
tags: ["程序员的自我修养：装载、链接与库"]
draft: false
---

ELF 目标文件格式开头是 ELF 文件头 ( ELF Header ) ，包含了描述整个文件的基本属性，如 ELF 文件版本、目标机器型号、程序入口地址等。

接着是 ELF 文件的各个段。其中与段有关的重要结构是段首部表 ( Section Header Table ) ，描述了 ELF 文件包含的所有段的信息，如段名、段长度、在文件中的偏移量、读写权限及其他段的属性。

最后是 ELF 中的辅助结构，如字符串表、符号表等。

<!--more-->

### 文件头

`readelf` 命令详细查看 ELF 文件。

```bash
$ readelf -h SimpleSection.o
ELF Header:
  Magic:   7f 45 4c 46 02 01 01 00 00 00 00 00 00 00 00 00
  Class: ELF64
  Data:                              2's complement, little endian
  Version:                           1 ( current )
  OS/ABI: UNIX - System V
  ABI Version:                       0
  Type: REL ( Relocatable file )
  Machine: Advanced Micro Devices X86-64
  Version:                           0x1
  Entry point address:               0x0
  Start of program headers:          0 ( bytes into file )
  Start of section headers:          1192 ( bytes into file )
  Flags:                             0x0
  Size of this header:               64 ( bytes )
  Size of program headers:           0 ( bytes )
  Number of program headers:         0
  Size of section headers:           64 ( bytes )
  Number of section headers:         14
  Section header string table index: 13
```

定义了 ELF 魔数、文件机器字节长度、数据存储方式、版本、运行平台、ABI 版本、ELF 重定位类型、硬件平台、硬件平台版本、入口地址、程序头入口和长度、段表的位置和长度以及段的数量等。

ELF 文件头结构与相关常数定义在 `/usr/include/elf.h`。ELF 文件分为 32 位和 64 位，对应的文件头为 `Elf32_Ehdr` 和 `Elf64_Ehdr`。`elf.h` 定义了一系列变量。

| 自定义类型    | 描述                        | 原始类型   | 长度 ( 字节 ) |
| ------------- | --------------------------- | ---------- | ------------ |
| Elf32_Addr    | 32 位版本程序地址           | `uint32_t` | 4            |
| Elf32_Half    | 32 位版本无符号短整型       | `uint16_t` | 2            |
| Elf32_Off     | 32 位版本文件偏移           | `uint32_t` | 4            |
| Elf32_Sword   | 32 位版本的 32 位有符号整型 | `int32_t`  | 4            |
| Elf32_Word    | 32 位版本的 32 位无符号整型 | `int32_t`  | 4            |
| Elf32_Sxword  | 32 位版本的 64 位有符号整型 | `int64_t`  | 8            |
| Elf32_Xword   | 32 位版本的 64 位无符号整型 | `uint64_t` | 8            |
| Elf32_Section | 32 位版本段索引             | `uint16_t` | 2            |
| Elf64_Addr    | 64 位版本程序地址           | `uint64_t` | 8            |
| Elf64_Half    | 64 位版本无符号短整型       | `uint16_t` | 2            |
| Elf64_Off     | 64 位版本文件偏移           | `uint64_t` | 8            |
| Elf64_Sword   | 64 位版本的 32 位有符号整型 | `uint32_t` | 4            |
| Elf64_Word    | 64 位版本的 32 位无符号整型 | `int32_t`  | 4            |
| Elf64_Sxword  | 64 位版本的 64 位有符号整型 | `int64_t`  | 8            |
| Elf64_Xword   | 64 位版本的 64 位无符号整型 | `uint64_t` | 8            |
| Elf64_Section | 64 位版本段索引             | `uint16_t` | 2            |

`Elf64_Ehdr` 的定义：

```c
# define EI_NIDENT ( 16 )

typedef struct {
    unsigned char   e_ident [EI_NIDENT]; /* Magic number and other info */
    Elf64_Half      e_type;             /* Object file type */
    Elf64_Half      e_machine;          /* Architecture */
    Elf64_Word      e_version;          /* Object file version */
    Elf64_Addr      e_entry;            /* Entry point virtual address */
    Elf64_Off       e_phoff;            /* Program header table file offset */
    Elf64_Off       e_shoff;            /* Section header table file offset */
    Elf64_Word      e_flags;            /* Processor-specific flags */
    Elf64_Half      e_ehsize;           /* ELF header size in bytes */
    Elf64_Half      e_phentsize;        /* Program header table entry size */
    Elf64_Half      e_phnum;            /* Program header table entry count */
    Elf64_Half      e_shentsize;        /* Section header table entry size */
    Elf64_Half      e_shnum;            /* Section header table entry count */
    Elf64_Half      e_shstrndx;         /* Section header string table index */
} Elf64_Ehdr;
```

除了 `e_ident` 对应 `Class`、`Data`、`Version`、`OS/ABI`、`ABI Version` 5 个参数外，剩下的参数和 `Elf64_Ehdr` 中的成员一一对应。

32 位 ELF 文件头大小 52 字节，64 位 ELF 文件头 64 字节。

1 ) `e_ident` 大小 16 字节，前 4 个字节称为 ELF 的魔数，固定为 `7f 45 4c 46`。第一个字节对应 `DEL` 控制符，后 3 个字节是 E、L、F 字母的 ASCII 码。几乎所有可执行文件的最开始几个字节都是魔数，如 `a.out` 格式最开始两个字节为 `01 07`。这种魔数用于确认文件类型，操作在加载可执行文件时会确认魔数是否正确，如果不正确会拒绝加载。

第 5 个字节标识 ELF 文件类型，`01` 表示 32 位 ，`02` 表示 64 位。

第 6 个字节是字节序，`01` 表示小端，`02` 表示大端。

第 7 个字节是 ELF 文件头版本，一般是 `01`。

第 8 到 16 字节没有意义，都填为 0。

2 ) `e_type` 大小 2 字节，描述 ELF 文件类型，可取以下值：

```c
# define ET_NONE        0        /* No file type */
# define ET_REL        1        /* Relocatable file */
# define ET_EXEC        2        /* Executable file */
# define ET_DYN        3        /* Shared object file */
# define ET_CORE        4        /* Core file */
# define    ET_NUM        5        /* Number of defined types */
# define ET_LOOS        0xfe00        /* OS-specific range start */
# define ET_HIOS        0xfeff        /* OS-specific range end */
# define ET_LOPROC    0xff00        /* Processor-specific range start */
# define ET_HIPROC    0xffff        /* Processor-specific range end */
```

3 ) `e_machine` 大小 2 字节，描述文件所在平台的架构。以 `EM_` 开头的变量都是可取的值，将近 200 行，只列出部分：

```c
# define EM_NONE         0    /* No machine */
# define EM_M32         1    /* AT&T WE 32100 */
# define EM_SPARC     2    /* SUN SPARC */
# define EM_386         3    /* Intel 80386 */
# define EM_68K         4    /* Motorola m68k family */
# define EM_88K         5    /* Motorola m88k family */
# define EM_IAMCU     6    /* Intel MCU */
# define EM_860         7    /* Intel 80860 */
# define EM_MIPS         8    /* MIPS R3000 big-endian */
# define EM_S370         9    /* IBM System/370 */
# define EM_MIPS_RS3_LE    10    /* MIPS R3000 little-endian */
......
```

4 ) `e_version` 大小 2 字节，描述 ELF 文件版本号，可取的值：

```c
# define EV_NONE        0        /* Invalid ELF version */
# define EV_CURRENT    1        /* Current version */
# define EV_NUM        2
```

5 ) `e_entry` 在 32 位中 4 字节，64 位中 8 字节，描述执行入口点，如果文件没有入口点，这个域保持 0。

6 ) `e_phoff` 在 32 位中 4 字节，64 位中 8 字节，表示 program header table 的偏移，如果文件没有 program header，这个值为 0。

7 ) `e_shoff` 在 32 位中 4 字节，64 位中 8 字节，表示 section header table 的偏移，如果文件没有 section header，这个值为 0。

8 ) `e_flags` 大小 4 字节，特定 CPU 的标志。Intel 架构中没有定义，因此值为 0。

9 ) `e_ehsize` 大小 2 字节，描述 ELF 文件头大小，32 位 ELF 是 52 字节，64 位 ELF 是 64 字节。

10 ) `e_phentsize` 大小 2 字节，表示 program header table 中入口大小。

11 ) `e_phnum` 大小 2 字节，表示 program header table 中入口的数量。与 `e_phentsize` 的积是 program header table 的大小。

12 ) `e_shentsize` 大小 2 字节，表示 section header table 中入口大小。

13 ) `e_shnum` 大小 2 字节，表示 section header table 中入口数量。与 `e_shentsize` 的积是 section header table 的大小。

14 ) `e_shstrndx` 大小 2 字节，表示 section header table 中字符串表索引。
