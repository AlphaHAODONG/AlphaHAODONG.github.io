# 内功一阶


# 1、[USACO09OCT] The Robot Plow G

## 题目描述 

Farmer John has purchased a new robotic plow in order to relieve him from the drudgery of plowing field after field after field. It achieves this goal but at a slight disadvantage: the robotic plow can only plow in a perfect rectangle with sides of integer length.

Because FJ's field has trees and other obstacles, FJ sets up the plow to plow many different rectangles, which might end up overlapping. He's curious as to just how many squares in his field are actually plowed after he programs the plow with various plow instructions, each of which describes a rectangle by giving its lower left and upper right x,y coordinates.

As usual, the field is partitioned into squares whose sides are parallel to the x and y axes. The field is X squares wide and Y squares high (1 <= X <= 240; 1 <= Y <= 240). Each of the I (1 <= I <= 200) plow instructions comprises four integers: Xll, Yll, Xur, and Yur (1 <= Xll <= Xur; Xll <= Xur <= X; 1 <= Yll <= Yur; Yll <= Yur <= Y) which are the lower left and upper right coordinates of the rectangle to be plowed. The plow will plow all the field's squares in the range (Xll..Xur, Yll..Yur) which might be one more row and column than would initially be assumed (depending on how you go about your assumptions, of course).

Consider a field that is 6 squares wide and 4 squares high. As FJ issues a pair of plowing instructions (shown), the field gets plowed as shown by '\*' and '#' (normally plowed field all looks the same, but '#' shows which were most recently plowed):

```cpp
......             **....             #####. 
......  (1,1)(2,4) **....  (1,3)(5,4) #####. 
......             **....             **.... 
......             **....             **.... 
```
A total of 14 squares are plowed.

POINTS: 25

Farmer John为了让自己从无穷无尽的犁田工作中解放出来，于是买了个新机器人帮助他犁田。这个机器人可以完成犁田的任务，可惜有一个小小的缺点：这个犁田机器人一次只能犁一个边的长度是整数的长方形的田地。

因为FJ的田地有树和其它障碍物，所以FJ设定机器人去犁很多不同的长方形。这些长方形允许重叠。他给机器人下了P个指令，每个指令包含一个要犁长方形的地。这片田地由长方形的左下角和右上角坐标决定。他很好奇最后到底有多少个方格的地被犁过了。

一般来说，田地被分割为很多小方格。这些方格的边和x轴或y轴平行。田地的宽度为X个方格，高度为Y个方格 (1 <= X <= 240; 1 <= Y <= 240). FJ执行了I (1 <= I <= 200)个指令，每个指令包含4个整数：Xll, Yll, Xur, Yur (1 <= Xll <= Xur; Xll <= Xur <=X; 1 <= Yll <= Yur; Yll <= Yur <= Y), 分别是要犁的长方形的左下角坐标和右上角坐标。机器人会犁所有的横坐标在Xll..Xur并且纵坐标在Yll..Yur范围内的所有方格的地。可能这个长方形会比你想象的多一行一列（就是说从第Xll列到第Xur列一共有Xur - Xll + 1列而不是Xur - Xll列）。

考虑一个6方格宽4方格高的田地。FJ进行了2个操作（如下），田地就被犁成"\*"和"#"了。虽然一般被犁过的地看起来都是一样的。但是标成"#"可以更清晰地看出最近一次被犁的长方形。

一共14个方格的地被犁过了。

## 输入格式

\* Line 1: Three space-separated integers: X, Y, and I

\* Lines 2..I+1: Line i+1 contains plowing instruction i which is described by four integers: Xll, Yll, Xur, and Yur

## 输出格式

\* Line 1: A single integer that is the total number of squares plowed

## 样例 #1

### 样例输入 #1

```
6 4 2 
1 1 2 4 
1 3 5 4
```

### 样例输出 #1

```
14
```

## 提示

As in the task's example.

题目意思并不是一针见血的，分析如下：

​		有一个6 x 4的方格，输入了2条指令，第一条指令是(1,1)和(2,4)围成的范围，一共是2 x 4 = 8个格子，第二条指令是(1,3)和(5,4)围成的范围，一共是5 x 2 = 10个格子。但是中间有（1,3）（1,4）（2,3）（2,4）是重复的格子，所以要减去，即最后答案为18 - 4 =14 个格子。

