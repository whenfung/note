---
title: URLSession
---

Alamofire 是对 URLSession 封装后的库，正如 AFNetworking 是对 NSURLSession 封装后的库。一般网络请求分三个步骤。

## 1. 设置 URL

```swift
// 设置 URL 地址
// guard 表示如果失败，则直接返回
guard let url = URL.init(string: "https://professordeng/login") else { return } 
```

其中，上述 URL 包括如下几个部分：

- 协议：https 协议
- 主机：服务器地址 `ip` 或绑定 `ip` 的域名。

## 2. 设置网络请求

```swift
var request = URLRequest.init(url: url)
request.httpMethod = "POST"
request.setValue("application/json", forHTTPHeaderField: "Content-Type")
let postData = ["identity":"qq@qq.com", "password" : "123456"]
request.httpBody = try?JSONSerialization.data(withJSONObject: postData, options: [])
request.timeoutInterval = 5
request.cachePolicy = .useProtocolCachePolicy
```

## 3. 发起请求

```swift
URLSession.shared.dataTask(with: request) { (data, response, error) in
    print("********网络请求***********")
    do {
        let list = try JSONSerialization.jsonObject(with: data!, options: .allowFragments)
        print(list)
    } catch {
        print(error)
    }
}.resume()
```

- `URLSession.shared` 为全局共享单例会话对象。

如果你想创建一个会话，可以使用如下代码：

```swift
let configuration = URLSessionConfiguration.background(withIdentifier: "request_id")
let session = URLSession.init(configuration: configuration, delegate: self, delegateQueue: OperationQueue.main)
session.dataTask(with: request) { (data, response, error) in
    print("*******网络请求*******")
    do {
        let list =  try JSONSerialization.jsonObject(with: data!, options: .allowFragments)
        print(list)
    }catch{
        print(error)
    }
}.resume()
```

- 设置一个唯一会话标识，通过标识来区分不同的会话任务。
- 遵循 `URLSessionDelegate` 代理，实现代理方法，在本类中监控任务进度。



