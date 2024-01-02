# 多维数组


### 1、一维数组

- 元素类型角度：数组是相同类型的变量的有序集合
- 内存角度：连续的一大片内存空间

![img](https://alphaguo-1322521250.cos.ap-beijing.myqcloud.com/202312312338346.jpeg)

#### 1.1 数组名

考虑下面这些声明：

```c
int a;
int b[10];
```

我们把a称作标量，因为它是个单一的值，这个变量值的类型是一个整数。我们把b称作数组，因为它是一些值的集合。下标和数组名一起使用，用于标识该集合中某个特定的值。例如，b[0]表示数组b的第1个值，b[4]表示第5个值。每个值都是一个特定的标量。
	那么问题是b的类型是什么？它所表示的又是什么？一个合乎逻辑的答案是它表示整个数组，但事实并非如此。在C中，在几乎所有数组名的表达式中，数组名的值是一个**指针常量**，也就是数组第一个元素的地址。它的类型取决于数组元素的类型：如果他们是int类型，那么数组名的类型就是“指向int的常量指针”；如果它们是其他类型，那么数组名的类型也就是“指向**其他类型**的常量指针”。

*请问：指针和数组是等价的吗？*

答案是**否定**的。数组名在表达式中使用的时候，编译器才会产生一个指针常量。那么数组在什么情况下不能作为指针常量呢？在以下两种场景下：

当数组名作为sizeof操作符的操作数的时候，此时sizeof返回的是整个数组的长度，而不是指针数组指针的长度。
当数组名作为`&`操作符的操作数的时候，此时返回的是一个指向数组的指针，而不是指向某个数组元素的指针常量。

```c
int arr[10];
//arr = NULL; //arr作为指针常量，不可修改
int *p = arr; //此时arr作为指针常量来使用
printf("sizeof(arr):%d\n", sizeof(arr)); //此时sizeof结果为整个数组的长度
printf("&arr type is %s\n", typeid(&arr).name()); //int(*)[10]而不是int*
```

#### 1.2 下标引用

```c
int arr[] = { 1, 2, 3, 4, 5, 6 };
```

***(arr + 3)** ,这个表达式是什么意思呢？

​	首先，我们说数组在表达式中是一个指向整型的指针，所以此表达式表示arr指针向后移动了3个元素的长度。然后通过间接访问操作符从这个新地址开始获取这个位置的值。这个和下标的引用的执行过程完全相同。所以如下表达式是等同的：

```c
*(arr + 3)
arr[3]
```

```c
int arr[] = { 5, 3, 6, 8, 2, 9 };
int *p = arr + 2;
printf("*p = %d\n", *p);//6
printf("*p = %d\n", p[-1]);//3
```

#### 1.3 数组和指针

指针和数组并不是相等的。为了说明这个概念，请考虑下面两个声明：

```c
int a[10];
int *b;
```

​		声明一个数组时，编译器根据声明所指定的元素数量为数组分配内存空间，然后再创建数组名，指向这段空间的起始位置。声明一个指针变量的时候，编译器只为指针本身分配内存空间，并不为任何整型值分配内存空间，指针并未初始化指向任何现有的内存空间。

​		因此，表达式`*a`是完全合法的，但是表达式`*b`却是非法的。`*b`将访问内存中一个不确定的位置，将会导致程序终止。另一方面b++可以通过编译，a++却不行，因为a是一个常量值。

#### 1.4 作为函数参数的数组名

当一个数组名作为一个参数传递给一个函数的时候发生什么情况呢？我们现在知道数组名其实就是一个指向数组第1个元素的指针，所以很明白此时传递给函数的是一份指针的拷贝。所以函数的形参实际上是一个指针。但是为了使程序员新手容易上手一些，编译器也接受数组形式的函数形参。因此下面两种函数原型是相等的：

```c
int print_array(int *arr);
int print_array(int arr[]);
```

​		我们可以使用任何一种声明，但哪一个更准确一些呢？答案是指针。因为实参实际上是个指针，而不是数组。同样sizeof arr值是指针的长度，而不是数组的长度。
​		现在我们清楚了，为什么一维数组中无须写明它的元素数目了，因为形参只是一个指针，并不需要为数组参数分配内存。另一方面，这种方式使得函数无法知道数组的长度。如果函数需要知道数组的长度，它必须显式传递一个长度参数给函数。

### 2、多维数组

​		如果某个数组的维数不止1个，它就被称为多维数组。接下来的案例讲解以二维数组举例。

```c
void test01(){
	//二维数组初始化
	int arr1[3][3] = {
		{ 1, 2, 3 },
		{ 4, 5, 6 },
		{ 7, 8, 9 }
	};
	int arr2[3][3] = { 1, 2, 3, 4, 5, 6, 7, 8, 9 };
	int arr3[][3] = { 1, 2, 3, 4, 5, 6, 7, 8, 9 };

	//打印二维数组
	for (int i = 0; i < 3; i++){
		for (int j = 0; j < 3; j ++){
			printf("%d ",arr1[i][j]);
		}
		printf("\n");
	}
}
```

#### 2.1 数组名

​		一维数组名的值是一个指针常量，它的类型是“指向元素类型的指针”，它指向数组的第1个元素。多维数组也是同理，多维数组的数组名也是指向第一个元素，只不过第一个元素是一个数组。例如：

```c
int arr[3][10]
```

​		可以理解为这是一个一维数组，包含了3个元素，只是每个元素恰好是包含了10个元素的数组。`arr`就表示指向它的第1个元素的指针，所以`arr`是一个指向了包含了10个整型元素的数组的指针。

#### 2.2 指向数组的指针(数组指针)

 		数组指针，它是指针，指向数组的指针。
		数组的类型由元素类型和数组大小共同决定：`int array[5]`  的类型为  int[5]；C语言可通过`typedef`定义一个数组类型：
		定义数组指针有一下三种方式：

```c
//方式一
void test01(){

	//先定义数组类型，再用数组类型定义数组指针
	int arr[10] = {1,2,3,4,5,6,7,8,9,10};
	//有typedef是定义类型，没有则是定义变量,下面代码定义了一个数组类型ArrayType
	typedef int(ArrayType)[10];
	//int ArrayType[10]; //定义一个数组，数组名为ArrayType

	ArrayType myarr; //等价于 int myarr[10];
	ArrayType* pArr = &arr; //定义了一个数组指针pArr，并且指针指向数组arr
	for (int i = 0; i < 10;i++){
		printf("%d ",(*pArr)[i]);
	}
	printf("\n");
}

//方式二
void test02(){

	int arr[10];
	//定义数组指针类型
	typedef int(*ArrayType)[10];
	ArrayType pArr = &arr; //定义了一个数组指针pArr，并且指针指向数组arr
	for (int i = 0; i < 10; i++){
		(*pArr)[i] = i + 1;
	}
	for (int i = 0; i < 10; i++){
			printf("%d ", (*pArr)[i]);
	}
	printf("\n");

}

//方式三
void test03(){
	
	int arr[10];
	int(*pArr)[10] = &arr;

	for (int i = 0; i < 10; i++){
		(*pArr)[i] = i + 1;

	}
	for (int i = 0; i < 10; i++){
		printf("%d ", (*pArr)[i]);
	}
	printf("\n");
}
```

#### 2.3 指针数组(元素为指针)

##### 2.3.1 栈区指针数组

```c
//数组做函数函数，退化为指针
void array_sort(char** arr,int len){

	for (int i = 0; i < len; i++){
		for (int j = len - 1; j > i; j --){
			//比较两个字符串
			if (strcmp(arr[j-1],arr[j]) > 0){
				char* temp = arr[j - 1];
				arr[j - 1] = arr[j];
				arr[j] = temp;
			}
		}
	}

}
//打印数组
void array_print(char** arr,int len){
	for (int i = 0; i < len;i++){
		printf("%s\n",arr[i]);
	}
	printf("----------------------\n");
}

void test(){
	
	//主调函数分配内存
	//指针数组
	char* p[] = { "bbb", "aaa", "ccc", "eee", "ddd"};
	//char** p = { "aaa", "bbb", "ccc", "ddd", "eee" }; //错误
	int len = sizeof(p) / sizeof(char*);
	//打印数组
	array_print(p, len);
	//对字符串进行排序
	array_sort(p, len);
	//打印数组
	array_print(p, len);
}
```

##### 2.3.2 堆区指针数组

```c
//分配内存
char** allocate_memory(int n){
	
	if (n < 0 ){
		return NULL;
	}

	char** temp = (char**)malloc(sizeof(char*) * n);
	if (temp == NULL){
		return NULL;
	}

	//分别给每一个指针malloc分配内存
	for (int i = 0; i < n; i ++){
		temp[i] = malloc(sizeof(char)* 30);
		sprintf(temp[i], "%2d_hello world!", i + 1);
	}
	return temp;
}

//打印数组
void array_print(char** arr,int len){
	for (int i = 0; i < len;i++){
		printf("%s\n",arr[i]);
	}
	printf("----------------------\n");
}

//释放内存
void free_memory(char** buf,int len){
	if (buf == NULL){
		return;
	}
	for (int i = 0; i < len; i ++){
		free(buf[i]);
		buf[i] = NULL;
	}

	free(buf);
}

void test(){
	
	int n = 10;
	char** p = allocate_memory(n);
	//打印数组
	array_print(p, n);
	//释放内存
	free_memory(p, n);
}
```

#### 2.4 二维数组三种参数形式

##### 2.4.1 二维数组的线性存储特性

```c
void PrintArray(int* arr, int len){
	for (int i = 0; i < len; i++){
		printf("%d ", arr[i]);
	}
	printf("\n");
}

//二维数组的线性存储
void test(){
	int arr[][3] = {
		{ 1, 2, 3 },
		{ 4, 5, 6 },
		{ 7, 8, 9 }
	};

	int arr2[][3] = { 1, 2, 3, 4, 5, 6, 7, 8, 9 };
	int len = sizeof(arr2) / sizeof(int);

	//如何证明二维数组是线性的？
	//通过将数组首地址指针转成Int*类型，那么步长就变成了4，就可以遍历整个数组
	int* p = (int*)arr;
	for (int i = 0; i < len; i++){
		printf("%d ", p[i]);
	}
	printf("\n");

	PrintArray((int*)arr, len);
	PrintArray((int*)arr2, len);
}
```

##### 2.4.2 二维数组的3种形式参数

```c
//二维数组的第一种形式
void PrintArray01(int arr[3][3]){
	for (int i = 0; i < 3; i++){
		for (int j = 0; j < 3; j++){
			printf("arr[%d][%d]:%d\n", i, j, arr[i][j]);
		}
	}
}

//二维数组的第二种形式
void PrintArray02(int arr[][3]){
    	for (int i = 0; i < 3; i++){
		for (int j = 0; j < 3; j++){
			printf("arr[%d][%d]:%d\n", i, j, arr[i][j]);
		}
	}
}

//二维数组的第二种形式
void PrintArray03(int(*arr)[3]){
	for (int i = 0; i < 3; i++){
		for (int j = 0; j < 3; j++){
			printf("arr[%d][%d]:%d\n", i, j, arr[i][j]);
		}
	}
}

void test(){
	
	int arr[][3] = { 
		{ 1, 2, 3 },
		{ 4, 5, 6 },
		{ 7, 8, 9 }
	};
	
	PrintArray01(arr);
	PrintArray02(arr);
	PrintArray03(arr);
}
```


