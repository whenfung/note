---
title: NSURLSession
---

objc 网络通信的基础就是 NSURLSession。一般网络请求分为以下几步：

## 1. 设置 URL

```objc
NSURL *url = [NSURL URLWithString: @"https://professordeng.com/login"];
```

## 2. 设置网络请求 

```objc
// 初始化请求
NSMutableURLRequest *request = [NSMutableURLRequest requestWithURL:url];
   
// 设置请求方法为 POST 请求
[request setHTTPMethod:@"POST"];
   
// 请求体类型是 json
[request setValue:@"application/json" forHTTPHeaderField:@"Content-Type"];
      
// 设置请求体
NSDictionary *json = @{
    @"identity" : @"test@test.com",
    @"password" : @"password"
};

// 协议数据需要封装为 NSData
NSData *postData = [NSJSONSerialization dataWithJSONObject:json options:NSJSONWritingPrettyPrinted error:nil];
   
// 将数据放到请求体中
[request setHTTPBody:postData];
```

## 3. 设置会话

```objc
// 创建默认配置对象
NSURLSessionConfiguration *defaultConfig = [NSURLSessionConfiguration defaultSessionConfiguration];
   
// 5 秒请求超时
defaultConfig.timeoutIntervalForRequest = 5.0;
   
   
// 初始化会话，配置就是默认会话，放到主线程
NSURLSession *session = [NSURLSession sessionWithConfiguration:defaultConfig delegate:nil delegateQueue:[NSOperationQueue mainQueue]];
```

## 4. 设置请求完成后的任务

```objc
// 设置请求任务，包括请求成功后需要做的事
NSURLSessionDataTask *task = [session dataTaskWithRequest:request completionHandler:^(NSData *data, NSURLResponse *response, NSError *error) {
    if (!error) {
        NSHTTPURLResponse *httpResponse = (NSHTTPURLResponse *)response;
        if ([httpResponse statusCode] != 200) {
            NSLog(@"登录失败");
        }
           
        // 转换为字典
        NSDictionary *loginMessage = [NSJSONSerialization JSONObjectWithData:data options:NSJSONReadingAllowFragments error:nil];
        NSLog(@"%@", loginMessage);
    } else {
        NSLog(@"error : %@", error.localizedDescription);
    }
}];

// 发起任务
[task resume];
```

## 总结

其他类型的网络请求大同小异，这里就不一一介绍了。网络请求主要有以下几步：

1. 配置网络请求，例如内容、方式等等。
2. 配置会话，例如会话超时设置等等。
3. 配置会话完成后的处理，例如保存网络数据等等。

