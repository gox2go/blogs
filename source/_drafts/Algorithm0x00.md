---
title: Algorithm0x00
date: 2020-06-09 07:56:45
tags: [Algorithm,C]
--

## 概述

## 内容

### 1.本题要求你写个程序把给定的符号打印成沙漏的形状。例如给定17个“*”，要求按下列格式打印

<!-- more -->

```
*****
 ***
  *
 ***
*****
```

所谓“沙漏形状”，是指每行输出奇数个符号；各行符号中心对齐；相邻两行符号数差2；符号数先从大到小顺序递减到1，再从小到大顺序递增；首尾符号数相等。

给定任意N个符号，不一定能正好组成一个沙漏。要求打印出的沙漏能用掉尽可能多的符号。

### 输入格式:

输入在一行给出1个正整数N（≤1000）和一个符号，中间以空格分隔。

### 输出格式:

首先打印出由给定符号组成的最大的沙漏形状，最后在一行中输出剩下没用掉的符号数。

### 输入样例:

```in
19 *
```

### 输出样例:

```out
*****
 ***
  *
 ***
*****
2
```

----

丑陋的代码。。。先贴在这里，等学完之后，希望能有变化。

```c
#include <stdio.h>
int getSum(int *k, int n) {
    int sum = -1;
    int prev = 0;
    while (sum <= n && n != 0) {
        prev = sum;
        sum += ((*k)++ * 2 + 1) * 2;
    };
    if (n != 0) {
        (*k)--;
    }
    return prev;
}
void printHourglass(int n, char ch) {
    int index = 0;
    int rest = n - getSum(&index, n);
    int line = 2 * index - 1;
    // print
    for (int i = 0; i < line; ++i) {
        if(i<index){ //[0,index]
            for (int j = 0; j < i; ++j) {
                printf(" ");
            }
            for (int j = 0; j < 2*(index-i)-1; ++j) {
                printf("%c",ch);
            }
            printf("\n");
        }else{ // i [index,2index -1]
            for (int j = 0; j <line-i-1 ; ++j) {
                printf(" ");
            }
            int newI = i-index+1;
            for (int j = 0; j < 2* newI +1 ; ++j) {
                printf("%c",ch);
            }
            printf("\n");
        }
    }
    printf("%d\n",rest);
}

int main() {
    int n;
    char ch;
    scanf("%d %c",&n,&ch);
    printHourglass(n,ch);
    return 0;
}
```



### 2.自测-2 素数对猜想 (20分)

让我们定义
$$
d_n为：d_n=p_{n+1}−p_n，
$$
其中 $ p_i $ 是第*i*个素数。显然有*d*1=1，且对于*n*>1有*d**n*是偶数。“素数对猜想”认为“存在无穷多对相邻且差为2的素数”。

现给定任意正整数`N`(<105)，请计算不超过`N`的满足猜想的素数对的个数。

### 输入格式:

输入在一行给出正整数`N`。

### 输出格式:

在一行中输出不超过`N`的满足猜想的素数对的个数。

### 输入样例:

```in
20
```

### 输出样例:

```out
4
```

----

这个代码我觉得不是很丑陋了

```c
#include <stdio.h>
#include <stdlib.h>

void printArr(int size, int *p)
{
    for (int j = 0; j < size; ++j) {
        printf("%d\t", p[j]);
        if ((j + 1) % 4 == 0) {
            printf("\n");
        }
    }
}

void getPrimeNum(int* p,int size)
{
    for (int j = 2; j < size; ++j) {
        int i = 2;
        while (i * j <= size) {
            p[i * j-1] = 0;
            i++;
        }
    }
}


void primeNum(int size) {
    int *p = (int *) malloc(sizeof(int) * size);
    for (int i = 0; i < size; ++i) {
        p[i] = i + 1;
    }
    printArr(size,p);
    getPrimeNum(p,size);
    printArr(size,p);
    int count = 0;
    for (int i = 2; i < size-2; ++i) {
        if(p[i]!=0 && p[i+2]!=0){
            count++;
            i=i+1;
        }
    }
    printf("%d\n",count);
    free(p);
}

int main() {
    int num;
    int n = scanf("%d",&num);
    primeNum(num);
    return 0;
}
```





