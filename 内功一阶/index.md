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

# 3、子数整数

## 题目描述

对于一个五位数 $\overline{a_1a_2a_3a_4a_5}$，可将其拆分为三个子数：

$sub_1=\overline{a_1a_2a_3}$

$sub_2=\overline{a_2a_3a_4}$

$sub_3=\overline{a_3a_4a_5}$

例如，五位数 $20207$ 可以拆分成

$sub_1=202$

$sub_2=020\ (=20)$

$sub_3=207$

现在给定一个正整数 $K$，要求你编程求出 $10000$ 到 $30000$ 之间所有满足下述条件的五位数，条件是这些五位数的三个子数 $sub_1,sub_2,sub_3$ 都可被 $K$ 整除。

## 输入格式

一个正整数 $K$。

## 输出格式

每一行为一个满足条件的五位数，要求从小到大输出。不得重复输出或遗漏。如果无解，则输出 `No`。

## 样例 #1

### 样例输入 #1

```
15
```

### 样例输出 #1

```
22555
25555
28555
30000
```

## 提示

$0<K<1000$

​		**先来分析：**

​		这道题的重点在于如何取出三个子数，比较人性化的一点是题目限定了标准为五位数，所以只需要通过/和%的运用就可以取出三个子数。

```cpp
#define _CRT_SECURE_NO_WARNINGS
#include <bits/stdc++.h>
using namespace std;
int main()
{
    int k, sub1, sub2, sub3;
    bool flag = 0;
    cin >> k;
    for (int i = 10000; i <= 30000; i++) 
    {
        sub1 = i / 100;
        sub2 = (i / 10) % 1000;
        sub3 = i % 1000;
        if (sub1 % k == 0 && sub2 % k == 0 && sub3 % k == 0)
        {
            flag = 1;
            cout << i << endl;
        }
    }
    if (!flag)
    {
        cout << "No" << endl;
    }
    system("pause");
    return 0;
}
```

# 4、The Blocks Problem

## 题面翻译

初始时从左到右有 $n$ 个木块，编号为 $0 \ldots n-1$,要求实现下列四种操作：

- `move a onto b` : 把 $a$ 和 $b$ 上方的木块归位，然后把 $a$ 放到 $b$ 上面。
- `move a over b` : 把 $a$ 上方的木块归位，然后把 $a$ 放在 $b$ 所在木块堆的最上方。
- `pile a onto b` : 把 $b$ 上方的木块归位，然后把 $a$ 及以上的木块坨到 $b$ 上面。
- `pile a over b` : 把 $a$ 及以上的木块坨到 $b$ 的上面。
- 一组数据的结束标志为 `quit`，如果有非法指令（如 $a$ 与 $b$ 在同一堆），无需处理。

输出:所有操作输入完毕后，从左到右，从下到上输出每个位置的木块编号。

