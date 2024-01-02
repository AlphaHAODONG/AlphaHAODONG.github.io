# 指针强化


### 1、指针是一种数据类型

#### 1.1 指针变量

指针是一种数据类型，占用内存空间，用来保存内存地址。

```c
void test01(){
	
	int* p1 = 0x1234;
	int*** p2 = 0x1111;

	printf("p1 size:%d\n",sizeof(p1));
	printf("p2 size:%d\n",sizeof(p2));


	//指针是变量，指针本身也占内存空间，指针也可以被赋值
	int a = 10;
	p1 = &a;

	printf("p1 address:%p\n", &p1);
	printf("p1 address:%p\n", p1);
	printf("a address:%p\n", &a);

}
```

#### 1.2 野指针和空指针

##### 1.2.1 空指针

标准定义了NULL指针，它作为一个特殊的指针变量，表示不指向任何东西。要使一个指针为NULL,可以给它赋值一个零值。为了测试一个指针百年来那个是否为NULL,你可以将它与零值进行比较。

对指针解引用操作可以获得它所指向的值。但从定义上看，NULL指针并未指向任何东西，因为对一个NULL指针因引用是一个非法的操作，在解引用之前，必须确保它不是一个NULL指针。

 如果对一个NULL指针间接访问会发生什么呢？结果因编译器而异。

***不允许向NULL和非法地址拷贝内存***

```c
void test(){
	char *p = NULL;
	//给p指向的内存区域拷贝内容
	strcpy(p, "1111"); //err

	char *q = 0x1122;
	//给q指向的内存区域拷贝内容
	strcpy(q, "2222"); //err		
}
```

##### 1.2.2 野指针

***在使用指针时，要避免野指针的出现：***

