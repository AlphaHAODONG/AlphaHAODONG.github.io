# 内存


extern，静态函数，，内存操作函数（memset,memcpy）

### 1、概念

![image-20231118180320417](https://alphaguo-1322521250.cos.ap-beijing.myqcloud.com/202312312332198.png)

#### 1.1**局部变量**

- **定义：** 局部变量是在函数内部声明的变量，只在声明它的函数中可见。
- **生命周期：** 局部变量的生命周期从声明开始，到函数执行结束。每次函数被调用时，都会创建新的局部变量。
- **示例：**

```c
cCopy code#include <stdio.h>

void exampleFunction() {
    int localVar = 42;  // 局部变量

    printf("Local Variable: %d\n", localVar);
}

int main() {
    exampleFunction();

    // 下面的语句将会引发编译错误，因为 localVar 只在 exampleFunction 函数中可见
    // printf("Trying to access local variable outside the function: %d\n", localVar);

    return 0;
}
```

#### 1.2**静态局部变量**

- **定义：** 静态局部变量是在函数内部声明的，但使用 `static` 关键字修饰的变量。与普通局部变量不同，静态局部变量的生命周期在整个程序运行期间，而不是在函数执行期间。
- **生命周期：** 静态局部变量的生命周期从程序开始运行到结束，但它仍然只在声明它的函数中可见。
- **示例：**

```c
cCopy code#include <stdio.h>

void exampleFunction() {
    static int staticLocalVar = 42;  // 静态局部变量

    printf("Static Local Variable: %d\n", staticLocalVar);

    // 修改静态局部变量的值，下次函数调用时保留修改后的值
    staticLocalVar++;
}

int main() {
    exampleFunction();
    exampleFunction();
    exampleFunction();

    return 0;
}
```

#### 1.3**全局变量**

- **定义：** 全局变量是在函数外部声明的变量，它可以在整个程序中的任何位置访问。
- **生命周期：** 全局变量的生命周期从程序开始运行到结束。
- **示例：**

```c
cCopy code#include <stdio.h>

int globalVar = 100;  // 全局变量

void exampleFunction() {
    printf("Global Variable: %d\n", globalVar);
}

int main() {
    exampleFunction();

    // 修改全局变量的值，其他函数调用时将反映修改后的值
    globalVar = 200;

    exampleFunction();

    return 0;
}
```

#### 1.4**静态全局变量**

- **定义：** 静态全局变量是在文件的顶部声明的，使用 `static` 关键字修饰的全局变量。它在整个文件中可见，但对其他文件是不可见的。
- **生命周期：** 静态全局变量的生命周期从程序开始运行到结束。
- **示例：**

```c
cCopy code// 文件1.c
#include <stdio.h>

static int staticGlobalVar = 500;  // 静态全局变量

void exampleFunction() {
    printf("Static Global Variable: %d\n", staticGlobalVar);
}

// 文件2.c
#include <stdio.h>

extern void exampleFunction();  // 外部声明，使得其他文件可以访问 exampleFunction

int main() {
    exampleFunction();

    return 0;
}
```

这些变量的作用域和生命周期不同，选择使用哪种类型取决于程序的需求。要注意，全局变量和静态全局变量可能导致程序的可维护性降低，因为它们在整个程序中可见，可能被多个函数修改。因此，在使用全局变量时应当谨慎，优先考虑使用函数的参数传递和返回值传递数据。

#### 1.5内存泄漏

内存泄漏是指程序在运行时分配了一块内存，但在不再使用它时没有释放或回收这块内存的情况。内存泄漏可能导致程序使用的内存逐渐增加，最终耗尽系统的可用内存，导致程序崩溃或系统变得不稳定。内存泄漏是一种常见的程序错误，因此在编写代码时需要特别注意动态内存的管理。

以下是一个简单的例子，演示了内存泄漏的情况：

```c
cCopy code#include <stdlib.h>

void memoryLeakExample() {
    // 分配一块内存
    int *ptr = (int *)malloc(sizeof(int));

    // 不释放内存，导致内存泄漏
}

int main() {
    memoryLeakExample();

    // 主函数结束，程序退出，但动态分配的内存没有被释放
    // 这会导致内存泄漏，因为无法再访问该内存并释放它

    return 0;
}
```

在这个例子中，`memoryLeakExample` 函数动态分配了一块整数大小的内存，但没有在函数结束时释放它。当程序运行时调用 `memoryLeakExample` 函数后，动态分配的内存就会变成不可访问，而系统无法再回收这块内存。这就是典型的内存泄漏情况。

为了避免内存泄漏，程序员通常应该确保在不再需要使用动态分配的内存时，通过调用 `free` 函数来释放它。修改上述代码如下：

```c
cCopy code#include <stdlib.h>

void noMemoryLeakExample() {
    // 分配一块内存
    int *ptr = (int *)malloc(sizeof(int));

    // 使用内存...

    // 释放内存
    free(ptr);
}

int main() {
    noMemoryLeakExample();

    // 内存在函数调用结束时得到释放，避免了内存泄漏

    return 0;
}
```

在这个修改后的例子中，动态分配的内存在函数结束时被正确释放，从而避免了内存泄漏。

### 2、内存操作函数

#### 2.1extern

`extern` 是C语言中的一个关键字，用于声明一个变量或函数的外部链接性（external linkage）。当一个变量或函数被声明为 `extern` 时，它意味着该变量或函数的定义存在于其他文件中，而不是当前文件中。`extern` 的使用主要涉及到多个源文件的情况，其中一个文件可能需要访问另一个文件中定义的变量或函数。

```c
cCopy code// 文件1.c
#include <stdio.h>

int globalVar = 42;  // 全局变量的定义

// 文件2.c
#include <stdio.h>

extern int globalVar;  // 外部声明，告知编译器 globalVar 是在其他文件中定义的

int main() {
    printf("Global Variable from another file: %d\n", globalVar);

    return 0;
}
```

#### 2.2静态函数

在C语言中，函数默认情况下是具有文件作用域的，即只在定义它的文件中可见。因此，函数默认情况下就是静态的。当函数被声明为 `static` 时，它的链接性被限制为文件作用域，不可在其他文件中访问。静态函数对于将函数的作用域限制在当前文件中是很有用的，可以避免与其他文件中相同名称的函数发生冲突。

```c
cCopy code// 文件1.c
#include <stdio.h>

static void staticFunction() {
    printf("This is a static function.\n");
}

// 文件2.c
// 尝试在文件2.c中调用 staticFunction 会导致编译错误，因为它是文件1.c的局部函数
```

#### 2.3memset

![image-20231118180946769](https://alphaguo-1322521250.cos.ap-beijing.myqcloud.com/202312312332145.png)

- **功能：** 用指定的值填充一块内存。

- 原型：

  ```c
  cCopy code
  void *memset(void *ptr, int value, size_t num);
  ```

- 示例：

  ```c
  cCopy code#include <stdio.h>
  #include <string.h>
  
  int main() {
      char str[20];
  
      // 使用memset将str数组的前10个字节填充为'A'
      memset(str, 'A', 10);
  
      printf("After memset: %s\n", str);
  
      return 0;
  }
  ```

#### 2.4memcpy

![image-20231118181006742](https://alphaguo-1322521250.cos.ap-beijing.myqcloud.com/202312312332242.png)

- **功能：** 从源内存地址复制一定数量的字节到目标内存地址。

- 原型：

  ```c
  cCopy code
  void *memcpy(void *dest, const void *src, size_t num);
  ```

- 示例：

  ```c
  cCopy code#include <stdio.h>
  #include <string.h>
  
  int main() {
      char src[] = "Hello";
      char dest[20];
  
      // 使用memcpy将src数组的内容复制到dest数组中
      memcpy(dest, src, strlen(src) + 1);
  
      printf("After memcpy: %s\n", dest);
  
      return 0;
  }
  ```

#### 2.5memcmp

![image-20231118181022525](https://alphaguo-1322521250.cos.ap-beijing.myqcloud.com/202312312332189.png)

- **功能：** 比较两块内存区域的前 num 个字节。

- 原型：

  ```c
  cCopy code
  int memcmp(const void *ptr1, const void *ptr2, size_t num);
  ```

- 示例：

  ```c
  cCopy code#include <stdio.h>
  #include <string.h>
  
  int main() {
      char str1[] = "abcd";
      char str2[] = "abCd";
  
      // 使用memcmp比较两个字符串的前3个字节
      int result = memcmp(str1, str2, 3);
  
      if (result == 0) {
          printf("The first 3 bytes are equal.\n");
      } else {
          printf("The first 3 bytes are not equal.\n");
      }
  
      return 0;
  }
  ```

#### 2.6malloc

![image-20231118181032955](https://alphaguo-1322521250.cos.ap-beijing.myqcloud.com/202312312332156.png)

- **功能：** 在堆上分配指定大小的内存块。

- 原型：

  ```c
  cCopy code
  void *malloc(size_t size);
  ```

- 示例：

  ```c
  cCopy code#include <stdio.h>
  #include <stdlib.h>
  
  int main() {
      int *arr;
  
      // 使用malloc分配包含5个整数的内存块
      arr = (int *)malloc(5 * sizeof(int));
  
      if (arr != NULL) {
          for (int i = 0; i < 5; ++i) {
              arr[i] = i * 2;
              printf("%d ", arr[i]);
          }
  
          // 释放动态分配的内存
          free(arr);
      } else {
          printf("Memory allocation failed.\n");
      }
  
      return 0;
  }
  ```

#### 2.7返回变量的地址

![image-20231119174009806](https://alphaguo-1322521250.cos.ap-beijing.myqcloud.com/202312312332182.png)

这种情况通常发生在函数内部创建了一个局部变量，然后返回该变量的地址。要确保返回的地址在函数调用结束后仍然有效，可以使用动态内存分配或者将变量声明为静态变量。

下面是一个使用动态内存分配的例子：

```c
cCopy code#include <stdio.h>
#include <stdlib.h>

int* createAndReturnArray(int size) {
    // 使用动态内存分配创建整数数组
    int *arr = (int *)malloc(size * sizeof(int));

    // 将数组元素赋值
    for (int i = 0; i < size; ++i) {
        arr[i] = i * 2;
    }

    // 返回数组的首地址
    return arr;
}

int main() {
    int size = 5;

    // 调用函数创建数组并获取数组的地址
    int *resultArray = createAndReturnArray(size);

    // 打印数组元素
    for (int i = 0; i < size; ++i) {
        printf("%d ", resultArray[i]);
    }

    // 释放动态分配的内存
    free(resultArray);

    return 0;
}
```

在这个例子中，`createAndReturnArray` 函数创建一个包含整数的数组，然后返回数组的首地址。在 `main` 函数中，调用了 `createAndReturnArray` 函数，并通过返回的地址访问了数组的元素。需要注意的是，在使用完动态分配的内存后，需要调用 `free` 函数释放该内存，以防止内存泄漏。

另外，如果变量是一个静态变量，它的地址可以直接返回，因为静态变量的生命周期跨越整个程序运行期间。

```c
cCopy code#include <stdio.h>

int* getStaticVariable() {
    // 声明为静态变量
    static int staticVar = 42;

    // 返回静态变量的地址
    return &staticVar;
}

int main() {
    // 获取静态变量的地址
    int *ptr = getStaticVariable();

    // 打印静态变量的值
    printf("Static Variable: %d\n", *ptr);

    return 0;
}
```

在这个例子中，`getStaticVariable` 函数返回了静态变量 `staticVar` 的地址，而 `main` 函数通过这个地址访问了静态变量的值。由于静态变量的生命周期跨越整个程序，返回其地址是安全的。

#### 2.8返回堆区的地址

![image-20231119174045220](https://alphaguo-1322521250.cos.ap-beijing.myqcloud.com/202312312332905.png)

返回堆区的地址通常是通过动态内存分配函数（例如`malloc`、`calloc`等）创建的对象的地址。当函数中使用动态内存分配创建对象时，可以通过返回指向这个对象的指针来返回堆区的地址。

以下是一个简单的例子，演示如何在函数中动态分配内存，并返回分配的内存地址：

```c
cCopy code#include <stdio.h>
#include <stdlib.h>

// 函数返回堆区分配的整数数组的地址
int* createAndReturnArray(int size) {
    // 使用动态内存分配创建整数数组
    int *arr = (int *)malloc(size * sizeof(int));

    // 将数组元素赋值
    for (int i = 0; i < size; ++i) {
        arr[i] = i * 2;
    }

    // 返回数组的首地址
    return arr;
}

int main() {
    int size = 5;

    // 调用函数创建数组并获取数组的地址
    int *resultArray = createAndReturnArray(size);

    // 打印数组元素
    for (int i = 0; i < size; ++i) {
        printf("%d ", resultArray[i]);
    }

    // 释放动态分配的内存
    free(resultArray);

    return 0;
}
```

在这个例子中，`createAndReturnArray` 函数创建了一个包含整数的数组，并返回了该数组的首地址。在 `main` 函数中，通过调用 `createAndReturnArray` 获取了数组的地址，并使用该地址访问了数组的元素。需要注意的是，调用者有责任在使用完动态分配的内存后调用 `free` 函数释放这些内存，以防止内存泄漏。

返回堆区的地址时，要确保调用者了解如何正确使用并最终释放分配的内存。

#### 2.9实参为一级指针的地址

![image-20231119174104845](https://alphaguo-1322521250.cos.ap-beijing.myqcloud.com/202312312332620.png)

实参为一级指针的地址，意味着函数接受一个指向指针的指针（二级指针）作为参数。这允许函数能够修改指针本身，而不仅仅是指针指向的数据。

下面是一个例子，演示了如何使用一级指针的地址作为实参，并在函数中修改指针本身：

```c
cCopy code#include <stdio.h>

// 函数接受一级指针的地址，修改指针的值
void modifyPointer(int **ptrAddress) {
    int newValue = 100;

    // 修改指针的值
    *ptrAddress = &newValue;
}

int main() {
    int originalValue = 42;
    int *myPointer = &originalValue;

    // 打印原始指针的值
    printf("Original Pointer Value: %p\n", (void *)myPointer);

    // 调用函数，传递指针的地址
    modifyPointer(&myPointer);

    // 打印修改后的指针的值
    printf("Modified Pointer Value: %p\n", (void *)myPointer);

    return 0;
}
```

在这个例子中，`modifyPointer` 函数接受一个指向指针的指针（`int **ptrAddress`），然后通过修改指针的值（`*ptrAddress = &newValue`）来改变原始指针指向的地址。在 `main` 函数中，创建了一个整数变量 `originalValue` 和一个指向它的指针 `myPointer`，然后调用了 `modifyPointer` 函数，传递了指针的地址。通过打印原始指针和修改后的指针的值，可以看到指针的值已经被修改。

这种技术在需要在函数内修改指针本身的情况下非常有用。在实际应用中，需要小心确保修改指针的行为不会导致悬挂指针（dangling pointers）或者内存泄漏。
