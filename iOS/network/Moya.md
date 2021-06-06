---
title: Moya
---

首先得配置 Xcode 允许 HTTP 请求：

在 info.plist 里面增加：

App Transport Security Settings 属性，

再在此属性内增加 Allow Arbitrary Loads ，并设置值为 YES。