### 3.自测-3 数组元素循环右移问题 (20分)

一个数组*A*中存有*N*（>0）个整数，在不允许使用另外数组的前提下，将每个整数循环向右移*M*（≥0）个位置，即将*A*中的数据由（*A*0*A*1⋯*A**N*−1）变换为（*A**N*−*M*⋯*A**N*−1*A*0*A*1⋯*A**N*−*M*−1）（最后*M*个数循环移至最前面的*M*个位置）。如果需要考虑程序移动数据的次数尽量少，要如何设计移动的方法？

### 输入格式:

每个输入包含一个测试用例，第1行输入*N*（1≤*N*≤100）和*M*（≥0）；第2行输入*N*个整数，之间用空格分隔。

### 输出格式:

在一行中输出循环右移*M*位以后的整数序列，之间用空格分隔，序列结尾不能有多余空格。

### 输入样例:

```in
6 2
1 2 3 4 5 6
```

### 输出样例:

```out
5 6 1 2 3 4
```

----

**感觉似乎有点投机取巧 而且耗时很久 12ms 看其他答案都是3-5ms..感觉有性能问题**

```c
#include <stdio.h>

void shiftright() {
    // get numbers
    int n;
    scanf("%d", &n);
    int off;
    scanf("%d", &off);
    int arr[n];
    printf("sizeof arr %d. off = %d\n", sizeof(arr), off);
    // shift right
    off = off % n;
    for (int i = 0; i < n; ++i) {
        if(i+off<n){
            scanf("%d",&arr[i+off]);
        }else{
            scanf("%d",&arr[i+off-n]);
        }
    }
    for (int j = 0; j < n; ++j) {
        if(j!=n-1){
            printf("%d ",arr[j]);
        }else{
            printf("%d",arr[j]);
        }
    }
}
int main()
{
    shiftright();
    return 0;
}
```



### 4.Notice that the number 123456789 is a 9-digit number consisting exactly the numbers from 1 to 9, with no duplication. Double it we will obtain 246913578, which happens to be another 9-digit number consisting exactly the numbers from 1 to 9, only in a different permutation. Check to see the result if we double it again!

Now you are suppose to check if there are more numbers with this property. That is, double a given number with *k* digits, you are to tell if the resulting number consists of only a permutation of the digits in the original number.

### Input Specification:

Each input contains one test case. Each case contains one positive integer with no more than 20 digits.

### Output Specification:

For each test case, first print in a line "Yes" if doubling the input number gives a number that consists of only a permutation of the digits in the original number, or "No" if not. Then in the next line, print the doubled number.

### Sample Input:

```in
1234567899
```

### Sample Output:

```out
Yes
2469135798
```

----

思路，我想用char *p 接收他们.试试看

折磨死了.... 学到了`malloc`和`calloc`的区别...学到了不轻易定义野指针，不轻易在stack里定义野指针......你知道`0x0df0adba`是什么吗...不想活了...

```c
void manipulateNumbers() {
    //1. test
    char *p= (char*)calloc(21, sizeof(char));
    int numcounts[10] = {0};
    int *const intp = (int *) calloc(21,sizeof(int));
    scanf("%s", p);

    // void fillupIntp(int *p,char *p) 然后按位计数
    const int len = strlen(p);
    for (int i = 0; i < len; ++i) {
        int num = (int) (*(p + len - i - 1) - '0');
        numcounts[num]++;
        *(intp + i) = num;
    }
    // 计算 *2 数值
    int forword = 0; // 进位标志
    int *temp = intp;
//    while (*temp != '\0') {
//    while (*temp != '\0') {
    int step =0;
    while(step<len)
    {
        step++;
        int product = *temp * 2 + forword;
        int oneplace = product % 10;
        forword = product / 10;

        // 赋值
        *temp = oneplace;
        // numcounts --
        numcounts[oneplace]--;
        temp++;
    }
    if (forword != 0) {
        *temp = forword;
    } else {
        temp--;
    }
    //打印乘积
    int result = 0; // all 0;
    for (int j = 0; j < 10; ++j) {
        if (numcounts[j] != 0)
        {
            result = 1;
            break;
        }
    }
    printf("%s\n",result==0?"Yes":"No");
    while (temp >= intp)
        printf("%d", *temp--);
    free(p);
    free(intp);
}
```



