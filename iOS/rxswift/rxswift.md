---
title: RxSwift
---

## Target Action

传统实现方法：

```swift
button.addTarget(self, action: #selector(buttonTapped), for: .touchUpInside)
```

```swift
func buttonTapped() {
    print("button Tapped")
}
```

通过 Rx 来实现：

```swift
let disposeBag = DisposeBag()
button.rx.tap
.subscribe(onNext: {
    print("button Tapped")
})
.disposed(by: disposeBag)
```

## 代理

传统实现方法：

```swift
class ViewController: UIViewController {
    ...
    override func viewDidLoad() {
        super.viewDidLoad()
        scrollView.delegate = self
    }
}

extension ViewController: UIScrollViewDelegate {
    func scrollViewDidScroll(_ scrollView: UIScrollView) {
        print("contentOffset: \(scrollView.contentOffset)")
    }
}
```

通过 Rx 来实现：

```swift
class ViewController: UIViewController {
    
    private let disposeBag = DisposeBag()
    @IBOutlet weak var scrollView: UIScrollView!
    ...
  
		override func viewDidLoad() {
        super.viewDidLoad()

        scrollView.rx.contentOffset
            .subscribe(onNext: { contentOffset in
                print("contentOffset: \(contentOffset)")
            })
            .disposed(by: disposeBag)
    }
}
```

你不需要书写代理的配置代码，就能获得想要的结果。

## 闭包回调

传统实现方法：

```swift
let url = URL(string: "https://professordeng.com")
URLSession.shared.dataTask(with: URLRequest(url: url!)) {
    (data, response, error) in
    guard error == nil else {
        print("Data Task Error: \(error!)")
        return
    }
    
    guard let data = data else {
        print("Data Task Error: unknown")
        return
    }
    
    print("Data Task Success with count: \(data.count)")
}.resume()
```

通过 Rx 来实现：

```swift
let url = URL(string: "https://professordeng.com")

URLSession.shared.rx.data(request: URLRequest(url: url!))
    .subscribe(onNext: { data in
        print("Data Task Success with count: \(data.count)")
    }, onError: { error in
        print("Data Task Error: \(error)")
    })
    .disposed(by: disposeBag)
```

回调也变得十分简单。

