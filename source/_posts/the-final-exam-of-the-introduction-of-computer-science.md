---
title: The final exam of the introduction of computer science
tags: []
id: '82'
categories:
  - - uncategorized
date: 2022-01-27 03:43:18
mathjax: true
---

### Question 1

1）Please explain why modern computers adopt binary systems? (5 marks)

2）What\`s the difference when storing an integer 1 and a character ’1’? (5 marks)

### Answer:

1）Binary numbers are ideal for machines because they require only two digits, which can easily be represented by the on and off states of a switch. When computers became electronic, the binary system was particularly appropriate because an electrical circuit is either on or off.

[https://www.britannica.com/technology/computer/History-of-computing](https://www.britannica.com/technology/computer/History-of-computing)

这是一个技术问题，不是科学问题，原则上什么进制都可以。

旋钮开关也是开关，只是on/off双态开关更容易实现、可靠性更高。还有一点就是，它能表达一切！

关键是开关电路和逻辑运算之间的联系才导致了现代计算机采用二进制系统。

题外话：1，据说Knuth有意设计十进制计算机系统；2，据说从信息论角度看e（自然对数的底）进制是信息密度最高的进制。3比2更靠近ee，不知道3态开关如何设计？

2）整数1和字符1的区别：

*   逻辑上是数据类型的不同，即语义解释不同；interger 1 是指整数1的二进制表示，character ’1’ 是指字符 ’1’ 的编码（ascii或任何其他编码方式）的二进制表示。
*   物理存储上的区别：存储空间上的区别，整数通常采用16bits或32bits存储，字符通常采用8bits存储。**注意**：这里有通常一词，也就是技术上完全可以采用同样的空间来存储。

### Question 2

How to test the equality of two floats in C? (5 marks), and why using ‘==’ is not an appropriate approach? (5 marks)

### Answer:

1，C语言中比较两个`float`相等通常采用`fabs(a-b)<1e-7`之类的方法，即两个`float a`和`b`的差值足够小即认为是相等的。

2，在C语言中用`==`比较时，如果两边的表达式的值在计算机中的表示完全一致就表示相等。大家知道浮点数在计算机中的表示不精确，所以，不能用`==`来作浮点数的相等比较。

**WAIT！WAIT！！WAIT！！！**

如果两个浮点数表达式`e1`和`e2`的值是一样的，难道它们在计算机中的表示不一样吗？当然不是啊。同样的值在计算机中的表示，尽管不精确，但必然也是一样的不精确！所以，用`==`比较没问题。

那么问题在哪儿呢？问题是，比如`e1=(a+b)+c`，`e2=a+(b+c)`，在数学上，`e1`和`e2`的**值**是相同的。但是在计算机上，由于计算顺序不同，每一步计算都可能引入误差，导致`e1`和`e2`的**值**不同！也就是说，结合律在计算机算术中并不成立！不同的值在计算机中的表示自然也就不同了。此时，如果用`==`作比较结果当然是不相等了。

虽然计算机表达不精确，但是在计算意义上有效位数的数值还是正确的。因此，对于不精确的浮点数而言，我们只比较其有效位数。

还有一个办法：把浮点数乘上106106取整，然后转换成浮点数，再除以106106。这样就可以用`==`来比较相等了。但是要注意整数的溢出问题！

### Question 3

Please describe the von Neumann architecture, and what is ALU？

### Answer:

1）The von Neumann architecture — also known as the von Neumann model or Princeton architecture — is a computer architecture based on a 1945 description by John von Neumann and others in the First Draft of a Report on the EDVAC. That **document** describes a design architecture for an electronic digital computer with these components:

*   A **processing unit** that contains an arithmetic logic unit and processor registers
*   A **control unit** that contains an instruction register and program counter
*   **Memory** that stores data and instructions, External mass storage
*   **Input** and **output** mechanisms

The term "von Neumann architecture" has evolved to mean any stored-program computer in which an instruction fetch and a data operation cannot occur at the same time because they share a common bus. This is referred to as the von Neumann bottleneck, and often limits the performance of the system.

其实，只是说出上述5大部件并不是对体系结构的描述！体系结构是指部件的划分以及这些部件之间的相互关系，包括连接和信息或控制流向，也就是说它的工作原理或者工作流程。至少得说出CPU中运行程序的三个阶段：取指令、指令解码、执行指令，的大致工作原理。**注意**，上面蓝色文字，整个**document**才描述了那个体系结构！

2）ALU stands for Arithmetic Logic Unit. 算术逻辑单元，做算术运算和逻辑运算的。

### Question 4

a) please design a new repetition structure that has the same function with the following code, while continue statement is not allowed. (5 marks)

```c
for (i=0; i<n; i++)
{
    ...
    if(i%2==0) continue;
}
```

b) Keep the while structure and eliminate the break statement. (5 marks)

```c
while (a)
{
    ...
    if (b)
        break;
} 
```

### Answer:

a）参考本站帖子，`continue`的意思是跳过下面的语句立刻进入下一轮循环。

```c
for (i = 0; i < n; i++)
{
    ...
    if (i % 2 == 0) continue;
    ...
    ...
}
```

消去`continue`，可以采用下面的代码结构：

```c
for (i = 0; i < n; i++)
{
    ...
    if (i % 2 != 0)
    {
        ...
        ...
    }
}
```

看清楚了，这里`if...continue`下面是有别的代码的。如果下面没有别的代码，如本题题面所描述的，直接删除`if...continue`语句即可。

另外，注意：如果把`for`中的终止条件改为`i < n && i % 2 != 0`是错误的，因为这样的话，`i`为奇数时就马上终止`for`循环了。还有，也不能用`i=i+2`来代替`i++`，因为循环体的前半部分需要对每个i循环。

下面的做法当然也没错：只是这不是结构化程序设计的初衷。

```c
for (i = 0; i < n; i++)
{
    ...
    if (i % 2 == 0) goto label;
    ...
label:  ;        // empty statement
}
```

b）参考本站帖子，`break`的意思是立刻终止并跳出循环。

```c
while (a)
{
    ...
    ...
    if (b) 
        break;
    ...
    ...
}
```

消去`break`，可以采用下面的代码结构：

```c
while (a && !b)
{
    ...
    ...
    if (!b) 
    {
        ...
        ...
    }
}
```

注意，这里`if`语句下面还有别的语句。如果没有别的语句，则可以删除`if`语句，只需要保留`while`中的条件即可。当然也可以用`goto`跳出循环。只是这不是结构化程序设计的初衷。 

```c
while (a)
{
    ...
    ...
    if (b) 
        goto label;
    ...
    ...
}
label: 
```

有同学采用do...while结构，这和消去break无关。

### Question 5

Write a program in C to print Fibonacci Series using recursion (5 marks) and repetition (5 marks) respectively.
$$
Fibonacci(n)= \left\{ {\begin{array}{ll}n & \text { if } n=0,1 \\ Fibonacci(n-1)+ Fibanacci(n-2) & \text { if } n \geqslant 2\\\end{array} } \right.
$$


### Answer:

打印Fibonacci Series？打印多少项？数列是无限的呀！还用递归打印？前面的项还不能为后面的项所利用！纯粹指数爆炸式的计算量啊，这得多奢侈啊！上课时特别提过，递归方式并不适合计算Fibonacci数，这还只是计算一个数哦，别说是整个数列了！

好吧，就当作练习吧。

还有，这个题目必须要给出需要打印的项数，否则只打印前2项或者前5项行不行？

另外，计算Fibonacci Series时，`int`类型很快会溢出。所以，最大项应该有所限制。可以计算到第`n = 46`项，接下来就会溢出了。大家可以run一下代码，看看是否越来越慢？

```c
#include <stdio.h>

