# 数组


字符数组是 C 语言中的一种数据类型，用于存储一系列字符。字符数组可以用来表示字符串或任何以 null 终止的字符序列。以下是关于字符数组的详细描述：

##### 1-1.**定义和声明**：

  字符数组是一个连续的内存块，用于存储字符。它可以通过以下方式定义和声明：

```c
  char myArray[10]; // 定义一个可以存储 10 个字符的字符数组
```


  这里的 `myArray` 是字符数组的名称，`char` 是元素的数据类型，`10` 是数组的大小。

##### 1-2.**初始化**：

  字符数组可以在声明时进行初始化，也可以在后续的代码中逐个字符赋值。例如：

```c
 char message[20] = "Hello, world!"; // 初始化字符数组
```

  或者：

```c
  char name[10];
  name[0] = 'J';
  name[1] = 'o';
  name[2] = 'h';
  // ...
```

##### 1-3.字符串表示：

  字符数组常常用来表示字符串。字符串是以 null 终止的字符序列，即以 `'\0'`（空字符）作为结束标志。例如：

```c
  char greeting[] = "Hello"; // 自动添加 '\0' 终止符
```

##### 1-4.**访问和操作**：

  字符数组中的每个字符都有一个对应的索引，从 0 开始。你可以使用索引来访问和修改字符数组中的元素。例如：

```c
  char word[] = "apple";
  char firstLetter = word[0]; // 获取第一个字符 'a'
  word[2] = 'r'; // 修改第三个字符为 'r'
```

#### 2.**字符串函数**：

  C 语言提供了一些处理字符数组和字符串的库函数，例如 `strlen`（获取字符串长度）、`strcpy`（复制字符串）、`strcat`（拼接字符串）等。

##### 2-1.strlen（计算字符串长度）：

  \- 功能：返回给定字符串的字符个数（不包括结尾的 null 终止字符）。
  \- 示例：

```c
   #include <string.h>
   int main() {
     char str[] = "Hello, world!";
     int length = strlen(str); // 返回 13
     return 0;
   }
```

##### 2-2.**strcpy**（复制字符串）：

  \- 功能：将一个字符串复制到另一个字符串。
  \- 示例：

```c
   #include <string.h>
   int main() {
     char source[] = "Hello";
     char destination[10];
     strcpy(destination, source); // 复制 "Hello" 到 destination
     return 0;
   }
```

##### 2-3.**strcat**（拼接字符串）：

  \- 功能：将一个字符串追加到另一个字符串的末尾。
  \- 示例：

```c
   #include <string.h>
   int main() {
     char str1[20] = "Hello, ";
     char str2[] = "world!";
     strcat(str1, str2); // 将 "world!" 拼接到 str1 后面
     return 0;
   }
```

##### 2-4. **strcmp**（比较字符串）：

  \- 功能：比较两个字符串的大小。
  \- 示例：

```c
   #include <string.h>
   int main() {
     char str1[] = "apple";
     char str2[] = "banana";
     int result = strcmp(str1, str2); // 返回负值，表示 str1 < str2
     return 0;
   }
```

##### 2-5. **strstr**（在字符串中查找子字符串）：

  \- 功能：在一个字符串中查找第一次出现的子字符串。
  \- 示例：

```c
   #include <string.h>
   int main() {
     char text[] = "This is a test.";
     char *substring = strstr(text, "is"); // 返回 "is is a test."
     return 0;
   }

```

##### 2-6. **fgets**（从文件流中读取一行）：

  \- 功能：从文件流中读取一行字符串，包括换行符，并将其存储到指定的字符数组中。
  \- 语法：`char *fgets(char *str, int num, FILE *stream);`
  \- 示例：



```c
  #include <stdio.h>
   int main() {
     char buffer[100];
     FILE *file = fopen("input.txt", "r"); // 假设存在 input.txt 文件
     if (file != NULL) {
       fgets(buffer, sizeof(buffer), file); // 从文件中读取一行到 buffer
       printf("Read from file: %s", buffer);
       fclose(file);
     }
     return 0;
   }
```

##### 2-7. **puts**（输出字符串到控制台）：

  \- 功能：将字符串输出到控制台，并自动添加换行符。
  \- 语法：`int puts(const char *str);`
  \- 示例：

```c
   #include <stdio.h>
   int main() {
     char message[] = "Hello, world!";
     puts(message); // 输出 "Hello, world!" 并换行
     return 0;
   }
```

##### 2-8. **fputs**（将字符串写入文件流）：

  \- 功能：将字符串写入指定的文件流，不会自动添加换行符。
  \- 语法：`int fputs(const char *str, FILE *stream);`
  \- 示例：

```c
   #include <stdio.h>
   int main() {
     char text[] = "This is some text.";
     FILE *file = fopen("output.txt", "w"); // 创建或覆盖 output.txt 文件
     if (file != NULL) {
       fputs(text, file); // 将字符串写入文件
       fclose(file);
     }
     return 0;
   }
```

##### 2-9. **scanf**（从标准输入读取格式化数据）：

  \- 功能：从标准输入读取格式化数据，并将其存储到指定的变量中。
  \- 语法：`int scanf(const char *format, ...);`
  \- 示例：

```c
  #include <stdio.h>
   int main() {
     char name[50];
     int age;
     printf("Enter your name and age: ");
     scanf("%s %d", name, &age); // 读取字符串和整数
     printf("Name: %s, Age: %d\n", name, age);
     return 0;
   }
```

#### 3.**注意事项**：

  \- 字符数组的大小应该足够容纳存储的字符，否则可能会导致内存溢出。
  \- 字符数组在声明时要留出一个额外的位置来存储终止字符 `'\0'`。
  \- 字符数组可以用于存储 ASCII 字符、特殊字符、数字等各种字符。

总之，字符数组是在 C 语言中用于存储字符序列的一种基本数据结构，通常用于表示字符串和处理文本数据。
