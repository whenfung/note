---
title: translatesAutoresizingMaskIntoConstraints
---

### translatesAutoresizingMaskIntoConstraints

- 把 autoresizingMask 转换为 Constraints
- 即：可以把 frame、bouds、center 方式布局的视图自动转化为约束形式。（此时该视图上约束已经足够 不需要手动去添加别的约束）

------

- 用代码创建的所有 view，其 translatesAutoresizingMaskIntoConstraints 默认是 YES。
- 用 IB 创建的所有 view ，autoresize 布局默认YES， autolayout 布局默认为 NO。

------

### 如何设置 translatesAutoresizingMaskIntoConstraints

- 视图使用代码创建，且为 frame 布局 ，不用去管 `translatesAutoresizingMaskIntoConstraints`
- 视图使用代码创建，且为 autolayout 布局，`translatesAutoresizingMaskIntoConstraints` 设置为 NO
- 视图 IB 创建，frame 布局，`translatesAutoresizingMaskIntoConstraints` 不用管（IB 默认为 YES）
- 视图 IB 创建，autolayout 布局，`translatesAutoresizingMaskIntoConstraints` 不用管（IB 默认为 NO）

------

### 为什么 translatesAutoresizingMaskIntoConstraints 使用约束布局时候，就要设置为 NO？

translatesAutoresizingMaskIntoConstraints 的本意是将 **frame 布局** 自动转化为 约束布局，转化的结果是为这个视图自动添加所有需要的约束，如果我们这时给视图添加自己创建的约束就一定会约束冲突。

为了避免上面说的约束冲突，我们在代码创建 **约束布局** 的控件时直接指定这个视图不能用 **frame 布局**（即translatesAutoresizingMaskIntoConstraints=NO），可以放心的去使用约束了。

------

### 例子：

- v1 是一个不使用 autolayout 的 view
- v2 是一个使用 autolayout 的 view
- 但 v1 成为 v2 的 subview 时
- v2 需要四条隐含的 constraint 来确定 v1 的位置，这些约束都是从 v1 的 frame 转化而来