int fib(int i);
int main()
{
    int f0 = 0,f1 = 1;
    int f2;
    int i, n;
    scanf("%d", &n);

    // print Fibonacci Series iteratively
    if (n >= 0)
        printf("%d\n", f0);
    if (n >= 1)
        printf("%d\n", f1);
    for (i = 2; i <= n; i++)
    {
        f2 = f0 + f1;
        printf("%d\n", f2);
        f0 = f1;
        f1 = f2;
    }

    // print Fibonacci Series recursively
    for (i = 0; i <= n; i++)
    {
        printf("%d\n", fib(i));
    }

    return 0;
}

int fib(int i)
{
    if (i < 2)
        return i;
    else
        return fib(i - 1) + fib(i - 2);
}
```

### Question 6

Write a program in C to print all unique elements in one dimensional array.

### Answer:

这题要求在一位数组中找出并打印那些唯一的元素。题面没说输入如何处理，那就随你。循规蹈矩的做法是用一个循环输入数据，一个循环输出数据，其中需要嵌套一个循环，以判断该元素是否在别处也出现过。

```c
#include <stdio.h>
#define SIZE 100

int main()
{
    int arr[SIZE];
    int i, j;
    int count;
    for (i = 0; i < SIZE; i++)
    {
        scanf("%d", &arr[i]);
    }
    for (i = 0; i < SIZE; i++)
    {
        count = 0;      // number of occurrences of arr[i]
        for (j = 0; count < 2 && j < SIZE; j++)
        {
            if (arr[i] == arr[j])
                count++;
        }
        if (count < 2)  // appears only once
            printf("%d\n", arr[i]);
    }

    return 0;
}
```

由于题面没规定输入数据，你完全可以自行设定数据。

*   全部初始化为00，则不用输出了。
*   初始化为0～n−10～n−1，则全部输出。

虽说这样不好，但也没错，和题目不矛盾。 

还有同学拿数据作下标用，类似基数排序。但是，数据可能很大，还可能有负数。这种做法在程序设计中肯定不可取。

### Question 7

Through investigation, the police determined that the murderer must be one of the four suspects. The following are the confessions of the four suspects: A said: not me. B said: it's C. C said: it's D. D said: C is lying! It is known that three people told the truth and one told a lie. Now, based on this information, please write a LOOP program to determine who is the murderer.

### Answer:

这个题目似乎见过。不过，那是为了写逻辑表达式。

题面描述是一个智力谜题，给定这样的条件，答案就是唯一的，只有一个答案，且永远不会变！

你经过推理分析，得出结论。这本身就是算法过程。所以，你写出`“答案是C”`就是最简答的算法，你写一个程序直接`printf("C");`就是最简单的程序，当然也应该算是对的！

好了，如果一定要在程序中判断求解，我们先得建立模型。你可以根据自己的理解来建立模型，可以简单可以复杂。

这里我们用字符`'A',` `'B'`, `'C'`,和`'D'`来分别表示4个嫌疑人。用字符变量s来遍历所有可能性，对于`s`的每一种可能，再根据四个嫌疑人的说辞进行真伪判断。比如，A said: not me。在模型中就是 `s != 'A'`。

*   `A said: not me. ---> s != 'A'`
*   `B said: it's C.    ---> s == 'C'`
*   `C said: it's D.    ---> s == 'D'`
*   `D said: C is lying! ---> s != 'D'`

