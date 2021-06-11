---
title: Alamofire
---

可以看作是 AFNetworking 的 swift 版本，其实是对 URL Session 的封装，当然你可以使用 URL Session，不过会比较繁琐，我建议是可以用 URL Session 学基础，当开发项目的时候使用库。

## get

外界在没有指定方法时默认为 get 方式。

```swift
let url = "https://jsonplaceholder.typicode.com/posts/1"
AF.request(url).responseJSON { response in
    switch response.result {
    // 返回的是 Any 类型，根据自己的需要进行转换
    case .success(let json):
        self.textView.text = "\(json)"
    case .failure(let error):
        self.textView.text = error.localizedDescription
    }
}
```

##  