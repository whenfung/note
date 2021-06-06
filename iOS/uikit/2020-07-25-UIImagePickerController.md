---
title: UIImagePickerController
---

一个 APP 最基本的功能之一就是用户可以自己设置自己的头像，这就涉及到照片或者视频的获取。

首先我们得在 `info.list` 设置字符串，当触及拍照权限等等时，会弹出相应提示，这里包括

```markdown
Privacy - Photo Library Usage Description : 需要打开相册
Privacy - Camera Usage Description : 需要使用摄像头来拍照
Privacy - Microphone Usage Description : 录制视频时需要打开麦克风
```

然后就可以愉快地使用 `UIImagePickerController` 了。

- 选择已有照片

```swift
// guard 和 if 类似，打不开相册就返回
guard UIImagePickerController.isSourceTypeAvailable(.photoLibrary) else {
    print("相册授权不成功")
    return
}
                
let picker = UIImagePickerController()
picker.sourceType = .photoLibrary
                
picker.delegate = self
self.present(picker, animated: true, completion: nil)
```

- 拍照

```swift
// guard 和 if 类似，打不开相册就返回
guard UIImagePickerController.isSourceTypeAvailable(.photoLibrary) else {
	return
}
                
let picker = UIImagePickerController()
picker.sourceType = .camera
            
picker.delegate = self
self.present(picker, animated: true, completion: nil)
```

- 选择录像

```swift
// guard 和 if 类似，打不开相册就返回
guard UIImagePickerController.isSourceTypeAvailable(.photoLibrary) else {
    return
}
                
let picker = UIImagePickerController()
picker.mediaTypes = [kUTTypeMovie as String]
                
picker.delegate = self
self.present(picker, animated: true, completion: nil)
```

- 录像

```swift
// guard 和 if 类似，打不开相册就返回
guard UIImagePickerController.isSourceTypeAvailable(.photoLibrary) else {
    return
}
                
let picker = UIImagePickerController()
picker.sourceType = .camera
                
picker.mediaTypes = [kUTTypeMovie as String]
picker.videoQuality = .typeHigh
picker.videoMaximumDuration = 10
                
picker.delegate = self
self.present(picker, animated: true, completion: nil)
```

`UIImagePickerController` 有相关的协议需要实现：

```swift
func imagePickerController(_ picker: UIImagePickerController, didFinishPickingMediaWithInfo info: [UIImagePickerController.InfoKey : Any]) {
    let mediaType = info[UIImagePickerController.InfoKey.mediaType] as! String
    if mediaType == (kUTTypeMovie as String) {
        videoUrl = info[UIImagePickerController.InfoKey.mediaURL] as? URL
            
        let asset = AVAsset(url:videoUrl)
            
        // 图片生成
        let gen = AVAssetImageGenerator(asset: asset)
            
        // 第二秒的截图
        let time = CMTime(seconds: 0, preferredTimescale: 2)
            
        let image = try! gen.copyCGImage(at: time, actualTime: nil)
        bgImage.image = UIImage(cgImage: image)
            
        playButton.isHidden = false
            
    } else {
        bgImage.image = info[UIImagePickerController.InfoKey.originalImage] as? UIImage
    }
        
    picker.dismiss(animated: true, completion: nil)
}
```

