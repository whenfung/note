---
title: Label
---

我们可以给标签 `forgetPasswordLabel` 添加点击事件。

```swift
func addTopToForgetPasswordLabel() -> Void {
    let click = UITapGestureRecognizer()
    
    // 点击跳转到忘记密码页面
    click.rx.event.asObservable().subscribe(onNext: {
        recognizer in
        
        let fpc = UIStoryboard.viewController(ForgetPasswordController.self)
        self.navigationController?.pushViewController(fpc, animated: true)
    }).disposed(by: disposeBag)
    forgetPasswordLabel.addGestureRecognizer(click)
    forgetPasswordLabel.isUserInteractionEnabled = true
}
```