​		第一次做题的时候可能会想去求出重复格子的个数，这种做法是可行的，但是不可取，所以换个思路，定义一个bool类型的数组，当某个格子被访问时，把它的值变为1，最后统计值为1的格子个数就可以了。。。

```cpp
#define _CRT_SECURE_NO_WARNINGS
#include <iostream>
using namespace std;
#include <string>
#include<cstring>
bool a[240 + 5][240 + 5];

//将访问到的每个格子状态变为1
void ex(int x, int y, int s, int t)
{
    for (int i = x; i <= s; i++)
    {
        for (int j = y;j <= t; j++)
        {
            a[i][j] = 1;
        }
    }
}
int main()
{
    int m, n, c;
    int x, y, s, t, count = 0;
    memset(a, 0, sizeof(a));
    cin >> m >> n >> c;
    for (int i = 0; i < c; i++)
    {
        cin >> x >> y >> s >> t;
        ex(x, y, s, t);
    }

    //统计在范围内有多少个格子变为1了
    for (int i = 1; i <= m; i++)
    {
        for (int j = 1; j <= n; j++)
        {
            if (a[i][j])
                count++;
        }
    }
    cout << count;
    system("pause");
    return 0;
}
```

# 2、[COCI2006-2007#1] Modulo

## 题面翻译

给出 $10$ 个整数，问这些整数除以 $42$ 后得到的余数有多少种。

- 第一个样例的十个结果是 $1,2,3,4,5,6,7,8,9,10$，有 $10$ 个不同的结果；
- 第二个样例结果都是 $0$，只有一个不同的结果；
- 第三个样例余数是 $39,40,41,0,1,2,40,41,0,1$，有 $0,1,2,39,40,41$ 这六个不同的结果。

## 题目描述

Given two integers A and B, A modulo B is the remainder when dividing A by B. For example, the numbers 7, 14, 27 and 38 become 1, 2, 0 and 2, modulo 3. Write a program that accepts 10 numbers as input and outputs the number of distinct numbers in the input, if the numbers are considered modulo 42.

## 输入格式

The input will contain 10 non-negative integers, each smaller than 1000, one per line.

## 输出格式

Output the number of distinct values when considered modulo 42 on a single line.

## 样例 #1

### 样例输入 #1

```
1
2
3
4
5
6
7
8
9
10
```

### 样例输出 #1

```
10
```

## 样例 #2

### 样例输入 #2

```
42
84
252
420
840
126
42
84
420
126
```

### 样例输出 #2

```
1
```

## 样例 #3

### 样例输入 #3

```
39
40
41
42
43
44
82
83
84
85
```

### 样例输出 #3

```
6
```

## 提示

In the first example, the numbers modulo 42 are 1, 2, 3, 4, 5, 6, 7, 8, 9 and 10.
In the second example all numbers modulo 42 are 0.
In the third example, the numbers modulo 42 are 39, 40, 41, 0, 1, 2, 40, 41, 0 and 1. There are 6 distinct numbers.

​		首先来分析：题目给出10个数，每个数除以42，要我们求出余数的种类个数，相同的余数算1个种类。

​		你可能会想到：先创建一个集合，把每一次得到的余数与集合中的元素比较，如果集合中没有此元素，则存入集合，那么有没有更简单的方法？

​		**思路如下：**

1. 用上桶排序，定义一个大小为42的bool类型数组`a[42]`，初始值为0。
2. 每一次求出的余数可以看做数组下标，把它的值变为1。`a[余数]=1`
3. 最后求出`a[i]=1`的个数

```cpp
#define _CRT_SECURE_NO_WARNINGS
#include <iostream>
using namespace std;
#include <string>
#include <cstring>
bool a[42];
int main()
{
    int sum = 0, x;
    memset(a, 0, sizeof(a));
    for (int i = 0; i < 10; i++)
    {
        cin >> x;
        a[x % 42] = 1;
    }
    for (int i = 0; i < 42; i++)
    {
        if (a[i])
        {
            sum++;
        }
    }
    cout << sum << endl;
    system("pause");
    return 0;
}
```


