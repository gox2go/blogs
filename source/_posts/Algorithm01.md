---
title: Algorithm01
date: 2020-06-11 09:43:26
tags: [Algorithm, C]
---

# 概述

算法第一周的一些PAT题目.旨在记录，用于之后回头看。

# 内容

## 1. 二分查找

本题要求实现二分查找算法。

### 函数接口定义：

```c++
Position BinarySearch( List L, ElementType X );
```

其中`List`结构定义如下：

```c++
typedef int Position;
typedef struct LNode *List;
struct LNode {
    ElementType Data[MAXSIZE];
    Position Last; /* 保存线性表中最后一个元素的位置 */
};
```

`L`是用户传入的一个线性表，其中`ElementType`元素可以通过>、==、<进行比较，并且题目保证传入的数据是递增有序的。函数`BinarySearch`要查找`X`在`Data`中的位置，即数组下标（注意：元素从下标1开始存储）。找到则返回下标，否则返回一个特殊的失败标记`NotFound`。

### 裁判测试程序样例：

```c++
#include <stdio.h>
#include <stdlib.h>

#define MAXSIZE 10
#define NotFound 0
typedef int ElementType;

typedef int Position;
typedef struct LNode *List;
struct LNode {
    ElementType Data[MAXSIZE];
    Position Last; /* 保存线性表中最后一个元素的位置 */
};

List ReadInput(); /* 裁判实现，细节不表。元素从下标1开始存储 */
Position BinarySearch( List L, ElementType X );

int main()
{
    List L;
    ElementType X;
    Position P;

    L = ReadInput();
    scanf("%d", &X);
    P = BinarySearch( L, X );
    printf("%d\n", P);

    return 0;
}

/* 你的代码将被嵌在这里 */
```

### 输入样例1：

```in
5
12 31 55 89 101
31
```

### 输出样例1：

```out
2
```

### 输入样例2：

```
3
26 78 233
31
```

### 输出样例2：

```
0
```

接口代码

```c
Position BinarySearch(List L, ElementType X) {
    Position highPosition = L->Last;
    Position lowPosition = 1;
    Position mid;
    Position result = NotFound;
    while (highPosition >= lowPosition) {
        mid = (highPosition + lowPosition) / 2;
        if (X > L->Data[mid]) {
            lowPosition = mid + 1;
        } else if (X < L->Data[mid]) {
            highPosition = mid - 1;
        }else{
            return mid;
        }
    }
    return result;
}
```

## 2. **Maximum Subsequence Sum**

Given a sequence of *K* integers { *N*1, *N*2, ..., *N**K* }. A continuous subsequence is defined to be { *N**i*, *N**i*+1, ..., *N**j* } where 1≤*i*≤*j*≤*K*. The Maximum Subsequence is the continuous subsequence which has the largest sum of its elements. For example, given sequence { -2, 11, -4, 13, -5, -2 }, its maximum subsequence is { 11, -4, 13 } with the largest sum being 20.

Now you are supposed to find the largest sum, together with the first and the last numbers of the maximum subsequence.

### Input Specification:

Each input file contains one test case. Each case occupies two lines. The first line contains a positive integer *K* (≤10000). The second line contains *K* numbers, separated by a space.

### Output Specification:

For each test case, output in one line the largest sum, together with the first and the last numbers of the maximum subsequence. The numbers must be separated by one space, but there must be no extra space at the end of a line. In case that the maximum subsequence is not unique, output the one with the smallest indices *i* and *j* (as shown by the sample case). If all the *K* numbers are negative, then its maximum sum is defined to be 0, and you are supposed to output the first and the last numbers of the whole sequence.

### Sample Input:

```in
10
-10 1 2 3 4 -5 -23 3 7 -21
```

### Sample Output:

```out
10 1 4
```

丑陋的代码...请您欣赏..算了别看了折腾死了

```c
//
// Created by Afrankis on 2020/6/11.
//

#include <stdio.h>

#define MAXSIZE 100000
#define MINIMUM 0

typedef int summary;
typedef int ElementType;

summary maxSubsequence(ElementType len, ElementType *list) {
    summary sum = MINIMUM;
    summary tempSum = sum;
    int step = 0;
    int anchor = step;
    int allNeg = 1;
    int allZero = 1;
    int tempStAnchor = anchor; // 可能起点锚点
    int tempEdAnchor = step; // 可能结束锚点
    int edAnchor = tempEdAnchor;
    for (; step < len; ++step) { // 一次遍历
        if (tempSum + list[step] < 0) { //小于零
            tempStAnchor = (step + 1) < len ? (step + 1) : tempStAnchor; // anchor 进一位
            tempSum = 0;
        } else { // sum + arr[step] >=0
            allNeg = 0;
            tempSum += list[step];
            tempEdAnchor = step;
            if (tempSum > sum) { // 增量
                allZero = 0;
                sum = tempSum; // 更新
                edAnchor = tempEdAnchor;
                anchor = tempStAnchor;
            } else { //小于，或者维持。什么都不做

            }
        }
    }
    if (allNeg == 1) {
        printf("%d %d %d",sum,list[0],list[len-1]);
        return 0;
    }
    if(allZero){
        printf("0 0 0");
        return 0;
    }
    printf("%d %d %d", sum, list[anchor], list[edAnchor]);
    return 0;
//    printf("%d",sum);
}


int main() {
//    int len = 10;
//    int list[10] = {-10,1,2,3,4,-5,-23,3,7,-21};
//    int len = 6;
//    int list[10] = {-2,11,-4,13,-5,-2};
    int len;
    int list[MAXSIZE];
    scanf("%d", &len);
    for (int i = 0; i < len; ++i) {
        scanf("%d", &list[i]);
    }
    maxSubsequence(len, list);
    return 0;
}

```



-----

# 总结

**第一题是关于有序数列二分查找；**

​	关键是取mid时候的进位，然后记得及时return就好。

**第二题是关于动态规划。**

​	关键是...emmm...我也还没找到关键。目前看来，需要选定锚点，注意移动的锚点和给锚点赋值的时机。欣赏一下其他人代码，然后再来改进。