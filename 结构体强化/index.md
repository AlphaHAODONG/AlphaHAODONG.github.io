# 结构体强化


### 1、结构体基础知识

#### 1.1 结构体类型的定义

```c
struct Person{
	char name[64];
	int age;
};

typedef struct _PERSON{
	char name[64];
	int age;
}Person;
```

注意：定义结构体类型时不要直接给成员赋值，结构体只是一个类型，编译器还没有为其分配空间，只有根据其类型定义变量时，才分配空间，有空间后才能赋值。

#### 1.2 结构体变量的定义

```c
struct Person{
	char name[64];
	int age;
}p1; //定义类型同时定义变量

struct{
	char name[64];
	int age;
}p2; //定义类型同时定义变量


struct Person p3; //通过类型直接定义
```

#### 1.3 结构体变量的初始化

```c
struct Person{
	char name[64];
	int age;
}p1 = {"john",10}; //定义类型同时初始化变量

struct{
	char name[64];
	int age;
}p2 = {"Obama",30}; //定义类型同时初始化变量

struct Person p3 = {"Edward",33}; //通过类型直接定义
```

#### 1.4 结构体成员的使用

```c
struct Person{
	char name[64];
	int age;
};
void test(){
	//在栈上分配空间
	struct Person p1;
	strcpy(p1.name, "John");
	p1.age = 30;
	//如果是普通变量，通过点运算符操作结构体成员
	printf("Name:%s Age:%d\n", p1.name, p1.age);

	//在堆上分配空间
	struct Person* p2 = (struct Person*)malloc(sizeof(struct Person));
	strcpy(p2->name, "Obama");
	p2->age = 33;
    //如果是指针变量，通过->操作结构体成员
	printf("Name:%s Age:%d\n", p2->name, p2->age);
}
```

#### 1.5 结构体赋值

##### 1.5.1 赋值基本概念

​		相同的两个结构体变量可以相互赋值，把一个结构体变量的值拷贝给另一个结构体，这两个变量还是两个独立的变量。

```c
struct Person{
	char name[64];
	int age;
};

void test(){
	//在栈上分配空间
	struct Person p1 = { "John" , 30};
	struct Person p2 = { "Obama", 33 };
	printf("Name:%s Age:%d\n", p1.name, p1.age);
	printf("Name:%s Age:%d\n", p2.name, p2.age);
	//将p2的值赋值给p1
	p1 = p2;
	printf("Name:%s Age:%d\n", p1.name, p1.age);
	printf("Name:%s Age:%d\n", p2.name, p2.age);
}
```

##### 1.5.2 深拷贝和浅拷贝

```c
//一个老师有N个学生
typedef struct _TEACHER{
	char* name;
}Teacher;

void test(){
	Teacher t1;
    t1.name = malloc(64);
	strcpy(t1.name , "John");

	Teacher t2;
	t2 = t1;

	//对手动开辟的内存，需要手动拷贝
	t2.name = malloc(64);
	strcpy(t2.name, t1.name);

	if (t1.name != NULL){
		free(t1.name);
		t1.name = NULL;
	}
	if (t2.name != NULL){
		free(t2.name);
		t1.name = NULL;
	}
}
```

#### 1.6 结构体数组

```c
struct Person{
	char name[64];
	int age;
};

void test(){
	//在栈上分配空间
	struct Person p1[3] = {
		{ "John", 30 },
		{ "Obama", 33 },
		{ "Edward", 25}
	};

	struct Person p2[3] = { "John", 30, "Obama", 33, "Edward", 25 };
	for (int i = 0; i < 3;i ++){
		printf("Name:%s Age:%d\n",p1[i].name,p1[i].age);
	}
	printf("-----------------\n");
	for (int i = 0; i < 3; i++){
		printf("Name:%s Age:%d\n", p2[i].name, p2[i].age);
        	}
	printf("-----------------\n");
	//在堆上分配结构体数组
	struct Person* p3 = (struct Person*)malloc(sizeof(struct Person) * 3);
	for (int i = 0; i < 3;i++){
		sprintf(p3[i].name, "Name_%d", i + 1);
		p3[i].age = 20 + i;
	}
	for (int i = 0; i < 3; i++){
		printf("Name:%s Age:%d\n", p3[i].name, p3[i].age);
	}
}
```

### 2、结构体嵌套指针

#### 2.1 结构体嵌套一级指针

```c
struct Person{
	char* name;
	int age;
};

void allocate_memory(struct Person** person){
	if (person == NULL){
		return;
	}
	struct Person* temp = (struct Person*)malloc(sizeof(struct Person));
	if (temp == NULL){
		return;
	}
	//给name指针分配内存
	temp->name = (char*)malloc(sizeof(char)* 64);
	strcpy(temp->name, "John");
	temp->age = 100;

	*person = temp;
}

void print_person(struct Person* person){
	printf("Name:%s Age:%d\n",person->name,person->age);
}
void free_memory(struct Person** person){
	if (person == NULL){
		return;
	}
	struct Person* temp = *person;
	if (temp->name != NULL){
		free(temp->name);
		temp->name = NULL;
	}

	free(temp);
}

void test(){
	
	struct Person* p = NULL;
	allocate_memory(&p);
	print_person(p);
	free_memory(&p);
}
```

#### 2.2 结构体嵌套二级指针

