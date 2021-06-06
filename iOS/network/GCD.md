---
title: GCD
---

Grand Central Dispatch（GCD）是 Apple 开发的一个多核编程方案。

- GCD 有啥用？

  现在的设备具有多核，多个任务可并行，加快处理速度。

## 基本概念

什么是任务、队列、线程？

| 专业名词 | 解释     |
| -------- | -------- |
| 任务     | 任务就是 |
| 队列     |          |
| 线程     |          |

## 想法

主队列 + 同步执行 = 死锁

是因为代码默认是在主线程执行的，如果同步执行，后来的任务会等待代码执行完，同时代码完成一段后，又回到队列后面等待任务，导致互相等待，发生死锁。

## Swift GCD

```swift
// GCD 主线程/子线程
DispatchQueue.main.asyncAfter(deadline: .now() + 3) {
    self.delayExecution()
}

DispatchQueue.global().asyncAfter(deadline: .now() + 3) {
    self.delayExecution()
}
```

## 参考文献

- <https://www.cnblogs.com/allencelee/p/6023213.html>

- <https://www.jianshu.com/p/2b46e5f91743>