### 5.Shuffling Machine (20分)

Shuffling is a procedure used to randomize a deck of playing cards. Because standard shuffling techniques are seen as weak, and in order to avoid "inside jobs" where employees collaborate with gamblers by performing inadequate shuffles, many casinos employ **automatic shuffling machines**. Your task is to simulate a shuffling machine.

The machine shuffles a deck of 54 cards according to a given random order and repeats for a given number of times. It is assumed that the initial status of a card deck is in the following order:

```
S1, S2, ..., S13, 
H1, H2, ..., H13, 
C1, C2, ..., C13, 
D1, D2, ..., D13, 
J1, J2
```

where "S" stands for "Spade", "H" for "Heart", "C" for "Club", "D" for "Diamond", and "J" for "Joker". A given order is a permutation of distinct integers in [1, 54]. If the number at the *i*-th position is *j*, it means to move the card from position *i* to position *j*. For example, suppose we only have 5 cards: S3, H5, C1, D13 and J2. Given a shuffling order {4, 2, 5, 3, 1}, the result will be: J2, H5, D13, S3, C1. If we are to repeat the shuffling again, the result will be: C1, H5, S3, J2, D13.

### Input Specification:

Each input file contains one test case. For each case, the first line contains a positive integer *K* (≤20) which is the number of repeat times. Then the next line contains the given order. All the numbers in a line are separated by a space.

### Output Specification:

For each test case, print the shuffling results in one line. All the cards are separated by a space, and there must be no extra space at the end of the line.

### Sample Input:

```in
2
36 52 37 38 3 39 40 53 54 41 11 12 13 42 43 44 2 4 23 24 25 26 27 6 7 8 48 49 50 51 9 10 14 15 16 5 17 18 19 1 20 21 22 28 29 30 31 32 33 34 35 45 46 47
```

### Sample Output:

```out
S7 C11 C10 C12 S1 H7 H8 H9 D8 D9 S11 S12 S13 D10 D11 D12 S3 S4 S6 S10 H1 H2 C13 D2 D3 D4 H6 H3 D13 J1 J2 C1 C2 C3 C4 D1 S5 H5 H11 H12 C6 C7 C8 C9 S2 S8 S9 H10 D5 D6 D7 H4 H13 C5
```

```
S7 C11 C10 C12 S1 H7 H8 H9 D8 D9 S11 S12 S13 D10 D11 D12 S3 S4 S6 S10 H1 H2 C13 D2 D3 D4 H6 H3 D13 J1 J2 C1 C2 C3 C4 D1 S5 H5 H11 H12 C6 C7 C8 C9 S2 S8 S9 H10 D5 D6 D7 H4 H13 C5
```

----

对不起，我不知道为什么，我不知道这题怎么做，我也不知道我为什么对了，也不知道为什么错了。

代码贴上来。学完回头看。

