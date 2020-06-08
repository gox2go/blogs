---
title: C_Memory
tags: C Memory
date: 2020-06-07 22:52:54
---


*c语言学习记录第一篇，内容包括：学习笔记，代码内容，资料和书籍内容查看*

## 1. 概要 

<!--已存在知识内容回述-->

**作用域、内存布局、堆空间开辟与释放**

**作用域**

​	包括变量、常量、functions 的生命周期 和有效范围；

**内存布局**

​	C语言的 四块内存布局

​	对于可执行文件的内存布局查看

​	每种内存块所包含的内容

**堆空间**

​	堆空间的开辟与释放

​	一些误区和其他内容

## 2. 详细内容

<!--包括一些记录和代码块-->

### 2.1 作用域

提到作用域？scope——(相当于在说 )某个对象/变量的作用范围：1.就近、2从定义开始。

没什么好说的了.

例外的是

**全局变量&声明&静态全局变量**

```c
// file A.c
#include <stdio.h>

int globalA00;
extern int globalB00;
extern int globalB01; 
int main()
{
  printf("%#x\n",globalB00); // 0x00;
  printf("%#x\n",globalB01); // error not defined variable 'globalB01';
  return 0;
}

// file B.c
#include <stdio.h>
int globalB00 = 0x00;
static int globalB01 = 0x01; // 作用域范围 --> 当前文件内
```

**关于函数形参初始化/定义的顺序**

```c
void testfunction(int arg00, int arg01, int arg02)
{
  printf("%p\t%p\t%p\n",&arg00,&arg01,&arg02);
}
```

1. `arg00,arg01,arg02`在 **栈区stack**中申请内存来保存这些变量
2. **栈区stack**的保存方式由高位到低位——地址位——的(`H (0xff00)->L(0x0000)`) 具体在[2.2](#2.2 内存布局)
3. 定义顺序是 从右至左——`arg02->arg01->arg00` 

**?是吗，事实却矛盾了～**

```c
#include <stdio.h>

int main()
{
  testfunction(1,1,1);
  return 0;
}
```


# ?

##### 关于`extern`关键字的使用~

##### 关于`auto`关键字的使用~

### 2.2 内存布局

申请内存，什么时候释放？

这些内存共享又是在什么阶段共享？——所说的一定是在可执行文件的过程中，在运行过程中。

四级布局

- 代码区（最低位）

  共享代码？所谓共享是和什么共享？

- 静态区

  常量区 -- 字符串常量区

  Y：`char *p = "Hello My first Blog.";`

  N：`chra p[]="Hello, I\'m a counterexample."`

  全局变量/global variables （）

  静态全局变量

  静态局部变量

  

- 堆区

  `malloc()`在堆区申请内存，在程序结束后释放或者 `free()` 调用时候释放

- 栈区（最高位）

  变量 ——局部变量、数组、指针、结构体、枚举类型、函数形参、函数返回值

  ​	

  

### 2.3 堆空间

```c 
char *p = (char*)malloc(sizeof(char)*100); // 申请100个char内存大小的堆区内存 假设1Byte 100Byte.
free(p); // 释放掉
int *p = (int*)malloc(sizof(int)*1000) // 申请1000个int类型内存大小的堆区内存 设 sizeof(int) == 4 4000Byte~~ 4KB.
```



## 3. 思考和答案

## 4. else

<!--关于 1byte 8bit 00 00 00 00 的存储和地址的一些事儿-->

<!--关于 数据在ide中显示的事情-->

调试的时候发现, 比如

`int a = 10`

对应的存储补码	`0x 00 00 00 0a`

事实上在ide里      `0x 0a 00 00 00`

再比如 `int a =-258`

对应的存储补码	`0x ff ff fe fe`

事实上在ide里 	`0x fe fe ff ff`

**原因是什么呢？？？**

事实上，上面说的是错的。

~~和IDE有关吗？ 不知道。~~

和什么有关呢？~~和计算机相关。~~ 不，事实上和处理器相关

> - [x86](https://zh.wikipedia.org/wiki/X86)、[MOS Technology 6502](https://zh.wikipedia.org/wiki/MOS_Technology_6502)、[Z80](https://zh.wikipedia.org/wiki/Z80)、[VAX](https://zh.wikipedia.org/wiki/VAX)、[PDP-11](https://zh.wikipedia.org/wiki/PDP-11)等处理器为小端序；
> - [Motorola 6800](https://zh.wikipedia.org/wiki/Motorola_6800)、[Motorola 68000](https://zh.wikipedia.org/wiki/Motorola_68000)、[PowerPC 970](https://zh.wikipedia.org/w/index.php?title=PowerPC_970&action=edit&redlink=1)、[System/370](https://zh.wikipedia.org/w/index.php?title=System/370&action=edit&redlink=1)、[SPARC](https://zh.wikipedia.org/wiki/SPARC)（除V9外）等处理器为大端序；
> - [ARM](https://zh.wikipedia.org/wiki/ARM)、[PowerPC](https://zh.wikipedia.org/wiki/PowerPC)（除PowerPC 970外）、[DEC Alpha](https://zh.wikipedia.org/wiki/DEC_Alpha)、[SPARC V9](https://zh.wikipedia.org/wiki/SPARC)、[MIPS](https://zh.wikipedia.org/wiki/MIPS)、[PA-RISC](https://zh.wikipedia.org/wiki/PA-RISC)及[IA64](https://zh.wikipedia.org/wiki/IA64)的字节序是可配置的。

那原因是什么呢？

**[大端法Big Endian和小端法Little Endian(Its called Endianness.)](https://zh.wikipedia.org/wiki/字节序)** 

我所看到的从小位->大位的就是 小端法的排序方式

关于这个的是 **“计算机系统”** 



