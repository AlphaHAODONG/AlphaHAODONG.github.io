# 数据类型


### 数据类型

#### 1.1、数据类型的基本概念

- 类型是对数据的抽象;
- 类型相同的数据具有相同的表示形式、存储格式以及相关操作;
- 程序中所有的数据都必定属于某种数据类型;
- 数据类型可以理解为创建变量的模具: ***\*固定大小内存的别名\****;

![img](https://alphaguo-1322521250.cos.ap-beijing.myqcloud.com/202312312334465.jpeg)

#### 1.2、数据类型别名

```c
typedef unsigned int u32;
typedef struct _PERSON{
	char name[64];
	int age;
}Person;

void test(){
	u32 val; //相当于 unsigned int val;
	Person person; //相当于 struct PERSON person;
}
```

#### 1.3、void数据类型

void字面意思是”无类型”,void* 无类型指针，无类型指针可以指向任何类型的数据。

void定义变量是没有任何意义的，当你定义void a，编译器会报错。

void真正用在以下两个方面：

-  对函数返回的限定；
-  Ø对函数参数的限定；



```c
//1. void修饰函数参数和函数返回
void test01(void){
	printf("hello world");
}

//2. 不能定义void类型变量
void test02(){
	void val; //报错
}

//3. void* 可以指向任何类型的数据，被称为万能指针
void test03(){
	int a = 10;
	void* p = NULL;
	p = &a;
	printf("a:%d\n",*(int*)p);
	
	char c = 'a';
	p = &c;
	printf("c:%c\n",*(char*)p);
}

//4. void* 常用于数据类型的封装
void test04(){
	//void * memcpy(void * _Dst, const void * _Src, size_t _Size);
}
```

#### 1.4、 sizeof操作符

`sizeof`是c语言中的一个操作符，类似于`++`、`--`等等。`sizeof`能够告诉我们编译器为某一特定数据或者某一个类型的数据在内存中分配空间时分配的大小，大小以字节为单位。

基本语法：

`sizeof(变量);`

`sizeof 变量；`

`sizeof(类型);`

***\*sizeof 注意点：\****

 `sizeof`返回的占用空间大小是为这个变量开辟的大小，而不只是它用到的空间。和现今住房的建筑面积和实用面积的概念差不多。所以对结构体用的时候，大多情况下就得考虑字节对齐的问题了；

 `sizeof`返回的数据结果类型是`unsigned int`；

 要注意数组名和指针变量的区别。通常情况下，我们总觉得数组名和指针变量差不多，但是在用`sizeof`的时候差别很大，对数组名用`sizeof`返回的是整个数组的大小，而对指针变量进行操作的时候返回的则是指针变量本身所占得空间，在32位机的条件下一般都是4。而且当数组名作为函数参数时，在函数内部，形参也就是个指针，所以不再返回数组的大小；

```c
//1. sizeof基本用法
void test01(){
	int a = 10;
	printf("len:%d\n", sizeof(a));
	printf("len:%d\n", sizeof(int));
	printf("len:%d\n", sizeof a);
}

//2. sizeof 结果类型
void test02(){
	unsigned int a = 10;
	if (a - 11 < 0){
		printf("结果小于0\n");
	}
	else{
		printf("结果大于0\n");
	}
	int b = 5;
	if (sizeof(b) - 10 < 0){
		printf("结果小于0\n");
	}
	else{
		printf("结果大于0\n");
	}
}

//3. sizeof 碰到数组
void TestArray(int arr[]){
	printf("TestArray arr size:%d\n",sizeof(arr));
}
void test03(){
	int arr[] = { 10, 20, 30, 40, 50 };
	printf("array size: %d\n",sizeof(arr));

	//数组名在某些情况下等价于指针
	int* pArr = arr;
	printf("arr[2]:%d\n",pArr[2]);
	printf("array size: %d\n", sizeof(pArr));

	//数组做函数函数参数，将退化为指针,在函数内部不再返回数组大小
	TestArray(arr);
}
```

#### 总结

l 数据类型本质是固定内存大小的别名，是个模具，C语言规定：通过数据类型定义变量；

l 数据类型大小计算（`sizeof`）；

l 可以给已存在的数据类型起别名`typedef`；

l 数据类型的封装（`void` 万能类型）；
