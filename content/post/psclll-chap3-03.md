---
title: "Psclll Chap3 03：Section Header Table"
date: 2021-05-22T09:22:45+08:00
categories: ["Compiler Principle"]
tags: ["程序员的自我修养：装载、链接与库"]
draft: true
---

段首部表（Section Header Table）是 ELF 文件中除文件头以外最重要的架构，描述了段名、段的长度、在文件中的偏移、读写权限及段的其他属性。编译器、链接器和装载器都依赖于段首部表来定位和访问各个段的属性。段首部表在 ELF 文件中的位置由 ELF 文件头的 `e_shoff` 成员决定。

<!--more-->

用 `readelf` 查看 ELF 文件中的段：

```bash
$ readelf -S SimpleSection.o
There are 14 section headers, starting at offset 0x4a8:

Section Headers:
  [Nr] Name              Type             Address           Offset
       Size              EntSize          Flags  Link  Info  Align
  [0]                   NULL             0000000000000000  00000000
       0000000000000000  0000000000000000           0     0     0
  [1] .text             PROGBITS         0000000000000000  00000040
       0000000000000061  0000000000000000  AX       0     0     1
  [2] .rela.text        RELA             0000000000000000  00000388
       0000000000000078  0000000000000018   I      11     1     8
  [3] .data             PROGBITS         0000000000000000  000000a4
       0000000000000008  0000000000000000  WA       0     0     4
  [4] .bss              NOBITS           0000000000000000  000000ac
       0000000000000004  0000000000000000  WA       0     0     4
  [5] .rodata           PROGBITS         0000000000000000  000000ac
       0000000000000004  0000000000000000   A       0     0     1
  [6] .comment          PROGBITS         0000000000000000  000000b0
       000000000000002b  0000000000000001  MS       0     0     1
  [7] .note.GNU-stack   PROGBITS         0000000000000000  000000db
       0000000000000000  0000000000000000           0     0     1
  [8] .note.gnu.propert NOTE             0000000000000000  000000e0
       0000000000000020  0000000000000000   A       0     0     8
  [9] .eh_frame         PROGBITS         0000000000000000  00000100
       0000000000000058  0000000000000000   A       0     0     8
  [10] .rela.eh_frame    RELA             0000000000000000  00000400
       0000000000000030  0000000000000018   I      11     9     8
  [11] .symtab           SYMTAB           0000000000000000  00000158
       00000000000001b0  0000000000000018          12    12     8
  [12] .strtab           STRTAB           0000000000000000  00000308
       000000000000007c  0000000000000000           0     0     1
  [13] .shstrtab         STRTAB           0000000000000000  00000430
       0000000000000074  0000000000000000           0     0     1
Key to Flags:
  W (write), A (alloc), X (execute), M (merge), S (strings), I (info),
  L (link order), O (extra OS processing required), G (group), T (TLS),
  C (compressed), x (unknown), o (OS specific), E (exclude),
  l (large), p (processor specific)
```

段首部表是一个以 `Elf64_Shdr` 结构体为元素的数组。数组元素个数等于段的个数，每个 `Elf64_Shdr` 结构体对应一个段。

`Elf64_Shdr` 称为段描述符（Section Descriptor）。对于 `SimpleSection.o` 来说，段首部表就是一个有 14 个元素的数组。第一个元素是无效的段描述符，类型为 `NULL`。其他段描述符都对应一个段，因此有 13 个有效的段。

`/usr/include/elf.h` 中的 `Elf64_Shdr` 结构体：

```c
typedef struct {
    Elf64_Word   sh_name;        /* Section name (string tbl index) */
    Elf64_Word   sh_type;        /* Section type */
    Elf64_Xword	 sh_flags;       /* Section flags */
    Elf64_Addr	 sh_addr;        /* Section virtual addr at execution */
    Elf64_Off	 sh_offset;      /* Section file offset */
    Elf64_Xword	 sh_size;        /* Section size in bytes */
    Elf64_Word	 sh_link;        /* Link to another section */
    Elf64_Word	 sh_info;        /* Additional section information */
    Elf64_Xword	 sh_addralign;   /* Section alignment */
    Elf64_Xword	 sh_entsize;     /* Entry size if section holds table */
} Elf64_Shdr;
```

1）`sh_name` 大小 4 字节，段的名称，指向 `.shstrtab` 字符串表的索引。

2）`sh_type` 大小 4 字节，描述段的类型。可取的值以 `SHT_` 开头，以下是部分：

| 常量           | 值   | 含义                                         |
| -------------- | ---- | -------------------------------------------- |
| `SHT_NULL`     | 0    | 无效段                                       |
| `SHT_PROGBITS` | 1    | 段包含了程序需要的数据，格式和含义由程序解释 |
| `SHT_SYMTAB`   | 2    | 包含一个符号表                               |
| `SHT_STRTAB`   | 3    | 包含一个字符串表                             |
| `SHT_RELA`     | 4    | 重定位表，包含重定位入口                     |
| `SHT_HASH`     | 5    | 包含一个符号散列表                           |
| `SHT_DYNAMIC`  | 6    | 包含动态链接的信息                           |

3）`sh_flags` 在 32 位中 4 字节，64 位中 8 字节。段的标志，每一位对应一个标志。常用的有：

| 常量            | 值           | 含义                     |
| --------------- | ------------ | ------------------------ |
| `SHF_WRITE`     | `0x1`        | 进程执行时段内的数据可写 |
| `SHF_ALLOC`     | `0x2`        | 进程执行时段需要占据内存 |
| `SHF_EXECINSTR` | `0x4`        | 段内包含可执行开机器指令 |
| `SHF_STRINGS`   | `0x20`       | 包含 0 结尾的字符串      |
| `SHF_MASKOS`    | `0x0ff00000` | 为特定 OS 保留 8 位        |
| `SHF_MASKPROC`  | `0xf0000000` | 为处理器保留             |

4）`sh_addr` 在 32 位中 4 字节，64 位中 8 字节。程序执行时段所在的虚拟地址。

5）`sh_offset` 在 32 位中 4 字节，64 位中 8 字节，段在文件内的偏移。

6）`sh_size` 在 32 位中 4 字节，64 位中 8 字节，段的大小。

7）`sh_link` 大小 4 字节，指向其他段的索引。

8）`sh_info` 大小 4 字节，存储额外信息，根据段的类型决定。

9）`sh_addralign` 在 32 位中 4 字节，64 位中 8 字节，段载入内存时按字节对齐。

10）`sh_entsize` 在 32 位中 4 字节，64 位中 8 字节，段内每条记录所占的字节数量。

结构体的十个成员对应于从 `Name` 开始的十个列。

段首部表长度为 `0x380`，即 `896` 字节，包含 14 个段描述符，每个段描述符 64 字节，等于 `sizeof(Elf64_Shdr)`。文件最后一个段 `Section Header Table` 结束后，长度为 `0x828`，即 `2088` 字节，即 `SimpleSection.o` 的文件大小。

