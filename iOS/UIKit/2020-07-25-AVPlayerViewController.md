---
title: AVPlayerViewController
---

苹果提供了专门的控制器播放视频，可以在故事板中构建，不过代码构建更简单。

```swift
// 生成播放视频的控制器
let playerVC = AVPlayerViewController()
// 设置播放视频的地址
playerVC.player = AVPlayer(url: videoUrl)
        
// 显示控制器并播放视频
self.present(playerVC, animated: true) {
	playerVC.player?.play()
}
```