野指针指向一个已删除的对象或未申请访问受限内存区域的[指针](http://baike.baidu.com/view/159417.htm)。与空指针不同，野指针无法通过简单地判断是否为 [NULL](http://baike.baidu.com/view/329484.htm)避免，而只能通过养成良好的编程习惯来尽力减少。对野指针进行操作很容易造成程序错误。

***什么情况下回导致野指针？***

***指针变量未初始化***

任何指针变量刚被创建时不会自动成为NULL指针，它的缺省值是随机的，它会乱指一气。所以，指针变量在创建的同时应当被初始化，要么将指针设置为NULL，要么让它指向合法的内存。

***指针释放后未置空***

有时指针在free或delete后未赋值 NULL，便会使人以为是合法的。别看free和delete的名字（尤其是delete），它们只是把指针所指的内存给释放掉，但并没有把指针本身干掉。此时指针指向的就是“垃圾”内存。释放后的指针应立即将指针置为NULL，防止产生“野指针”。

***指针操作超越变量作用域***

不要返回指向栈内存的指针或引用，因为栈内存在函数结束时会被释放。

```c
void test(){
	int* p = 0x001; //未初始化
	printf("%p\n",p);
	*p = 100;
}
```

***操作野指针是非常危险的操作，应该规避野指针的出现：***

***初始化时置 NULL***

指针变量一定要初始化为NULL，因为任何指针变量刚被创建时不会自动成为NULL指针，它的缺省值是随机的。

 ***释放时置 NULL***

当指针p指向的内存空间释放时，没有设置指针p的值为NULL。delete和free只是把内存空间释放了，但是并没有将指针p的值赋为NULL。通常判断一个指针是否合法，都是使用if语句测试该指针是否为NULL。

#### 1.3 指针的步长

指针是一种数据类型，是指它指向的内存空间的数据类型。指针所指向的内存空间决定了指针的步长。指针的步长指的是，当指针+1时候，移动多少字节单位。

思考如下问题：

```c
int a = 0xaabbccdd;
unsigned int *p1 = &a;
unsigned char *p2 = &a;

//为什么*p1打印出来正确结果？
printf("%x\n", *p1);
//为什么*p2没有打印出来正确结果？
printf("%x\n", *p2);

//为什么p1指针+1加了4字节？
printf("p1  =%d\n", p1);
printf("p1+1=%d\n", p1 + 1);
//为什么p2指针+1加了1字节？
printf("p2  =%d\n", p2);
printf("p2+1=%d\n", p2 + 1);
```

指针的步长通常使用 `sizeof` 运算符来获取，该运算符返回指定类型或对象的大小（以字节为单位）。因此，获取指针的步长可以通过 `sizeof` 应用到指针所指的类型上。

下面是一个简单的例子，说明如何获取指针的步长：

```c
cCopy code#include <stdio.h>

int main() {
    int arr[] = {1, 2, 3, 4, 5};
    int *ptr = arr;

    // 获取指针的步长
    size_t stepSize = sizeof(*ptr);

    printf("Step size of the pointer: %zu bytes\n", stepSize);

    // 演示使用步长移动到下一个元素
    for (int i = 0; i < 5; ++i) {
        printf("Element %d: %d\n", i, *ptr);

        // 移动到下一个元素
        ptr += 1; // 此处的 1 表示移动一个步长，即一个元素的大小
    }

    return 0;
}
```

在这个例子中，`ptr` 是一个指向整数的指针，`sizeof(*ptr)` 返回整数的大小，因此 `stepSize` 的值表示指针的步长。然后，通过不断增加指针的值，我们可以在数组中移动到下一个元素。

请注意，这里使用 `size_t` 类型来保存步长，因为 `sizeof` 的返回类型是 `size_t`。

#### 1.4 间接访问操作符

通过一个指针访问它所指向的地址的过程叫做间接访问，或者叫解引用指针，这个用于执行间接访问的操作符是*。

注意：对一个int*类型指针解引用会产生一个整型值，类似地，对一个float*指针解引用会产生了一个float类型的值。

- 在指针声明时，* 号表示所声明的变量为指针
- 在指针使用时，* 号表示操作指针所指向的内存空间

1. 相当通过地址(指针变量的值)找到指针指向的内存，再操作内存
2. 放在等号的左边赋值（给内存赋值，写内存）
3. 放在等号的右边取值（从内存中取值，读内存）

```c
//解引用
void test01(){

	//定义指针
	int* p = NULL;
	//指针指向谁，就把谁的地址赋给指针
	int a = 10;
	p = &a;
	*p = 20;//*在左边当左值，必须确保内存可写
	//*号放右面，从内存中读值
	int b = *p;
	//必须确保内存可写
	char* str = "hello world!";
	*str = 'm';

	printf("a:%d\n", a);
	printf("*p:%d\n", *p);
	printf("b:%d\n", b);
}
```

### 2、指针的意义_间接赋值

#### 2.1 间接赋值的三大条件

通过指针间接赋值成立的三大条件：

1. 2个变量（一个普通变量一个指针变量、或者一个实参一个形参）
2. 建立关系
3. 通过 * 操作指针指向的内存

```c
void test(){
	int a = 100;	//两个变量
	int *p = NULL;
	//建立关系
	//指针指向谁，就把谁的地址赋值给指针
	p = &a;
	//通过*操作内存
	*p = 22;
}
```

#### 2.2 如何定义合适的指针变量

```c
void test(){
	int b;  
	int *q = &b; //0级指针
	int **t = &q;
	int ***m = &t;
}
```

#### 2.3 间接赋值：从0级指针到1级指针

```c
int func1(){ return 10; }

void func2(int a){
	a = 100;
}
//指针的意义_间接赋值
void test02(){
	int a = 0;
	a = func1();
	printf("a = %d\n", a);

	//为什么没有修改？
	func2(a);
	printf("a = %d\n", a);
}

//指针的间接赋值
void func3(int* a){
	*a = 100;
}

void test03(){
	int a = 0;
	a = func1();
	printf("a = %d\n", a);

	//修改
	func3(&a);
	printf("a = %d\n", a);
}
```

#### 2.4 间接赋值：从1级指针到2级指针

```c
void AllocateSpace(char** p){
	*p = (char*)malloc(100);
	strcpy(*p, "hello world!");
}

void FreeSpace(char** p){

	if (p == NULL){
		return;
	}
	if (*p != NULL){
		free(*p);
		*p = NULL;
	}

}

void test(){
	
	char* p = NULL;

	AllocateSpace(&p);
	printf("%s\n",p);
	FreeSpace(&p);

	if (p == NULL){
		printf("p内存释放!\n");
	}
}
```

- 用1级指针形参，去间接修改了0级指针(实参)的值。
- 用2级指针形参，去间接修改了1级指针(实参)的值。
- 用3级指针形参，去间接修改了2级指针(实参)的值。
- 用n级指针形参，去间接修改了n-1级指针(实参)的值。

### 3、指针做函数参数

指针做函数参数，具备输入和输出特性：

- 输入：主调函数分配内存
- 输出：被调用函数分配内存

#### 3.1 输入特性

```c
void fun(char *p /* in */)
{
	//给p指向的内存区域拷贝内容
	strcpy(p, "abcddsgsd");
}

void test(void)
{
	//输入，主调函数分配内存
	char buf[100] = { 0 };
	fun(buf);
	printf("buf  = %s\n", buf);
}
```

#### 3.2 输出特性

```c
void fun(char **p /* out */, int *len)
{
	char *tmp = (char *)malloc(100);
	if (tmp == NULL)
	{
		return;
	}
	strcpy(tmp, "adlsgjldsk");

	//间接赋值
	*p = tmp;
	*len = strlen(tmp);
}

void test(void)
{
	//输出，被调用函数分配内存，地址传递
	char *p = NULL;
	int len = 0;
	fun(&p, &len);
	if (p != NULL)
	{
		printf("p = %s, len = %d\n", p, len);
	}
```

### 4、字符串指针

#### 4.1 字符串指针做函数参数

##### 4.1.1 字符串基本操作

```c
//字符串基本操作
//字符串是以0或者'\0'结尾的字符数组，(数字0和字符'\0'等价)
void test01(){

	//字符数组只能初始化5个字符，当输出的时候，从开始位置直到找到0结束
	char str1[] = { 'h', 'e', 'l', 'l', 'o' };
	printf("%s\n",str1);

	//字符数组部分初始化，剩余填0
	char str2[100] = { 'h', 'e', 'l', 'l', 'o' };
	printf("%s\n", str2);

	//如果以字符串初始化，那么编译器默认会在字符串尾部添加'\0'
	char str3[] = "hello";
	printf("%s\n",str3);
	printf("sizeof str:%d\n",sizeof(str3));
	printf("strlen str:%d\n",strlen(str3));

	//sizeof计算数组大小，数组包含'\0'字符
	//strlen计算字符串的长度，到'\0'结束

	//那么如果我这么写,结果是多少呢？
	char str4[100] = "hello";
	printf("sizeof str:%d\n", sizeof(str4));
	printf("strlen str:%d\n", strlen(str4));

	//请问下面输入结果是多少？sizeof结果是多少？strlen结果是多少？
	char str5[] = "hello\0world"; 
	printf("%s\n",str5);
	printf("sizeof str5:%d\n",sizeof(str5));
	printf("strlen str5:%d\n",strlen(str5));

	//再请问下面输入结果是多少？sizeof结果是多少？strlen结果是多少？
	char str6[] = "hello\012world";
	printf("%s\n", str6);
	printf("sizeof str6:%d\n", sizeof(str6));
	printf("strlen str6:%d\n", strlen(str6));
}
```

![image-20231219181014966](https://alphaguo-1322521250.cos.ap-beijing.myqcloud.com/202312312336401.png)



八进制和十六进制转义字符：

   ***在C中有两种特殊的字符，八进制转义字符和十六进制转义字符，八进制字符的一般形式是'\ddd'，d是0-7的数字。十六进制字符的一般形式是'\xhh'，h是0-9或A-F内的一个。八进制字符和十六进制字符表示的是字符的ASCII码对应的数值。比如 ： n '\063'表示的是字符'3'，因为'3'的ASCII码是30（十六进制），48（十进制），63（八进制）。n '\x41'表示的是字符'A'，因为'A'的ASCII码是41（十六进制），65（十进制），101（八进制）。***

##### 4.1.2 字符串拷贝功能实现

```c
//拷贝方法1
void copy_string01(char* dest, char* source ){

	for (int i = 0; source[i] != '\0';i++){
		dest[i] = source[i];
	}

}

//拷贝方法2
void copy_string02(char* dest, char* source){
	while (*source != '\0' /* *source != 0 */){
		*dest = *source;
		source++;
		dest++;
	}
}

//拷贝方法3
void copy_string03(char* dest, char* source){
	//判断*dest是否为0，0则退出循环
	while (*dest++ = *source++){}
}

```

##### 4.1.3 字符串反转模型

![img](https://alphaguo-1322521250.cos.ap-beijing.myqcloud.com/202312312336416.jpeg)

```c
void reverse_string(char* str){

	if (str == NULL){
		return;
	}

	int begin = 0;
	int end = strlen(str) - 1;
	
	while (begin < end){
		
		//交换两个字符元素
		char temp = str[begin];
		str[begin] = str[end];
		str[end] = temp;

		begin++;
		end--;
	}

}

void test(){
	char str[] = "abcdefghijklmn";
	printf("str:%s\n", str);
	reverse_string(str);
	printf("str:%s\n", str);
}
```

#### 4.2 字符串的格式化

##### 4.2.1 sprintf

```c
#include <stdio.h>
int sprintf(char *str, const char *format, ...);
功能：
     根据参数format字符串来转换并格式化数据，然后将结果输出到str指定的空间中，直到    出现字符串结束符 '\0'  为止。
参数： 
	str：字符串首地址
	format：字符串格式，用法和printf()一样
返回值：
	成功：实际格式化的字符个数
	失败： - 1
```

```c
void test(){
	
	//1. 格式化字符串
	char buf[1024] = { 0 };
	sprintf(buf, "你好,%s,欢迎加入我们!", "John");
	printf("buf:%s\n",buf);

	memset(buf, 0, 1024);
	sprintf(buf, "我今年%d岁了!", 20);
	printf("buf:%s\n", buf);

	//2. 拼接字符串
	memset(buf, 0, 1024);
	char str1[] = "hello";
	char str2[] = "world";
	int len = sprintf(buf,"%s %s",str1,str2);
	printf("buf:%s len:%d\n", buf,len);

	//3. 数字转字符串
	memset(buf, 0, 1024);
	int num = 100;
	sprintf(buf, "%d", num);
	printf("buf:%s\n", buf);
	//设置宽度 右对齐
	memset(buf, 0, 1024);
	sprintf(buf, "%8d", num);
	printf("buf:%s\n", buf);
	//设置宽度 左对齐
	memset(buf, 0, 1024);
	sprintf(buf, "%-8d", num);
	printf("buf:%s\n", buf);
	//转成16进制字符串 小写
	memset(buf, 0, 1024);
	sprintf(buf, "0x%x", num);
	printf("buf:%s\n", buf);

	//转成8进制字符串
	memset(buf, 0, 1024);
	sprintf(buf, "0%o", num);
	printf("buf:%s\n", buf);
}
```

##### 4.2.2 sscanf

\*#include <stdio.h>*

*int sscanf**(**const char *****str**,** const char *****format**,** **...);***

*功能：*

  *从str指定的字符串读取数据，并根据参数format字符串来转换并格式化数据。*

*参数：*

​	*str：指定的字符串首地址*

​	*format：字符串格式，用法和scanf**()**一样*

*返回值：*

​	*成功：成功则返回参数数目，失败则返回-1*

​	*失败： **-** 1*

| **格式**     | **作用**                           |
| ------------ | ---------------------------------- |
| `%*s`或`%*d` | 跳过数据                           |
| `%[width]s`  | 读指定宽度的数据                   |
| `%[a-z]`     | 匹配a到z中任意字符(尽可能多的匹配) |
| `%[aBc]`     | 匹配a、B、c中一员，贪婪性          |
| `%[^a]`      | 匹配非a的任意字符，贪婪性          |
| `%[^a-z]`    | 表示读取除a-z以外的所有字符        |

 

```c
//1. 跳过数据
void test01(){
	char buf[1024] = { 0 };
	//跳过前面的数字
	//匹配第一个字符是否是数字，如果是，则跳过
	//如果不是则停止匹配
	sscanf("123456aaaa", "%*d%s", buf); 
	printf("buf:%s\n",buf);
}

//2. 读取指定宽度数据
void test02(){
	char buf[1024] = { 0 };
	//跳过前面的数字
	sscanf("123456aaaa", "%7s", buf);
	printf("buf:%s\n", buf);
}

//3. 匹配a-z中任意字符
void test03(){
	char buf[1024] = { 0 };
	//跳过前面的数字
	//先匹配第一个字符，判断字符是否是a-z中的字符，如果是匹配
	//如果不是停止匹配
	sscanf("abcdefg123456", "%[a-z]", buf);
	printf("buf:%s\n", buf);
}

//4. 匹配aBc中的任何一个
void test04(){
	char buf[1024] = { 0 };
	//跳过前面的数字
	//先匹配第一个字符是否是aBc中的一个，如果是，则匹配，如果不是则停止匹配
//如果匹配失败，则后续不再匹配
	sscanf("abcdefg123456", "%[aBc]", buf);
	printf("buf:%s\n", buf);
}

//5. 匹配非a的任意字符
void test05(){
	char buf[1024] = { 0 };
	sscanf("bcdefag123456", "%[^a]", buf);
	printf("buf:%s\n", buf);
}

//6. 匹配非a-z中的任意字符
void test06(){
	char buf[1024] = { 0 };
	sscanf("123456ABCDbcdefag", "%[^a-z]", buf);
	printf("buf:%s\n", buf);
}
```

### 5、const使用

```c
//const修饰变量
void test01(){
	//1. const基本概念
	const int i = 0;
	//i = 100; //错误，只读变量初始化之后不能修改

	//2. 定义const变量最好初始化
	const int j;
	//j = 100; //错误，不能再次赋值

	//3. c语言的const是一个只读变量，并不是一个常量，可通过指针间接修改
	const int k = 10;
	//k = 100; //错误，不可直接修改，我们可通过指针间接修改
	printf("k:%d\n", k);
	int* p = &k;
	*p = 100;
	printf("k:%d\n", k);
}

//const 修饰指针
void test02(){

	int a = 10;
	int b = 20;
	//const放在*号左侧 修饰p_a指针指向的内存空间不能修改,但可修改指针的指向
	const int* p_a = &a;
	//*p_a = 100; //不可修改指针指向的内存空间
	p_a = &b; //可修改指针的指向

	//const放在*号的右侧， 修饰指针的指向不能修改，但是可修改指针指向的内存空间
	int* const p_b = &a;
	//p_b = &b; //不可修改指针的指向
	*p_b = 100; //可修改指针指向的内存空间

	//指针的指向和指针指向的内存空间都不能修改
	const int* const p_c = &a;
}
//const指针用法
struct Person{
	char name[64];
	int id;
	int age;
	int score;
};

//每次都对对象进行拷贝，效率低，应该用指针
void printPersonByValue(struct Person person){
	printf("Name:%s\n", person.name);
	printf("Name:%d\n", person.id);
	printf("Name:%d\n", person.age);
	printf("Name:%d\n", person.score);
}

//但是用指针会有副作用，可能会不小心修改原数据
void printPersonByPointer(const struct Person *person){
	printf("Name:%s\n", person->name);
	printf("Name:%d\n", person->id);
	printf("Name:%d\n", person->age);
	printf("Name:%d\n", person->score);
}
void test03(){
	struct Person p = { "Obama", 1101, 23, 87 };
	//printPersonByValue(p);
	printPersonByPointer(&p);
}
```

### 6、位运算

​		可以使用C对变量中的个别位进行操作。您可能对人们想这样做的原因感到奇怪。这种能力有时确实是必须的，或者至少是有用的。C提供位的逻辑运算符和移位运算符。在以下例子中，我们将使用二进制计数法写出值，以便您可以了解对位发生的操作。在一个实际程序中，您可以使用一般的形式的整数变量或常量。例如不适用00011001的形式，而写为25或者031或者0x19.在我们的例子中，我们将使用8位数字，从左到右，每位的编号是7到0。

#### **6.1位逻辑运算符**

​		4个位运算符用于整型数据，包括char.将这些位运算符成为位运算的原因是它们对每位进行操作，而不影响左右两侧的位。请不要将这些运算符与常规的逻辑运算符(&& 、||和!)相混淆，常规的位的逻辑运算符对整个值进行操作。

##### 6.1.1 按位取反 `~`

一元运算符`~`将每个1变为0，将每个0变为1，如下面的例子：

```c
~(10011010)
01100101
```

假设a是一个unsigned char，已赋值为2.在二进制中，2是00000010.于是-a的值为11111101或者253。请注意该运算符不会改变a的值，a仍为2。

```c
unsigned char a = 2;   //00000010
unsigned char b = ~a;  //11111101
printf("ret = %d\n", a); //ret = 2
printf("ret = %d\n", b); //ret = 253
```

##### 6.1.2 位与（AND）：`&`

二进制运算符`&`通过对两个操作数逐位进行比较产生一个新值。对于每个位，只有两个操作数的对应位都是1时结果才为1。

```c
   (10010011) 
 & (00111101) 
 = (00010001)
```

C也有一个组合的位与-赋值运算符：`&=`。下面两个将产生相同的结果：

```c
val &= 0377
val = val & 0377
```

##### 6.1.3 位或（OR）：`|`

二进制运算符`|`通过对两个操作数逐位进行比较产生一个新值。对于每个位，如果其中任意操作数中对应的位为1，那么结果位就为1.

```c
	(10010011)
  | (00111101)
  = (10111111)
```

C也有组合位或-赋值运算符： `|=`

```c
val |= 0377
val = val | 0377
```

##### 6.1.4 位异或：

二进制运算符^对两个操作数逐位进行比较。对于每个位，如果操作数中的对应位有一个是1(但不是都是1)，那么结果是1.如果都是0或者都是1，则结果位0.

```c
	(10010011)
  ^ (00111101)
  = (10101110)
```

C也有一个组合的位异或-赋值运算符： `^=`

```c
val ^= 0377
val = val ^ 0377
```

#### 6.2移位运算符

##### 6.2.1左移`<<`

左移运算符<<将其左侧操作数的值的每位向左移动，移动的位数由其右侧操作数指定。空出来的位用0填充，并且丢弃移出左侧操作数末端的位。在下面例子中，每位向左移动两个位置。

```c
(10001010) << 2
(00101000)
```

该操作将产生一个新位置，但是不改变其操作数。

```c
1 << 1 = 2;
2 << 1 = 4;
4 << 1 = 8;
8 << 2 = 32
```

左移一位相当于原值`*2`。

##### 6.2.2右移`>>`

右移运算符>>将其左侧的操作数的值每位向右移动，移动的位数由其右侧的操作数指定。丢弃移出左侧操作数有段的位。对于unsigned类型，使用0填充左端空出的位。**对于有符号类型，结果依赖于机器。空出的位可能用0填充，或者使用符号(最左端)位的副本填充。**

```c
//有符号值
(10001010) >> 2
(00100010)     //在某些系统上的结果值

(10001010) >> 2
(11100010)     //在另一些系统上的结果

//无符号值
(10001010) >> 2
(00100010)    //所有系统上的结果值
```

右移一位相当于原值`/2`。

![img](https://alphaguo-1322521250.cos.ap-beijing.myqcloud.com/202312312336427.png)


