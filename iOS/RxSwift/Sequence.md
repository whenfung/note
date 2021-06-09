---
title: 可监听序列
---

RxSwift 把所有发生的事件放到序列中，这些事件都是有顺序的，而我们只需要监听该序列是否有事件产生。

## 1. 创建序列

如下，我们创建一个序列，并放了 10 个事件进去，只要我们监听该序列，我们就可以响应里面的每一个事件。

```swift
// 创建序列，事件源是 Int
let numbers: Observable<Int> = Observable.create { observer -> Disposable in

    observer.onNext(0)
    observer.onNext(1)
    observer.onNext(2)
    observer.onNext(3)
    observer.onNext(4)
    observer.onNext(5)
    observer.onNext(6)
    observer.onNext(7)
    observer.onNext(8)
    observer.onNext(9)
    observer.onCompleted()

    return Disposables.create()
}
```

如下就是监听代码：

```swift
numbers.subscribe { (event: Event<Int>) in
    print(event)
}.disposed(by: disposeBag)
```

## 2. 网络事物监听

序列监听：监听里面是否有事件发生，如果有的话那么就作出响应，这在网络处理上有得天独厚的优势。

```swift
let json: Observable<JSON> = Observable.create { (observer) -> Disposable in
    
    let task = URLSession.shared.dataTask(with: URL(string: "https://jsonplaceholder.typicode.com/posts/1")!) { data, response, error in
        
        guard error == nil else {
            observer.onError(error!)
            return
        }
        
        guard let data = data,
            let jsonObject = try? JSONSerialization.jsonObject(with: data, options: .mutableLeaves) else {
                return
        }
        
        observer.onNext(jsonObject)
        observer.onCompleted()
    }
    
    task.resume()
    
    return Disposables.create { task.cancel() }
}
```

序列创建好后，在合适的地方订阅，每订阅一次就会监听序列直到完成。

```swift
json.subscribe(onNext: { (json) in
        print(json)
    }, onError: { (json) in
        print(json)
    }, onCompleted: {
        print("网络请求完成")
    }).disposed(by: disposeBag)
```

也就是说，每触发一次就订阅一次（例如点击更新按钮）。

`subscribe` 后面的 `onNext`、`onError`、 `onCompleted` 分别响应我们创建 json 时，构建函数里面的 `onNext`、`onError`、 `onCompleted` 事件。我们称这些事件为 Event：

```swift
public enum Event<Element> {
    case next(Element)
    case error(Swift.Error)
    case completed
}
```

## 3. Single

**Single** 是 `Observable` 的另外一个版本。不像 `Observable` 可以发出多个元素，它要么只能发出一个元素，要么产生一个 `error` 事件。

- 发出一个元素，或一个 `error` 事件
- 不会共享附加作用

一个比较常见的例子就是执行 HTTP 请求，然后返回一个**应答**或**错误**。不过你也可以用 **Single** 来描述任何只有一个元素的序列。

### 3.1 创建 single

创建 **Single** 和创建 **Observable** 非常相似：

```swift
func getRepo(_ repo: String) -> Single<[String: Any]> {

    return Single<[String: Any]>.create { single in
        let url = URL(string: "https://api.github.com/repos/\(repo)")!
        let task = URLSession.shared.dataTask(with: url) {
            data, _, error in

            if let error = error {
                single(.error(error))
                return
            }

            guard let data = data,
                  let json = try? JSONSerialization.jsonObject(with: data, options: .mutableLeaves),
                  let result = json as? [String: Any] else {
                single(.error(DataError.cantParseJSON))
                return
            }

            single(.success(result))
        }

        task.resume()

        return Disposables.create { task.cancel() }
    }
}
```

### 3.2 使用 Single

之后，我们可以使用 Single：

```swift
getRepo("ReactiveX/RxSwift")
    .subscribe(onSuccess: { json in
        print("JSON: ", json)
    }, onError: { error in
        print("Error: ", error)
    })
    .disposed(by: disposeBag)
```

订阅提供一个 `SingleEvent` 的枚举：

```swift
public enum SingleEvent<Element> {
    case success(Element)
    case error(Swift.Error)
}
```

你还可以使用 `asSingle()` 将 `Observable` 转换为 Single。

```swift
import UIKit
import RxSwift
import RxCocoa

class ViewController: UIViewController {
    
    private let disposeBag = DisposeBag()
    
    override func viewDidLoad() {
        super.viewDidLoad()

        Observable.of("1").asSingle()
            .subscribe(onSuccess: { text in
                print(text)
            }) { (error: Error) in
                print(error)
        }.disposed(by: disposeBag)
        
    }
}
```

## 4. Completable

**Completable** 是 `Observable` 的另外一个版本。不像 `Observable` 可以发出多个元素，它要么只能产生一个 `completed` 事件，要么产生一个 `error` 事件。

- 发出零个元素
- 发出一个 `completed` 事件或者一个 `error` 事件
- 不会共享附加作用

**Completable** 适用于那种你只关心任务是否完成，而不需要在意任务返回值的情况。它和 `Observable<Void>` 有点相似。

### 4.1 如何创建 Completable

创建 **Completable** 和创建 **Observable** 非常相似：

```

```



