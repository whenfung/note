---
title: animation
---

动画是多个帧的图片的播放。`UIKit` 提供 `UIViewPropertyAnimator` 实现。iOS 中创建精致的动画效果不需要写复杂的代码。

你只需要知道一个简单的方法即可，它位于 UIView 类中。

```swift
UIView.animate(withDutation: TimeInterval, animations: () -> Void)
```

我们只要在 `UIView.animate` 中指定首帧和尾帧，中间的帧系统会自动帮我们计算。

## 1. 显示动画

- 设置动画起始位置，移动动画。

  ```swift
  let startPosition = CGAffineTransform(translationX: 500, y: 0)
  ```

- 设置动画开始尺寸（原尺寸的 10 倍）

  ```swift
  let startScale = CGAffineTransform(scaleX: 10, y: 10)
  ```

- 将动画起始位置和起始尺寸赋给按钮，并设置起始透明度为 100%

  ```swift
  // 也许 startPosition.concatenating(startScale) 和 startScale.concatenating(startPosition) 的效果不一样，自己试试
  button.transform = startPosition.concatenating(startScale)
  button.alpha = 0
  ```

- 在需要的时候启动动画效果，元素会在 0.4 秒内将变化以动画的形式展开

  ```swift
  // dampingRatio 是子弹的反弹效果
  UIViewPropertyAnimator(duration: 0.4, dampingRatio: 0.5) {
  	button.alpha = 1
  	button.transform = .identity
  }.startAnimation(afterDelay: 0.1)
  ```

## 2. 配合手势动画

给一个组件添加拖拽手势，然后我们可以根据拖拽手势做一些炫酷的动画（例如摇摆效果）。如果组件没有拖拽手势，可以在故事板中添加。

```swift
@IBAction func dragStackView(_ sender: UIPanGestureRecognizer) {
    switch sender.state {
    case .changed:
        // 将手势转化为当前视图的坐标
        let translation = sender.translation(in: view)
        // 位置跟随鼠标（手）移动
        let position = CGAffineTransform(translationX: translation.x, y: translation.y)
        // 歪了的效果
        let angle = sin(translation.x / stackView.frame.width)
        stackView.transform = position.rotated(by: angle)
        
    case .ended:
        // 还原操作
        UIViewPropertyAnimator(duration: 0.5, dampingRatio: 0.5) {
            self.stackView.transform = .identity
        }.startAnimation()
    default:
        break
    }
}
```

# spring animation

震荡效果的动画，可以先将组件大小设置为 0。

```swift
let startPosition = CGAffineTransform(scaleX: 0, y: 0)
```

然后在合适的时间调用如下函数：

```swift
UIView.animate(withDuration: 0.3, delay: 0, usingSpringWithDamping: 0.3, initialSpringVelocity: 0.5, options: [], animations: {
	self.ratingStackView.transform = CGAffineTransform.identity
}, completion: nil)
```