```c
//一个老师有N个学生
typedef struct _TEACHER{
	char name[64];
	char** students;
}Teacher;

void create_teacher(Teacher** teacher,int n,int m){

	if (teacher == NULL){
		return;
	}

	//创建老师数组
	Teacher* teachers = (Teacher*)malloc(sizeof(Teacher)* n);
	if (teachers == NULL){
		return;
	}

	//给每一个老师分配学生
    	int num = 0;
	for (int i = 0; i < n; i ++){
		sprintf(teachers[i].name, "老师_%d", i + 1);
		teachers[i].students = (char**)malloc(sizeof(char*) * m);
		for (int j = 0; j < m;j++){
			teachers[i].students[j] = malloc(64);
			sprintf(teachers[i].students[j], "学生_%d", num + 1);
			num++;
		}
	}

	*teacher = teachers;	
}

void print_teacher(Teacher* teacher,int n,int m){
	for (int i = 0; i < n; i ++){
		printf("%s:\n", teacher[i].name);
		for (int j = 0; j < m;j++){
			printf("  %s",teacher[i].students[j]);
		}
		printf("\n");
	}
}

void free_memory(Teacher** teacher,int n,int m){
	if (teacher == NULL){
		return;
	}

	Teacher* temp = *teacher;

	for (int i = 0; i < n; i ++){
		
		for (int j = 0; j < m;j ++){
			free(temp[i].students[j]);
			temp[i].students[j] = NULL;
		}

		free(temp[i].students);
		temp[i].students = NULL;
	}

	free(temp);
    }

void test(){
	
	Teacher* p = NULL;
	create_teacher(&p,2,3);
	print_teacher(p, 2, 3);
	free_memory(&p,2,3);
}
```

### 3、结构体成员偏移量

```c
//一旦结构体定义下来，则结构体中的成员内存布局就定下了
#include <stddef.h>
struct Teacher
{
	char a;
	int b;
};

void test01(){

	struct Teacher  t1;
	struct Teacher*p = &t1;


	int offsize1 = (int)&(p->b) - (int)p;  //成员b 相对于结构体 Teacher的偏移量
	int offsize2 = offsetof(struct Teacher, b);

	printf("offsize1:%d \n", offsize1); //打印b属性对于首地址的偏移量
	printf("offsize2:%d \n", offsize2);
}
```

### 4、结构体字节对齐

​		在用`sizeof`运算符求某结构体所占空间时，并不是简单的将结构体中所有元素各自占的空间相加，这里涉及到内存字节对齐的问题。

​		从理论上讲，对于任何变量的访问都可以从任何地址开始访问，但是实际上访问特定类型的变量只能在特定的地址访问，这就需要各个变量在空间上按一定的规则排列，而不是简单的顺序排列，这就是**内存对齐**。

#### 4.1 内存对齐

##### 4.1.1 内存对齐原因

​		内存的最小单元是一个字节，当cpu从内存中读取数据的时候，是一个一个字节读取，所以内存对我们是如下图这样的：

![img](https://alphaguo-1322521250.cos.ap-beijing.myqcloud.com/202312312338987.png)

​		但实际上cpu将内存当成多个块，每次从内存中读取一个块，这个块的大小可能是2、4、8、16等，

​		内存对齐是操作系统为了提高访问内存的策略。操作系统在访问内存的时候，每次读取一定长度（这个长度是操作系统默认的对齐数，或者默认对齐数的整数倍）。如果没有对齐，为了访问一个变量可能产生二次访问。

​		*那么实现内存对齐的好处是：*

- 提高存取数据的速度。比如有的平台每次都是从偶地址处读取数据，对于一个int型的变量，若从偶地址单元处存放，则只需一个读取周期即可读取该变量；但是若从奇地址单元处存放，则需要两个读取周期读取该变量。

- 某些平台只能在特定的地质处访问特定类型的数据，否则抛出硬件异常给操作系统。

- 对于标准数据类型，它的地址只要是它的长度的整数倍。
- 对于非标准数据类型，比如结构体，要遵循一下对齐原则：

\1. 数组成员对齐规则。第一个数组成员应该放在offset为0的地方，以后每个数组成员应该放在offset为***min（当前成员的大小，#pargama pack(n)）***整数倍的地方开始（***比如int在32位机器为４字节，#pargama pack(2)***，那么从2的倍数地方开始存储）。

\2. 结构体总的大小，也就是sizeof的结果，必须是***min（结构体内部最大成员，#pargama pack(n)）***的整数倍，不足要补齐。

\3. 结构体做为成员的对齐规则。如果一个结构体B里嵌套另一个结构体A,还是以最大成员类型的大小对齐，但是结构体A的起点为A内部最大成员的整数倍的地方。（struct B里存有struct A，A里有char，int，double等成员，那A应该从8的整数倍开始存储。），结构体A中的成员的对齐规则仍满足原则1、原则2。



手动设置对齐模数:

- #pragma pack(show)
  显示当前packing alignment的字节数，以warning message的形式被显示。

- #pragma pack(n)
  指定packing的数值，以字节为单位，缺省数值是8，合法的数值分别是1,2,4,8,16。

#### 4.2 内存对齐案例

```c
#pragma pack(4)

typedef struct _STUDENT{
	int a;
	char b;
	double c;
	float d;
}Student;

typedef struct _STUDENT2{
	char a;
	Student b; 
	double c;
}Student2;

void test01(){

	//Student
	//a从偏移量0位置开始存储
	//b从4位置开始存储
	//c从8位置开始存储
	//d从12位置开存储
	//所以Student内部对齐之后的大小为20 ，整体对齐，整体为最大类型的整数倍 也就是8的整数倍 为24

	printf("sizeof Student:%d\n",sizeof(Student));

	//Student2 
	//a从偏移量为0位置开始 
	//b从偏移量为Student内部最大成员整数倍开始，也就是8开始
	//c从8的整数倍地方开始,也就是32开始
	//所以结构体Sutdnet2内部对齐之后的大小为：40 ， 由于结构体中最大成员为8，必须为8的整数倍 所以大小为40
	printf("sizeof Student2:%d\n", sizeof(Student2));
}
```


