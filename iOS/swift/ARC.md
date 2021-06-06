---
title: ARC
---

闭包调用实例的属性或方法时容易产生循环引用，导致内存泄露。我们可以在闭包中加入 `[weak self]` 或· `[unowned self]` 解决。选择方式如下：

- 在闭包和捕获的实例总是互相引用并且总是同时销毁时，将闭包内的捕获定义为 `[unowned self]`。
- 在被捕获的引用可能会变为 `nil` 时，将闭包内的捕获定义为 `[weak self]`。弱引用总是可选类型，并且当引用的实例被销毁后，弱引用的值会自动置为 `nil`。这使我们可以在闭包体内检查它们是否存在。

注意

> 如果被捕获的引用绝对不会变为 `nil`，应该用无主引用，而不是弱引用。

## 参考资料

- [自动引用计数](https://www.runoob.com/manual/gitbook/swift5/source/_book/chapter2/23_Automatic_Reference_Counting.html)

