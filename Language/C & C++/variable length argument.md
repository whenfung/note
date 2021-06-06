---
title: 不定长参数
---

有时候一个函数的参数数量不固定，例如 C 语言的 `open()` 函数和 `printf()` 函数。

C 语言提供了可变参数，能根据具体的需求接受可变数量的参数。一般格式如下：

```c
int func(int, ... ) 
{
   .
   .
   .
}
 
int main()
{
   func(2, 2, 3);
   func(3, 2, 3, 4);
}
```

函数 `func()` 的最后一个参数写成省略号，即 `...`，省略号之前的那个参数是 `int`，代表了要传递的可变参数的总数。为了使用这个功能，需要使用 `stdarg.h` 头文件，该文件提供了实现可变参数功能的函数和宏。具体步骤如下：

1. 定义一个函数，最后一个参数为省略号，省略号前面可以设置自定义参数。
2. 在函数定义中创建一个 `va_list` 类型变量，该类型是在 `stdarg.h` 头文件中定义的。
3. 使用 `int` 参数和 `va_start` 宏来初始化 `va_list` 变量为一个参数列表。宏 `va_start` 是在 `stdarg.h` 头文件中定义的。
4. 使用 `va_arg` 宏和 `va_list` 变量来访问参数列表中的每个项。

例如我们要求 n 个整数的和，实现如下：

```c
#include <stdio.h>
#include <stdarg.h>

int sum(int num, ...) 
{
  va_list valist;
  va_start(valist, num);  // 为 num 个参数初始化 valist

  int sum = 0;
  for(int i = 0; i < num; i ++)
    sum += va_arg(valist, int);   // 逐个访问参数
  va_end(valist);   // 清理为 valist 保留的内存
  return sum;
}

int main() {
  printf("1 + 2 = %d\n", sum(2, 1, 2));
  printf("1 + 2 + 3 = %d\n", sum(3, 1, 2, 3));
  return 0;
}
```

## 实现原理

可变参数的工作原理，以 32 位机为例：

1. 函数参数的传递存储在栈中，从右至左压入栈中，压栈过程为递减。
2. 参数的传递以 4 字节对齐，`float` / `double` 这里不讨论。