```c
//
// Created by ShenGuoxiang on 2020/6/10.
//
#include <stdio.h>
#include <string.h>
#include <stdlib.h>

//void initCards(char cards[][4], int len) {
//    char colors[5] = {'S', 'H', 'C', 'D', 'J'};
//    int nums = 13;
//    int i = 0;
//    int colorSize = sizeof(colors);
//    for (int j = 0; j < colorSize; ++j) {
//        if (j == colorSize - 1) {
//            for (int k = 0; k < 2; ++k) {
//                cards[i][0] = colors[j];
//                char p[2];
//                sprintf(p, "%d", k+1);
//                strcat(cards[i++], p);
//            }
//        } else {
//            for (int k = 0; k < nums; ++k) {
//                cards[i][0] = colors[j];
//                char p[2];
//                sprintf(p, "%d", k+1);
//                strcat(cards[i++], p);
//            }
//        }
//    }
//}

void initCards(char** cards,int len)
{
    char *colors[5] = {"S", "H", "C", "D", "J"};
    int nums = 13;
    int i = 0;
    int colorSize = sizeof(colors)/ sizeof(char*);
    for (int j = 0; j < colorSize; ++j) {

        if (j == colorSize - 1) {
            for (int k = 0; k < 2; ++k) {
                cards[i] = (char*)calloc(4, sizeof(char));
                char *p = (char*)calloc(2, sizeof(char));
                strcat(cards[i],colors[j]);
                sprintf(p, "%d", k+1);
                strcat(cards[i++], p);
                free(p);
            }
        } else {
            for (int k = 0; k < nums; ++k) {
                cards[i] = (char*)calloc(4, sizeof(char));
                char *p = (char*)calloc(2, sizeof(char));
                strcat(cards[i],colors[j]);
                sprintf(p, "%d", k+1);
                strcat(cards[i++], p);
                free(p);
            }
        }
    }
}



void printCards(char **cards, int len)
{
    for (int j = 0; j < len; ++j) {
        if(j!=len-1)
        {
            printf("%s ",cards[j]);
        }else{
            printf("%s",cards[j]);
        }
    }
}
int getFinalPosition(int index,int repeatTimes,int permutation[])
{
    int position = index;
    if(permutation[index]==index+1) // permutation = [1,54] index = [0,53]
        return position;
    for (int i = 0; i < repeatTimes; ++i) {
        position = permutation[position];
        position--;
    }
    return position;
}

void shuffleCards(int repeatTimes,int permutation[]) {

//    char **cards = (char**)calloc(55, sizeof(char*));
    char *cards[54] = {"S1", "S2", "S3", "S4", "S5", "S6", "S7", "S8", "S9", "S10", "S11","S12","S13",
                      "H1", "H2", "H3", "H4", "H5", "H6", "H7", "H8", "H9", "H10", "H11","H12","H13",
                      "C1", "C2", "C3", "C4", "C5", "C6", "C7", "C8", "C9", "C10", "C11","C12","C13",
                      "D1", "D2", "D3", "D4", "D5", "D6", "D7", "D8", "D9", "D10", "D11","D12","D13",
                      "J1","J2"};
    char **cardResult = (char**)calloc(55, sizeof(char*));
//    int len = sizeof(cards) / sizeof(cards[0]);
    int len = 54;
//    initCards(cards, len);
    for (int i = 0; i < len; ++i) {
        int position = getFinalPosition(i,repeatTimes,permutation);
        cardResult[position] = (char*)calloc(4, sizeof(char*));
        strcpy(cardResult[position],cards[i]);
    }

    // print
    printCards(cardResult,len);
//    for (int j = 0; j < len; ++j) {
//        free(cardResult[j]);
//        free(cards[j]);
//    }
//    free(cards);
//    free(cardResult);

}


int main() {
//    int permutation[54] = {36,52,37,38,3,39,40,53,54,41,
//                           11,12,13,42,43,44,2,4,23,24,25,
//                           26,27,6,7,8,48,49,50,51,9,
//                           10,14,15,16,5,17,18,19,1,20,
//                           21,22,28,29,30,31,32,33,34,
//                           35,45,46,47};
//
//    int repeatTimes = 2;
    int permutation[54];
    int repeatTimes ;
    scanf("%d",&repeatTimes);
    for (int i = 0; i < sizeof(permutation)/ sizeof(int); ++i) {
        scanf("%d",permutation+i);
    }
    shuffleCards(repeatTimes,permutation);
    return 0;
}
```



#### 至此，5题结束。