---
title: WKWebView
---

一般 App 都有隐私政策，我们可以使用一个网页实现，然后在 App 中直接使用 WKWebView 将网页显示出来即可。

要使用 WKWebView 首先得将包含 WKWebView 的框架加入到项目中，步骤如下：

- 选中左侧栏项目 ➡️ 选中 TARGETS ➡️ 选中 General 栏 ➡️ 在 Frameworks 中添加 `WebKit.framework`。

这样，就完成了 WKWebView 的添加。

使用的时候需要导入该库，使用 `import WebKit` 即可。

## 1. 简单使用

故事板支持直接拖拽，然后我们在 ViewController 中添加如下代码：

```swift
import UIKit
import WebKit

class ViewController: UIViewController, WKNavigationDelegate {

    @IBOutlet weak var webView: WKWebView!
    
    override func viewDidLoad() {
        super.viewDidLoad()
        
        webView.navigationDelegate = self
        
        let url = URL(string: "https://professordeng.com")
        let request = URLRequest(url: url!)
        webView.load(request)
    }
    
    func webView(_ webView: WKWebView, didStartProvisionalNavigation navigation: WKNavigation!) {
        print("开始加载")
    }
    
    func webView(_ webView: WKWebView, didCommit navigation: WKNavigation!) {
        print("内容开始返回")
    }
    
    func webView(_ webView: WKWebView, didFinish navigation: WKNavigation!) {
        print("加载完成")
    }
    
    func webView(_ webView: WKWebView, didFail navigation: WKNavigation!, withError error: Error) {
        print("加载失败")
    }
}
```

其中， WKNavigationDelegate 主要与 Web 视图界面加载过程有关。

## 2. WKUIDelegate

WKUIDelegate 主要与 Web 视图解码显示和提示框相关。

## 3. 进度条实现

直接在最上方放个 UIProgressView 实现即可，在不同的加载时期显示不同的进度即可。

## 4. 加载圈实现

将 ActivityIndicatorView 放中间即可，当加载完后消失即可。