感谢 [jxdql2001](https://www.luogu.com.cn/user/27114) 提供的翻译。

## 题目描述

[problemUrl]: https://uva.onlinejudge.org/index.php?option=com_onlinejudge&Itemid=8&category=3&page=show_problem&problem=37

[PDF](https://uva.onlinejudge.org/external/1/p101.pdf)

![](https://alphaguo-1322521250.cos.ap-beijing.myqcloud.com/202401161902572.png)

## 输入格式

![](https://alphaguo-1322521250.cos.ap-beijing.myqcloud.com/202401161902484.png)

## 输出格式

![](https://alphaguo-1322521250.cos.ap-beijing.myqcloud.com/202401161902463.png)

## 样例 #1

### 样例输入 #1

```
10
move 9 onto 1
move 8 over 1
move 7 over 1
move 6 over 1
pile 8 over 6
pile 8 over 5
move 2 over 1
move 4 over 9
quit
```

### 样例输出 #1

```
0: 0
1: 1 9 2 4
2:
3: 3
4:
5: 5 8 7 6
6:
7:
8:
9:
```

​		**先来分析：**

​		这个题意不是一针见血的，应该把`样例输出`横着看，`:`左边的`1234567890`表示的是9个位置，`:`右边的是每个位置上的木块。

# 5、移除元素

给你一个数组 `nums` 和一个值 `val`，你需要 **[原地](https://baike.baidu.com/item/原地算法)** 移除所有数值等于 `val` 的元素，并返回移除后数组的新长度。

不要使用额外的数组空间，你必须仅使用 `O(1)` 额外空间并 **[原地 ](https://baike.baidu.com/item/原地算法)修改输入数组**。

元素的顺序可以改变。你不需要考虑数组中超出新长度后面的元素。

 

**说明:**

为什么返回数值是整数，但输出的答案是数组呢?

请注意，输入数组是以**「引用」**方式传递的，这意味着在函数里修改输入数组对于调用者是可见的。

你可以想象内部操作如下:

```cpp
// nums 是以“引用”方式传递的。也就是说，不对实参作任何拷贝
int len = removeElement(nums, val);

// 在函数里修改输入数组对于调用者是可见的。
// 根据你的函数返回的长度, 它会打印出数组中 该长度范围内 的所有元素。
for (int i = 0; i < len; i++) {
    print(nums[i]);
}
```

 

**示例 1：**

```
输入：nums = [3,2,2,3], val = 3
输出：2, nums = [2,2]
解释：函数应该返回新的长度 2, 并且 nums 中的前两个元素均为 2。你不需要考虑数组中超出新长度后面的元素。例如，函数返回的新长度为 2 ，而 nums = [2,2,3,3] 或 nums = [2,2,0,0]，也会被视作正确答案。
```

**示例 2：**

```
输入：nums = [0,1,2,2,3,0,4,2], val = 2
输出：5, nums = [0,1,3,0,4]
解释：函数应该返回新的长度 5, 并且 nums 中的前五个元素为 0, 1, 3, 0, 4。注意这五个元素可为任意顺序。你不需要考虑数组中超出新长度后面的元素。
```

 

**提示：**

- `0 <= nums.length <= 100`
- `0 <= nums[i] <= 50`
- `0 <= val <= 100`

```c
int removeElement(int* nums, int numsSize, int val) {
    int *p=nums;//p找等于val的元素
    int *q=nums+numsSize-1;//q找不等于val的元素

    while(p<=q){
        if(*q==val){
            q--;
            continue;
        }
        if(*p==val){
            *p=*q;
            q--;
        }else{
            p++;
        }
    }
    return p-nums;
}
```

```cpp
class Solution {
	public:
		int removeElement(vector<int>& nums, int val) {
			int j=nums.size()-1;
			for(int i=0; i<=j; i++) {
				if(nums[i]==val) {
					swap(nums[i--],nums[j--]);
				}
			}
			return j+1;
		}
};
```



# 6、旋转链表

给你一个链表的头节点 `head` ，旋转链表，将链表每个节点向右移动 `k` 个位置。

 

**示例 1：**

![img](https://alphaguo-1322521250.cos.ap-beijing.myqcloud.com/202401311347494.jpeg)

```
输入：head = [1,2,3,4,5], k = 2
输出：[4,5,1,2,3]
```

**示例 2：**

![img](https://alphaguo-1322521250.cos.ap-beijing.myqcloud.com/202401311347478.jpeg)

```
输入：head = [0,1,2], k = 4
输出：[2,0,1]
```

 

**提示：**

- 链表中节点的数目在范围 `[0, 500]` 内
- `-100 <= Node.val <= 100`
- `0 <= k <= 2 * 109`

```cpp
#define _CRT_SECURE_NO_WARNINGS
#include <iostream>
using namespace std;
#include <string>



struct ListNode {
	int val;
	ListNode *next;
	ListNode() : val(0), next(nullptr) {}
	ListNode(int x) : val(x), next(nullptr) {}
	ListNode(int x, ListNode *next) : val(x), next(next) {}
};

class Solution {
	public:
		ListNode* rotateRight(ListNode* head, int k) {
			ListNode *p=head;
			ListNode *q=head;
			
			if(head==nullptr || k==0){
				return head;
			}

			int length=0;
			for(ListNode *i=head; i!=nullptr; i=i->next) {
				length++;
			}
			k=k%length;
			//把p移动k个节点 
			for(int i=0; i<k; i++) {
				p=p->next;
			}

			while(p->next!=nullptr) {
				//把p移动到原链表的尾节点
				//此时q指向的是新链表的尾节点
				q=q->next;
				p=p->next;
			}
			//把原链表的尾节点的next指向原链表的头节点
			p->next=head;
			//设置新链表的头节点
			head=q->next;
			//q指向的是新链表的尾节点，next置空
			q->next=nullptr;
			return head;
		}
};

int main() {

	return 0;
}


```

 

# 7、[ 逆波兰表达式求值](https://leetcode.cn/problems/evaluate-reverse-polish-notation/)

给你一个字符串数组 `tokens` ，表示一个根据 [逆波兰表示法](https://baike.baidu.com/item/逆波兰式/128437) 表示的算术表达式。

请你计算该表达式。返回一个表示表达式值的整数。

**注意：**

- 有效的算符为 `'+'`、`'-'`、`'*'` 和 `'/'` 。
- 每个操作数（运算对象）都可以是一个整数或者另一个表达式。
- 两个整数之间的除法总是 **向零截断** 。
- 表达式中不含除零运算。
- 输入是一个根据逆波兰表示法表示的算术表达式。
- 答案及所有中间计算结果可以用 **32 位** 整数表示。

 

**示例 1：**

```
输入：tokens = ["2","1","+","3","*"]
输出：9
解释：该算式转化为常见的中缀算术表达式为：((2 + 1) * 3) = 9
```

**示例 2：**

```
输入：tokens = ["4","13","5","/","+"]
输出：6
解释：该算式转化为常见的中缀算术表达式为：(4 + (13 / 5)) = 6
```

**示例 3：**

```
输入：tokens = ["10","6","9","3","+","-11","*","/","*","17","+","5","+"]
输出：22
解释：该算式转化为常见的中缀算术表达式为：
  ((10 * (6 / ((9 + 3) * -11))) + 17) + 5
= ((10 * (6 / (12 * -11))) + 17) + 5
= ((10 * (6 / -132)) + 17) + 5
= ((10 * 0) + 17) + 5
= (0 + 17) + 5
= 17 + 5
= 22
```

 

**提示：**

- `1 <= tokens.length <= 104`
- `tokens[i]` 是一个算符（`"+"`、`"-"`、`"*"` 或 `"/"`），或是在范围 `[-200, 200]` 内的一个整数

 

**逆波兰表达式：**

逆波兰表达式是一种后缀表达式，所谓后缀就是指算符写在后面。

- 平常使用的算式则是一种中缀表达式，如 `( 1 + 2 ) * ( 3 + 4 )` 。
- 该算式的逆波兰表达式写法为 `( ( 1 2 + ) ( 3 4 + ) * )` 。

逆波兰表达式主要有以下两个优点：

- 去掉括号后表达式无歧义，上式即便写成 `1 2 + 3 4 + * `也可以依据次序计算出正确结果。
- 适合用栈操作运算：遇到数字则入栈；遇到算符则取出栈顶两个数字进行计算，并将结果压入栈中



# 8、矩阵链乘 Matrix Chain Multiplication

## 题面翻译

## 矩阵链乘

### 题目描述

  假设你必须评估一种表达形如 A*B*C*D*E，其中 A,B,C,D,E是矩阵。既然矩阵乘法是关联的，那么乘法的顺序是任意的。然而，链乘的元素数量必须由你选择的赋值顺序决定。

  例如，A，B，C分别是 50 * 10 ，10 * 20 和 20 * 5 的矩阵。现在有两种方案计算 A * B * C ,即（A * B) * C 和 A*(B * C)。  
   第一个要进行15000次基本乘法，而第二个只进行3500次。

  你的任务就是写出一个程序判定以给定的方式相乘需要多少次基本乘法计算。

### 输入格式

  输入包含两个部分：矩阵和表达式。 
   输入文件的第一行包含了一个整数 n(1  $\leq$  n  $\leq$  26), 代表矩阵的个数。接下来的n行每一行都包含了一个大写字母，说明矩阵的名称，以及两个整数，说明行与列的个数。  
   第二个部分严格遵守以下的语法：

SecondPart = Line {  Line  } <EOF>
Line       = Expression <CR>
Expression = Matrix | "(" Expression Expression ")"
Matrix     = "A" | "B" | "C" | ... | "X" | "Y" | "Z"

###输出格式

  对于每一个表达式，如果乘法无法进行就输出 " error "。否则就输出一行包含计算所需的乘法次数。 

感谢@onceagain 提供翻译

## 题目描述

[problemUrl]: https://uva.onlinejudge.org/index.php?option=com_onlinejudge&Itemid=8&category=6&page=show_problem&problem=383

[PDF](https://uva.onlinejudge.org/external/4/p442.pdf)

![](https://alphaguo-1322521250.cos.ap-beijing.myqcloud.com/202404211732436.png)

## 输入格式

![](https://alphaguo-1322521250.cos.ap-beijing.myqcloud.com/202404211732454.png)

## 输出格式

![](https://alphaguo-1322521250.cos.ap-beijing.myqcloud.com/202404211732451.png)

## 样例 #1

### 样例输入 #1

```
9
A 50 10
B 10 20
C 20 5
D 30 35
E 35 15
F 15 5
G 5 10
H 10 20
I 20 25
A
B
C
(AA)
(AB)
(AC)
(A(BC))
((AB)C)
(((((DE)F)G)H)I)
(D(E(F(G(HI)))))
((D(EF))((GH)I))
```

### 样例输出 #1

```
0
0
0
error
10000
error
3500
15000
40500
47500
15125
```

```cpp
#define _CRT_SECURE_NO_WARNINGS // 禁用安全警告

#include <bits/stdc++.h> // 包含通用的标准库头文件
using namespace std;

const int maxSize = 26 + 5; // 定义最大尺寸为31
struct Matrix { // 定义矩阵结构体
    int a, b; // 矩阵的行和列
    Matrix(int a = 0, int b = 0) // 构造函数，初始化矩阵的行和列
        : a(a), b(b)
    {}
} m[maxSize]; // 创建Matrix类型的数组m

stack<Matrix> s; // 声明一个存放Matrix类型的栈s

int main01()
{
    int n; // 声明整数变量n
    char c; // 声明字符变量c
    string str; // 声明字符串变量str
    cin >> n; // 从标准输入流读取一个整数n
    for (int i = 0; i < n; i++) { // 循环读取n次
        cin >> c; // 从标准输入流读取一个字符c
        int k = c - 'A'; // 如果输入的是字母，则转换为整数
        cin >> m[k].a >> m[k].b; // 从标准输入流读取两个整数，分别赋值给m[k].a和m[k].b
    }
    while (cin >> str) { // 循环读取字符串str，直到读取失败
        int len = str.length(); // 获取字符串str的长度
        bool error = false; // 声明布尔变量error，并初始化为false
        int ans = 0; // 声明整数变量ans，并初始化为0
        for (int i = 0; i < len; i++) { // 循环遍历字符串str
            if (isalpha(str[i])) { // 如果str[i]是字母
                s.push(m[str[i] - 'A']); // 将m[str[i]-'A']入栈
            }
            else if (str[i] == ')') { // 如果str[i]是右括号
                Matrix m2 = s.top(); // 栈顶元素出栈，并赋值给m2
                s.pop(); // 弹出栈顶元素
                Matrix m1 = s.top(); // 再次将栈顶元素出栈，并赋值给m1
                s.pop(); // 弹出栈顶元素
                if (m1.b != m2.a) { // 如果m1的列数不等于m2的行数
                    error = true; // 将error标记为true
                    break; // 退出循环
                }
                ans += m1.a * m1.b * m2.b; // 计算矩阵相乘的结果，并累加到ans上
                s.push(Matrix(m1.a, m2.b)); // 将新的矩阵入栈，行数为m1的行数，列数为m2的列数
            }
        }
        if (error) { // 如果error为true
            cout << "error" << endl; // 输出错误信息
        }
        else { // 否则
            cout << ans << endl; // 输出计算结果
        }
    }
    system("pause"); // 暂停系统，等待用户按任意键继续
    return 0; // 返回0，表示程序正常结束
}

```

# 9、打印队列 Printer Queue

## 题面翻译

学生会里只有一台打印机，但是有很多文件需要打印，因此打印任务不可避免地需要等待。有些打印任务比较急，有些不那么急，所以每个任务都有一个1～9间的优先级，优先级越高表示任务越急。

打印机的运作方式如下：首先从打印队列里取出一个任务J，如果队列里有比J更急的任务，则直接把J放到打印队列尾部，否则打印任务J（此时不会把它放回打印队列）。
输入打印队列中各个任务的优先级以及所关注的任务在队列中的位置（队首位置为0），输出该任务完成的时刻。所有任务都需要1分钟打印。例如，打印队列为{1,1,9,1,1,1}，目前处于队首的任务最终完成时刻为5。

输入T
接下来T组数据
每组数据输入N，TOP。接下来N个数，TOP代表队列首

Translated by @HuangBo

## 题目描述

[problemUrl]: https://uva.onlinejudge.org/index.php?option=com_onlinejudge&Itemid=8&category=243&page=show_problem&problem=3252

[PDF](https://uva.onlinejudge.org/external/121/p12100.pdf)

![](https://alphaguo-1322521250.cos.ap-beijing.myqcloud.com/202404211821803.png)

## 输入格式

![](https://alphaguo-1322521250.cos.ap-beijing.myqcloud.com/202404211821763.png)

## 输出格式

![](https://alphaguo-1322521250.cos.ap-beijing.myqcloud.com/202404211821831.png)

## 样例 #1

### 样例输入 #1

```
3  //3组数据
1 0  //共1个任务，你的任务下标为0
5    //你的任务是5
4 2  //共4个任务，你的任务下标为2
1 2 3 4  //你的任务是3
6 0    //共6个任务，你的任务下标为0
1 1 9 1 1 1  //你的任务是1
```

### 样例输出 #1

```
1
2
5
```

```cpp

#define _CRT_SECURE_NO_WARNINGS
#include <bits/stdc++.h>
using namespace std;
int main()
{
    int T, n, m;//T:几组优先级，n:本组优先级个数，m:你的优先级下标
    cin >> T;
    for (int i = 0; i < T; i++) {
        queue<int> q;//记录每个优先级的下标
        vector<int> a, b;//a:原优先级排序,b:降序后的优先级排序
        int k = 0, x;//x:输入所有的优先级
        cin >> n >> m;
        for (int j = 0; j < n; j++) {
            cin >> x;
            a.push_back(x);
            b.push_back(x);
            q.push(j);
        }
        sort(b. begin(), b.end(), greater<int>());//降序
        int w = 0;
        int max = 0;
        while (!q.empty()) {
            max = b[w];
            int t = q.front();
            if (a[t] < max) {
                q.pop();
                q.push(t);
            }
            else {
                if (t == m) {
                    cout << ++k << endl;
                    break;
                }
                else {
                    q.pop();
                    k++;
                    w++;
                }
            }
        }
    }
    system("pause");
    return 0;
}

```

# 10、第 k 小整数

## 题目描述

现有 $n$ 个正整数，要求出这 $n$ 个正整数中的第 $k$ 个最小整数（相同大小的整数只计算一次）。

## 输入格式

第一行为 $n$ 和 $k$; 第二行开始为 $n$ 个正整数的值，整数间用空格隔开。

## 输出格式

第$k$个最小整数的值；若无解，则输出 `NO RESULT`。

## 样例 #1

### 样例输入 #1

```
10 3
1 3 3 7 2 5 1 2 4 6
```

### 样例输出 #1

```
3
```

## 提示

$n \leq 10000$，$k \leq 1000$，正整数均小于 $30000$。

```cpp
#define _CRT_SECURE_NO_WARNINGS // 禁用安全警告

#include <bits/stdc++.h> // 包含通用的标准库头文件
using namespace std;

int n, k, a[10005]; // 声明整数变量n、k，以及整数数组a，数组长度为10005

int main()
{
    cin >> n >> k; // 从标准输入流读取两个整数，分别赋值给n和k
    for (int i = 0; i < n; i++) { // 循环读取n次
        cin >> a[i]; // 从标准输入流读取一个整数，并存储到数组a中的第i个位置
    }
    sort(a, a + n); // 对数组a进行升序排序

    // unique函数返回值为指向不重复序列的结尾的迭代器，将不重复序列的长度存储到n1中
    int n1 = unique(a, a + n) - a;//表示去重后的元素个数 

    if (k <= n1) { // 如果k小于等于不重复序列的长度n1
        cout << a[k - 1]; // 输出第k小的元素
    }
    else { 
        cout << "NO RESULT"; 
    }
    system("pause"); 
    return 0; 
}
```



# 11、[AHOI2001] 彩票摇奖

## 题目描述

为了丰富人民群众的生活、支持某些社会公益事业，北塔市设置了一项彩票。该彩票的规则是：

1. 每张彩票上印有 $7$ 个各不相同的号码，且这些号码的取值范围为 $1\sim33$。
2. 每次在兑奖前都会公布一个由七个各不相同的号码构成的中奖号码。
3. 共设置 $7$ 个奖项，特等奖和一等奖至六等奖。

兑奖规则如下：
- 特等奖：要求彩票上 $7$ 个号码都出现在中奖号码中。
- 一等奖：要求彩票上有 $6$ 个号码出现在中奖号码中。
- 二等奖：要求彩票上有 $5$ 个号码出现在中奖号码中。
- 三等奖：要求彩票上有 $4$ 个号码出现在中奖号码中。
- 四等奖：要求彩票上有 $3$ 个号码出现在中奖号码中。
- 五等奖：要求彩票上有 $2$ 个号码出现在中奖号码中。
- 六等奖：要求彩票上有 $1$ 个号码出现在中奖号码中。

注：兑奖时并不考虑彩票上的号码和中奖号码中的各个号码出现的位置。例如，中奖号码为 $23\ 31\ 1\ 14\ 19\ 17\ 18$，则彩票 $12\ 8\ 9\ 23\ 1\ 16\ 7$ 由于其中有两个号码（$23$ 和 $1$）出现在中奖号码中，所以该彩票中了五等奖。

现已知中奖号码和小明买的若干张彩票的号码，请你写一个程序帮助小明判断他买的彩票的中奖情况。

## 输入格式

输入的第一行只有一个自然数 $n$，表示小明买的彩票张数；

第二行存放了 $7$ 个介于 $1$ 和 $33$ 之间的自然数，表示中奖号码；

在随后的 $n$ 行中每行都有 $7$ 个介于 $1$ 和 $33$ 之间的自然数，分别表示小明所买的 $n$ 张彩票。

## 输出格式

依次输出小明所买的彩票的中奖情况（中奖的张数），首先输出特等奖的中奖张数，然后依次输出一等奖至六等奖的中奖张数。

## 样例 #1

### 样例输入 #1

```
2
23 31 1 14 19 17 18
12 8 9 23 1 16 7
11 7 10 21 2 9 31
```

### 样例输出 #1

```
0 0 0 0 0 1 1
```

## 提示

#### 数据规模与约定

对于 $100\%$ 的数据，保证 $1 \leq n\lt1000$。

```cpp
#define _CRT_SECURE_NO_WARNINGS
#include <iostream>
using namespace std;
#include <string>
int f[35],p[10];//f[35]为判断每个数字是否为中奖号码的数组，若1为中奖号码，则f[1]=1;若2不为中奖号码，则f[2]=0
int main() {
	int n,m,count=0;
	cin>>n;//n为彩票张数
	for(int i=0; i<7; i++) { //输入7个中奖号码
		cin>>m;//m为中奖号码
		f[m]=1;//将中奖号码的值域设为1
	}
	for(int i=0; i<n; i++) { //小明购买了n张彩票
		count=0;//count为每张彩票中奖号码的个数 
		for(int j=0; j<7; j++) {
			cin>>m;//小明购买的号码
			if(f[m]) {
				count++;//如果f[m]=1，则表明此号码为中奖号码,count++
			}
		}
		p[count]++;//中奖号码为count个的彩票张数 
	}
	for(int i=7;i>1;i--){
		cout<<p[i]<<" ";
	}
	cout<<p[1];
	return 0;
}
```


