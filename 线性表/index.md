# 线性表


### 1、线性表基本概念

​		线性结构是一种最简单且常用的数据结构。线性结构的基本特点是节点之间满足线性关系。本章讨论的动态数组、链表、栈、队列都属于线性结构。他们的共同之处，是节点中有且只有一个开始节点和终端节点。按这种关系，可以把它们的所有节点排列成一个线性序列。但是，他们分别属于几种不同的抽象数据类型实现，它们之间的区别，主要就是操作的不同.

线性表是零个或者多个数据元素的有限序列，***数据元素之间是有顺序的，数据元素个数是有限的，数据元素的类型必须相同***

线性表的性质：

1. a0 为线性表的第一个元素，只有一个后继。
2. an 为线性表的最后一个元素，只有一个前驱。
3. 除 a0 和 an 外的其它元素 ai，既有前驱，又有后继。
4. 线性表能够逐项访问和顺序存取。

线性表的抽象数据类型定义：

```c
ADT线性表（List）
Data
线性表的数据对象集合为{ a1, a2, ……, an }，每个元素的类型均为DataType。其中，除第一个元素a1外，每个元素有且只有一个直接前驱元素，除了最后一个元素an外，每个元素有且只有一个直接后继元素。数据元素之间的关系是一一对应的。

Operation（操作）
// 初始化，建立一个空的线性表L。
InitList(*L);
// 若线性表为空，返回true，否则返回false
ListEmpty(L);
// 将线性表清空
ClearList(*L);
// 将线性表L中的第i个位置的元素返回给e
GetElem(L, i, *e);
// 在线性表L中的第i个位置插入新元素e
ListInsert(*L, i, e);
// 删除线性表L中的第i个位置元素，并用e返回其值
ListDelete(*L, i, *e);
// 返回线性表L的元素个数
ListLength(L);
// 销毁线性表
DestroyList(*L);
```

### 2、线性表的顺序存储

​		采用顺序存储是表示线性表最简单的方法，具体做法是：将线性表中的元素一个接一个的存储在一块连续的存储区域中，这种顺序表示的线性表也成为顺序表。

#### 2.1 线性表顺序存储(动态数组)的设计与实现

**操作要点:**

- 插入元素算法
- 判断线性表是否合法
- 判断插入位置是否合法
- 判断空间是否满足
- 把最后一个元素到插入位置的元素后移一个位置
- 将新元素插入
- 线性表长度加1
- 获取元素操作
- 判断线性表是否合法
- 判断位置是否合法
- 直接通过数组下标的方式获取元素
- 删除元素算法
- 判断线性表是否合法
- 判断删除位置是否合法
- 将元素取出
- 将删除位置后的元素分别向前移动一个位置
- 线性表长度减1
- 元素的插入

![img](https://alphaguo-1322521250.cos.ap-beijing.myqcloud.com/202401021728780.jpg)

- 元素的删除

![img](https://alphaguo-1322521250.cos.ap-beijing.myqcloud.com/202401021730580.jpg)

***注意: 链表的容量和链表的长度是两个不同的概念***

```c

#define _CRT_SECURE_NO_WARNINGS
#include <stdio.h>
#include <stdlib.h>
#include <string.h>


struct Person 
{
    char name[64];
    int age;
};

//动态数组结构体
 struct dynamiArray
{
     //真实在堆区开辟数组的指针
     void** p1;
     //数组容量
     int m_Capacity;
     //数组长度
     int m_Size;
};
 //初始化数组
 struct dynamiArray * init_dynamicArray(int capacity)
 {
     if (capacity <= 0) {
         return NULL;
     }
     //给数组分配内存
     struct dynamiArray* arr = malloc(sizeof(struct dynamiArray));
     if (arr == NULL) {

         return NULL;
     }
     //给数组初始化
     arr->p1 = malloc(sizeof(void*) * capacity);
     arr->m_Capacity = capacity;
     arr->m_Size = 0;
     return arr;
 };



 //插入数组
 void insert_dynamiArray(struct dynamiArray* arr, void* data, int pos)
 {
     if (arr == NULL)
     {
         return;
     }
     if (data == NULL)
     {
         return;
     }
     if (pos<0 || pos>arr->m_Size)
     {
         //尾插
         pos = arr->m_Size;
     }
     //判断数组是否满了
     if (arr->m_Size == arr->m_Capacity)
     {
         //计算新内存空间大小
         int newCapacity = arr->m_Capacity * 2;

         //开辟新的空间
         void** newsPace = malloc(sizeof(void*) * newCapacity);

         //将原空间的数据拷贝到新空间下
         memcpy(newsPace, arr->p1, sizeof(void*)* arr->m_Capacity);

         //释放原空间
         free(arr->p1);

         //更改指针指向
         arr->p1 = newsPace;

         //更新新的容量
         arr->m_Capacity = newCapacity;
     }
     for (int i = arr->m_Size; i >pos; i--)
     {
         arr->p1[i] = arr->p1[i-1];
     }
     arr->p1[pos] = data;
     arr->m_Size++;

 }

 //遍历
 void foreach_dynamiArray(struct dynamiArray* arr,void(*myPrint)(void*))
 {
    if(arr==NULL)
     {
     return;
     }
    for (int i = 0; i < arr->m_Size; i++)
    {
     myPrint(arr->p1[i]);
    }
 }

 void printPerson(void* data)
 {
     struct Person* person = data;
     printf("姓名: %s 年龄: %d \n", person->name, person->age);
}
 void test01()
 {
     struct dynamiArray* arr = init_dynamicArray(3);
   
     struct Person p1 = { "赵云",18 };
     struct Person p2 = { "关羽",19 };
     struct Person p3 = { "张飞",20 };
     struct Person p4 = { "刘备",21 };

     printf("插入数据前，数组的容量为  %d\n", arr->m_Capacity);
     printf("插入数据前，数组的大小为  %d\n", arr->m_Size);

     insert_dynamiArray(arr, &p1, 0);
     insert_dynamiArray(arr, &p2, -1);
     insert_dynamiArray(arr, &p3, 0);
     insert_dynamiArray(arr, &p4, -1);

  
     foreach_dynamiArray(arr, printPerson);
     printf("插入数据后，数组的容量为  %d\n", arr->m_Capacity);
     printf("插入数据后，数组的大小为  %d\n", arr->m_Size);
 
 }
int main()
{
    test01();
    system("pause");
    return 0;
}

```

#### 2.2 优点和缺点

优点：

- 无需为线性表中的逻辑关系增加额外的空间。
- 可以快速的获取表中合法位置的元素。

缺点：

- 插入和删除操作需要移动大量元素。
