---
title: just
---

创建 `Observable` 发出唯一的一个元素。

```swift
let button = UIButton()
view.addSubview(button)

button.setTitle("获取验证码", for: .normal)
button.backgroundColor = .lightGray

button.translatesAutoresizingMaskIntoConstraints = false
NSLayoutConstraint.activate([
    button.centerXAnchor.constraint(equalTo: view.centerXAnchor),
    button.centerYAnchor.constraint(equalTo: view.centerYAnchor),
    button.widthAnchor.constraint(equalToConstant: 290),
    button.heightAnchor.constraint(equalToConstant: 44)
])

button.rx.tap
    .flatMap({ _ -> Observable<Int> in
        return Observable.just(1)
    })
    .subscribe(onNext: { element in
        print("element: \(element)")
    }).disposed(by: disposeBag)
```

使用 `flatMap` 将每一个点击事件转换为包含 1 的序列，然后将这些序列合并为一个序列。