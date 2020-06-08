---
title: C-structures
date: 2020-06-08 08:07:12
tags: C struct Union typedef Enum
---

## 0.概要

<!--包括的内容-->

结构体，`Union`，`typedef`，`Enum`;

一些代码；

一些产生的疑惑，会在内容里产生，会在最后找到答案，或者没找到。

## 1.内容

- **结构体`struct`**
  - 定义和使用，
  - 结构体大小、内存结构
  - 结构体和指针
  - 结构体和函数

- `Union`

- `ENUM`

- `typedef`

------

### 1.1 结构体

#### 	定义和使用

```c
// definition 在header文件内
struct Person 
{
    char name[20];
    unsigned int age;
    char gender;
}

// Usage 1.
// in source file
void function()
{
    struct Person p1; // variable
    struct Person * pp; // pointer
    
    
    // 变量使用 '.' 
    p1.age = 20;
    p1.gender = 'M';
    strcpy(p1.name,"Sam"); // ??char 数组的使用
    
    // 指针使用'->'
    pp->age = 30;
    pp->gender = 'F';
    strcpy(pp->gender,"Helen");
}
```

**上面的代码产生问题啦~**

```c
struct Person* pp;
pp->age = 30; // 到这里就崩溃了哦~
```

稍微分析一下，就明白，我定义了一个指针，却没告诉他指向，他就是NULL...然后操作指针...emmm.对不起。

#### 结构体和指针

```c
// 结构体指针
1. heap申请内存
struct Person* pp;
pp = (struct Person*)malloc(sizeof(struct Person));

2. 找stack里的地址
 Struct Person
{
    int ...;
    char ...;
}person;

struct Person* pp = &person; // 这样就可以啦~
```



****

### 1.2 `Union`

### 1.3 `typedef`

## 2.问题与可能的答案

**Q1**	

??char数组的使用

`char name[20];`   

`*name = "hello"`

有什么问题呢？

1. `char name[20];` 

```
1.在stack中申请内存
2.name是一个char* const 的指针 指向位置不能改变
	2.1 *name 的值可以改变.
	2.2 *name 表示的是 name[0], 是一个char类型的变量 
3.但是 sizeof(name) 表示的却是整个数组的长度. -- 20;
```

2. `*name = "hello"`; 有什么问题呢？

``` c
1. "hello" 在静态区字符串常量区 申请了一块内存. 
    1.1 如果 char* p = "hello"; //  p 指向字符串常量区地址， 
	1.2 可以理解"hello" 表示一个 const char*， 所以 p 指向 可以改，但是 *p 绝对不能改。
2. *name 是一个 char 类型的变量~ "hello" 字符串地址怎么能赋值给他呢？ 
3. 讲size的话，*name 是1byte == 8bits;"hello" 首地址在32为是4byte->32bits,64位os里8byte->64bits,会溢出的。
4. *name = "hello" 相当于截取了 "hello" 地址位的最后8位 赋值给了 name[0];     
```

3. **很正常的产生了一个问题，怎么去赋值未初始化的字符数组呢？**

```c
char name[20]; // char* const name , name 指针位置是动不了的哦
想给name一个 "Thomes" 的名字;
char* nameStep = name; // 找一个可以动的
const char *p = "Thomes"; // Thomes 是不能改动的哦
do
{
    *nameStep = *p;
    p++;
    nameStep++;
    
}while(*p != '\0') 
// 这就是strcpy的逻辑，实际上strcpy更有趣一点~ 它只移动一个指针
char * strcpy(char* name, const char *p)
{	
    char ch;
    char *step = p;
    const long long offset = name - p -1 ; // 内存位置差
    do
    {	
        ch = *step++;
        step[offset] = ch;
    }while(ch !='\0');
    return name;
}
    
// 事实上，是有问题的，如果name给的位数不够怎么办？ ->有个函数叫 strncpy.
```

关于[strncpy](https://devdocs.io/c/string/byte/strncpy)

**Q2**	

?? 哪一种是正确的？

```c
const char *p ="123";
printf("%s\n",*p);
printf("%s\n",p);
```

```c
1.p是什么？*p是什么
    1.1 p 是指针 指向字符串常量区"123" 的首地址
    1.2 p 的大小是 指针大小 
    1.3 *p 是一个大小的值 就是"123" 收地址代表的字符的值那么->
    	printf("P[0]= %c\n",*p);是可以的~ 且能打印 ‘1’
2."%s"表示什么？ //https://devdocs.io/c/io/fprintf
   参数必须是一个指针，而且指针要指向字符数组首字母，大小要明确，如果不明确就 打印每个字节直到第一个'null'   
```

> writes a **character string**
>
> [The argument must be a pointer to the initial element of an array of characters. *Precision* specifies the maximum number of bytes to be written. If *Precision* is not specified, writes every byte up to and not including the first null terminator.](https://devdocs.io/c/io/fprintf)



## 3.OTHERS



