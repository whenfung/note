---
title: curl
---

curl 是测试后端接口的命令行工具，如果你喜欢图形化工具可以使用 postman。

格式如下：

```bash'
curl -X ... -H ... -d ... url
```

参数有：

`-X` ：方法 method，例如

```bash
-X POST
```

`-H` ：头部字段，多个字段需要多个 `-H`，例如：

```bash
-H "Content-Type: application/json"
```

`-d` ：body 字段，例如：

```bash
-d '{"type" : "apple"}'
```

## 参考资料

- [curl 的用法指南](http://www.ruanyifeng.com/blog/2019/09/curl-reference.html)