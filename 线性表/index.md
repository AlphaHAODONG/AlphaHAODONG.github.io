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



 //插入数据
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

 //按照位置删除数据
 void removeByPos_dynamiArray(struct dynamiArray* arr, int pos)
 {
    if(arr==NULL)
    {
        return;
    }
    //无效的删除位置
    if(pos<0||pos>arr->m_Size-1)
    {
        return;
    }
    for (int i = pos; i < arr->m_Size; i++)
    {
        arr->p1[i] = arr->p1[i + 1];
    }
    arr->m_Size--;

 }

 int comparePerson(void *data1,void*data2)
 {
     struct Person* p1 = data1;
     struct Person* p2 = data2;
     return strcmp(p1->name, p2->name) == 0 && p1->age == p2->age;
 }
 //按照值删除
 void removeByValue_dynamiArray(struct dynamiArray* arr, void* data, int(*myCompare)(void*, void*))
 {
     if(arr==NULL)
     {
         return;
     }
     if(data==NULL)
     {
         return;
     }
     for (int i = 0; i < arr->m_Size; i++)
     {
         if (myCompare(arr->p1[i], data))
         {
             removeByPos_dynamiArray(arr, i);
             break;
         }
     }
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

 void destory_dynamiArray(struct dynamiArray* arr)
 {
     if (arr == NULL)
     {
         return;
     }
     if (arr->p1 != NULL)
     {
         free(arr->p1);
         arr->p1 = NULL;
     }
     free(arr);
     arr = NULL;

     return;
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
    /* 姓名: 张飞 年龄 : 20
     姓名 : 赵云 年龄 : 18
     姓名 : 关羽 年龄 : 19
     姓名 : 刘备 年龄 : 2*/

     printf("插入数据后，数组的容量为  %d\n", arr->m_Capacity);
     printf("插入数据后，数组的大小为  %d\n", arr->m_Size);

     printf("删除关羽\n");
     removeByPos_dynamiArray(arr, 2);
     foreach_dynamiArray(arr, printPerson);

     printf("删除刘备\n");
     struct Person p = { "刘备",21 };
     removeByValue_dynamiArray(arr, &p, comparePerson);
     foreach_dynamiArray(arr, printPerson);
 
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

### 3、线性表的链式存储(单向链表)

​		前面我们写的线性表的顺序存储(动态数组)的案例，最大的缺点是插入和删除时需要移动大量元素，这显然需要耗费时间，能不能想办法解决呢？链表。
​		链表为了表示每个数据元素与其直接后继元素之间的逻辑关系，每个元素除了存储本身的信息外，还需要存储指示其直接后继的信息。

![img](https://alphaguo-1322521250.cos.ap-beijing.myqcloud.com/202401031746442.jpg)

**单链表**

- 线性表的链式存储结构中，每个节点中只包含一个指针域，这样的链表叫单链表。
- 通过每个节点的指针域将线性表的数据元素按其逻辑次序链接在一起（如图）。

![img](https://alphaguo-1322521250.cos.ap-beijing.myqcloud.com/202401031747875.jpg)

**概念解释：**

- 表头结点

  链表中的第一个结点，包含指向第一个数据元素的指针以及链表自身的一些信息

- 数据节点

  链表中代表数据元素的结点，包含指向下一个数据元素的指针和数据元素的信息

- 尾节点

  链表中的最后一个数据结点，其下一元素指针为空，表示无后继。

![img](https://alphaguo-1322521250.cos.ap-beijing.myqcloud.com/202401031749936.jpg)

#### 3.1 线性表的链式存储(单项链表)的设计与实现

1. 插入操作![img](https://alphaguo-1322521250.cos.ap-beijing.myqcloud.com/202401031750094.jpg)

   ```c
   node->next = current->next;
   current->next = node;
   ```

   

2. 删除操作

![img](https://alphaguo-1322521250.cos.ap-beijing.myqcloud.com/202401031751651.jpg)

```c
current->next = ret->next;
```

```c
#define _CRT_SECURE_NO_WARNINGS
#include <stdio.h>
#include <stdlib.h>
#include <string.h>

//节点
struct LinkNode
{
    //数据域
    void* data;
    //指针域
    struct LinkNode* next;
};
struct Node
{
    char name[64];
    int age;
};
//链表
struct LList
{
    //头结点
    struct LinkNode pHeader;
    //链表长度
    int m_Size;
};
typedef void* LinkList;

//初始化链表
LinkList init_LinkList()
{
    struct LList* myList = malloc(sizeof(struct LList));
    if(myList==NULL)
    {
        return NULL;
    }
    myList->pHeader.data = NULL;
    myList->pHeader.next = NULL;
    myList->m_Size = 0;
    return myList;
}

//插入数据
void insert_LinkList(LinkList list, int pos, void* data)
{
    struct LList* myList = list;
    if (list == NULL || data == NULL)
    {
        return;
    }
    if (pos<0 || pos>myList->m_Size)
    {
        pos = myList->m_Size;
    }
    //创建一个临时节点,用来寻找新节点的前驱结点
    struct LinkNode* pCurrent = &myList->pHeader;
    for (int i = 0; i < pos; i++)
    {
        pCurrent = pCurrent->next;
    }//此时pCurrent节点就是新节点的前驱节点
    struct LinkNode* newNode = malloc(sizeof(struct LinkNode));
    newNode->data = data;
    newNode->next = NULL;

    newNode->next = pCurrent->next;
    pCurrent->next = newNode;

    //更新链表长度
    myList->m_Size++;



    return;
}

void printNode(void* data)
{
    struct Node* node = data;
    printf("姓名：%s  年龄：%d\n", node->name, node->age);
}
//遍历
void foreach_LinkList(LinkList list, void(*myPrint)(void*))
{
    if (list == NULL)
    {
        return;
    }
    struct LList* myList = list;
    struct LinkNode* pCurrent = myList->pHeader.next;
    while (pCurrent != NULL) {
        myPrint(pCurrent->data);
        pCurrent = pCurrent->next;
    }

}

//删除链表节点
void removeBuPos_LinkList(LinkList list, int pos)
{
    if (list == NULL)
    {
        return;
    }
    struct LList* mylist = list;
    if (pos<0 || pos>mylist->m_Size - 1)
    {
        return;
    }
    //找到待删除位置的前驱节点位置
    struct LinkNode* pCurrent = &mylist->pHeader;
    for (int i = 0; i < pos; i++)
    {
        pCurrent = pCurrent->next;
    }
    //此时pcurrent就是待删除节点的前驱节点位置
    struct LinkNode* pDel = pCurrent->next;//pdel就是要删除的节点
    pCurrent->next = pDel->next;

    free(pDel);
    pDel = NULL;

    //更新链表长度
    mylist->m_Size--;

}

//回调函数，删除对比
int CompareNode(void* data1, void* data2)
{
    struct Node* n1 = data1;
    struct Node* n2 = data2;
    return strcmp(n1->name, n2->name) == 0 && n1->age == n2->age;

}

//按值删除链表节点
void removeByValue_LinkList(LinkList list, void* data, int (*myCompare)(void*, void*))
{
    if (list == NULL)
    {
        return;
    }
    if (data == NULL)
    {
        return;
    }

    //将list还原成真实的链表结构体
    struct LList* mylist = list;

    //创建两个辅助指针变量
    struct LinkNode* pPrey = &mylist->pHeader;
    struct LinkNode* pCurrent = mylist->pHeader.next;

    //遍历链表，找用户要删除的数据
    for (int i = 0; i < mylist->m_Size; i++)
    {
        if (myCompare(data, pCurrent->data))
        {
            //找到要修改的数据了，更改指针指向
            pPrey->next = pCurrent->next;

            free(pCurrent);
            pCurrent = NULL;

            mylist->m_Size++;
            break;
        }
        pPrey = pCurrent;
        pCurrent = pCurrent->next;
    }

}

//清除链表
void clear_LinkList(LinkList list) {
    if (list == NULL) {
        return;
    }
    struct LList* myList = list;
    struct LinkNode* pCurrent = myList->pHeader.next;
    struct LinkNode* pNext = NULL;

    while (pCurrent != NULL) {
        pNext = pCurrent->next;
        free(pCurrent);
        pCurrent = pNext;
    }

    myList->pHeader.next = NULL;
    myList->m_Size = 0;
}


//销毁链表
void destory_LinkList(LinkList list)
{
    if (list == NULL)
    {
        return;
    }
    clear_LinkList(list);
    free(list);
    list = NULL;
}

void test02()
{
    LinkList myList = init_LinkList();
    struct Node p1 = { "赵云",18 };
    struct Node p2 = { "关羽",19 };
    struct Node p3 = { "张飞",20 };
    struct Node p4 = { "刘备",21 };
    struct Node p5 = { "黄忠",22 };
    struct Node p6 = { "曹操",23 };

    insert_LinkList(myList, 0, &p1);
    insert_LinkList(myList, -1, &p2);
    insert_LinkList(myList, 0, &p3);
    insert_LinkList(myList, -1, &p4);
    insert_LinkList(myList, 0, &p5);
    insert_LinkList(myList, -1, &p6);

    foreach_LinkList(myList, printNode);
        /*姓名：黄忠  年龄：22
        姓名：张飞  年龄：20
        姓名：赵云  年龄：18
        姓名：关羽  年龄：19
        姓名：刘备  年龄：21
        姓名：曹操  年龄：23*/
    printf("--------------------------------------------------\n");

    printf("按位置要删除刘备------------------------\n");
    removeBuPos_LinkList(myList, 4);
    foreach_LinkList(myList, printNode);

    printf("按值要删除黄忠------------------------\n");
    removeByValue_LinkList(myList, &p5, CompareNode);
    foreach_LinkList(myList, printNode);

    printf("清空链表-------------------------------\n");
    clear_LinkList(myList);
    foreach_LinkList(myList, printNode);

    printf("销毁链表------------------------------\n");
    destory_LinkList(myList);
    return;
}
int main()
{
    test02();
        
    system("pause");
    return 0;
}

```

#### 3.2 优点和缺点

1. 优点：

   无需一次性定制链表的容量 

   插入和删除操作无需移动数据元素

2. 缺点：

   数据元素必须保存后继元素的位置信息

   获取指定数据的元素操作需要顺序访问之前的元素


