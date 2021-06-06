---
title: assert
---

assert 宏的原型定义在 `assert.h` 中，其作用是如果它的条件返回错误，则终止程序执行。

assert 在 C 调试方面特别强大，基本上可以边调试边写代码。在 release 版本中，将 assert 语句关闭，所有的 assert 语句将和注释一样，不影响程序性能。

## 1. 样例

```c
#include <stdio.h>
#include <assert.h>

int main() {
  int t = 1;
  int f = -1;
  assert(t > 0);          // 断言为真, 继续执行
  printf("t = %d\n", t);  // 正常输出
  assert(f > 0);          // 断言为假, 结束进程
  printf("f = %d\n", f);  // 到不了这里 
  return 0;
}
```

运行此程序，得到如下结果：

```bash
t = 1
a.out: assert.c:9: main: Assertion `f > 0' failed.
[1]    28393 abort (core dumped)  ./a.out
```