题目条件告知有三句真话一句假话。也就是说模型中的解满足：四句话的真值加起来应该等于3。即

*   `((s != 'A') + (s == 'C') + (s == 'D') + (s != 'D')) == 3`

因此，一个常规的代码看上去是这样的：

```c
#include <stdio.h>

int main()
{
    char s;
    for (s = 'A'; s <= 'D'; s++)
    {
        if (((s != 'A') + (s == 'C') + (s == 'D') + (s != 'D')) == 3)
            printf("The murder is %c.\n", s);
    }
    return 0;
}
```

其实，这样的题目适合只写逻辑表达式。

### Question 8

Suppose in a test there are totally 4 questions that need to be answered, full mark of each question is 25. The minimum passing score of this test is 60. You need to design a structure to store this test result for each candidate (6 marks), find who failed to pass the test (7 marks), and print the result in descending order (7 marks).  
Input:  
4  
CS004 3 15 21 13  
CS003 5 14 21 23   
CS002 22 1 2 0   
CS001 12 22 13 15  
Among them, the first column gives the number of candidates. The next following column give a candidate's admission ticket number and the corresponding marks from question 1 to 4.  
Output requirements:  
Print the number of candidates who failed to pass this test in the first line, and display the results (including ticket number and test score) in a descending order according their test scores.  
Output:  
2  
CS004 52  
CS002 25

### Answer:

这个题目没啥意思，没啥可说的。需要注意的是：

*   1，设计结构体
*   2，输入输出，随便。
*   3，挑选出总分低于60的。
*   4，按降序打印，只打印那些低于60分的。

代码如下，相信大家都能看明白的。

```c
#include <stdio.h>
#define MAX 100
typedef struct
{
    char ticket[10];
    int c1, c2, c3, c4;
    int score;
} TEST;

int main()
{
    TEST stu[MAX];
    int orderd[MAX];
    int i, pass, failed, n;
    // input total number of candidates, n <= MAX
    scanf("%d", &n);
    // input all data，and count how many failed
    failed = 0;
    for (i = 0; i < n; i++)
    {
        scanf("%s", stu[i].ticket);
        scanf("%d%d%d%d", &stu[i].c1, &stu[i].c2, &stu[i].c3, &stu[i].c4);
        stu[i].score = stu[i].c1 + stu[i].c2 + stu[i].c3 + stu[i].c4;
        if (stu[i].score < 60)
            failed++;
    }

    // initialize order array
    for (i = 0; i < n; i++)
        orderd[i] = i;

    // sort stu array according to score decreasingly
    for (pass = 0; pass < n - 1; pass++)
    {
        for (i = 0; i < n - pass - 1; i++)
        {
            if (stu[orderd[i]].score < stu[orderd[i + 1]].score)
            {
                int temp;
                temp = orderd[i];
                orderd[i] = orderd[i + 1];
                orderd[i + 1] = temp;
            }
        }
    }

    // print out result
    printf("%d\n", failed);
    for (i = n - failed; i < n; i++)
    {
        printf("%s %d\n", stu[orderd[i]].ticket, stu[orderd[i]].score);
    }

    return 0;
}
```