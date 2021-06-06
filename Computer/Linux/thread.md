---
title: Linux 线程
---

进程（Process）是资源分配的最小单位，线程（Lightweight Process, LWP）是执行的最小单位。

一般一个最简单的程序最少会有一个线程，就是程序本身，也就是主函数。单线程的进程可以简单的认为只有一个线程的进程。一个线程阻塞并不会影响到另外一个线程。多线程的进程可以尽可能的利用系统 CPU 资源。Linux 可以 `ps -aLf` 查看当前运行的进程。

## 1. 创建线程

源代码为 `create.c`。

Linux 用 `pthread_create()` 创建一个新线程，原型为：

```c
int pthread_create(pthread_t *thread, const pthread_attr_t *attr,
                          void *(*start_routine) (void *), void *arg);
```

参数有 4 个：

1. `pthread_t *thread` ：代表创建线程的唯一标识，是一个结构体，需要我们创建好后，将这个结构体的指针传递过去。
2. `const pthread_attr_t *attr` ：代表创建这个线程的一些配置，比如分配栈的大小等等。一般我们可以填 NULL，代表默认的创建线程的配置。
3. `void *(*start_routine) (void *)` ：代表一个函数的地址，创建线程时，会调用这个函数，函数的返回值是 `void*`，函数的参数也是 `void*`，一般格式就像 `void* func(void* arg){}`。
4. `void* arg`  ：代表调用第三个函数传递的参数。

函数成功返回 0，如果不等于 0 则代表函数调用失败，此时通过 `strerror(errno)` 可以打印出具体的错误。

注意：每个线程都拥有一份 `errno` 副本，不同的线程拥有不同的 `errno`。

 `gcc` 编译的时候需要加上 `-lpthread` 用来链接 `libpthread.so` 动态库。

```bash
gcc test.c -lpthread
```

## 2. 线程挂起

源代码为 `join.c`。

有时候我们在一个线程中创建了另外一个线程，主线程要等到创建的线程返回了，获取该线程的返回值后主线程才退出。这个时候就需要将主线程挂起。函数原型如下

```c
int pthread_join(pthread_t thread, void **retval);
```

## 3. 线程终止

源代码为 `exit.c`

线程终止的三种情况：

1. 线程只是从启动函数中返回，返回值是线程的退出码。
2. 线程可以被同一进程中的其他线程取消。
3. 线程调用 `pthread_exit()`。

`pthread_exit()` 的原型如下

```c
void pthread_exit(void *retval);
```

## 4. 线程分离

源代码为 `detach.c`

如果主线程不等待一个某线程，同时对该线程的返回值不感兴趣，可以设置这个线程为被分离状态，让系统在线程退出的时候自动回收它所占用的资源。

线程分离的函数原型如下：

```c
int pthread_detach(pthread_t thread);
```

一个线程不能自己调用 `pthread_detach()` 改变自己为被分离状态，只能由其他线程调用 `pthread_detach()`。

当然在使用 `pthread_create()` 时可以设置第二个参数，使线程一创建就是分离状态的线程。

## 5. 线程取消

源代码为 `cancel.c`。

取消一个线程的函数原型为：

```c
int pthread_cancel(pthread_t thread);
```

## 6. 线程同步

不同步的源码为 `nosyn.c`。

同步的源码为 `syn.c`。

可以使用 `PTHREAD_MUTEX_INITIALIZER` 是初始化一个快速锁的宏定义。

加锁解锁的函数为：

```c
int pthread_mutex_lock(pthread_mutex_t *mutex);
int pthread_mutex_unlock(pthread_mutex_t *mutex);
```

使用临界资源前加锁，使用完临界资源后解锁。