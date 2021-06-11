---
title: Alert View
---

使用 RxSwift 和 snapkit 实现自定义弹出框。

```swift
//
//  ViewController.swift
//  Mail
//
//  Created by deng on 2020/9/4.
//  Copyright © 2020 deng. All rights reserved.
//

import UIKit
import RxSwift
import RxCocoa
import SnapKit

class ViewController: UIViewController {

    let disposeBag = DisposeBag()
    let mask = UIView()
    let alertView = UIView()
    let messageLabel = UILabel()
    let titleLabel = UILabel()
    let button1 = UIButton()
    let button2 = UIButton()

    override func viewDidLoad() {
        super.viewDidLoad()
        
//        let title = "账号已绑定"
//        let message = "该墨墨账号已绑定微信，可前往登录。"
//        buildMask(textColor: UIColor.init(named: "text")!,
//                  tintColor: UIColor.init(named: "highlight")!,
//                  title: title,
//                  message: message)
        
        let title = "提示"
        let message = "根据国家网络安全法要求，需要绑定手机再使用，如果点击跳过，将无法使用墨墨部分功能。"
        buildMask(textColor: UIColor.init(named: "alert")!,
                  tintColor: UIColor.init(named: "alert")!,
                  title: title,
                  message: message)
        
        button1.rx.tap
            .subscribe(onNext: { (Void) in
                self.mask.removeFromSuperview()
                print("取消")
            }).disposed(by: disposeBag)
        
        button2.rx.tap
        .subscribe(onNext: { (Void) in
            self.mask.removeFromSuperview()
            print("去登录")
        }).disposed(by: disposeBag)
    }

    @IBAction func openMail(_ sender: Any) {
        self.view.addSubview(self.mask)
        mask.snp.makeConstraints { (maker: ConstraintMaker) in
            maker.width.equalToSuperview()
            maker.height.equalToSuperview()
        }
    }
    
    func buildMask(textColor: UIColor, tintColor: UIColor, title: String, message: String) {
        alertView.addSubview(titleLabel)
        alertView.addSubview(messageLabel)
        alertView.addSubview(button1)
        alertView.addSubview(button2)
        mask.addSubview(alertView)
        
        mask.backgroundColor = UIColor(white: 0, alpha: 0.3)
            
        alertView.backgroundColor = .white
        alertView.layer.cornerRadius = 10
        alertView.clipsToBounds = true
        alertView.snp.makeConstraints { (maker: ConstraintMaker) in
            maker.width.equalTo(290)
            maker.centerX.equalTo(mask)
            maker.centerY.equalTo(mask).multipliedBy(0.8)
        }
        
        titleLabel.text = title
        titleLabel.font = UIFont.systemFont(ofSize: 16, weight: .bold)
        titleLabel.textColor = textColor
        titleLabel.textAlignment = .center
        titleLabel.snp.makeConstraints { (maker: ConstraintMaker) in
            maker.top.equalToSuperview()
            maker.left.equalToSuperview()
            maker.right.equalToSuperview()
            maker.height.equalTo(44)
        }
        
        // 添加下划线
        let underline = UIView()
        alertView.addSubview(underline)
        underline.backgroundColor = UIColor.init(named: "underline")
        underline.snp.makeConstraints { (maker: ConstraintMaker) in
            maker.bottom.equalTo(titleLabel.snp.bottom)
            maker.left.equalToSuperview()
            maker.right.equalToSuperview()
            maker.height.equalTo(1)
        }
        
        let paragraphStye = NSMutableParagraphStyle()//调整行间距
        paragraphStye.lineSpacing = 5
        paragraphStye.lineBreakMode = NSLineBreakMode.byWordWrapping
        
        messageLabel.attributedText = NSMutableAttributedString.init(string: message, attributes: [NSAttributedString.Key.paragraphStyle:paragraphStye])
        messageLabel.font = UIFont.systemFont(ofSize: 15)
        messageLabel.numberOfLines = 0
        messageLabel.textColor = textColor
        messageLabel.snp.makeConstraints { (maker: ConstraintMaker) in
            maker.top.equalTo(titleLabel.snp.bottom).offset(15)
            maker.left.equalToSuperview().offset(15)
            maker.right.equalToSuperview().offset(-15)
        }
            
        button1.backgroundColor = UIColor.init(named: "background")
        button1.setTitleColor(tintColor, for: .normal)
        button1.setTitle("取消", for: .normal)
        button1.layer.cornerRadius = 5
        button1.clipsToBounds = true
        button1.snp.makeConstraints { (maker: ConstraintMaker) in
            maker.top.equalTo(messageLabel.snp.bottom).offset(15)
            maker.bottom.equalToSuperview().offset(-15)
            maker.left.equalToSuperview().offset(15)
            maker.height.equalTo(44)
            maker.width.equalTo(125)
        }
            
        button2.backgroundColor = UIColor.init(named: "background")
        button2.setTitleColor(tintColor, for: .normal)
        button2.setTitle("去登录", for: .normal)
        button2.layer.cornerRadius = 5
        button2.clipsToBounds = true
        button2.snp.makeConstraints { (maker: ConstraintMaker) in
            maker.top.equalTo(messageLabel.snp.bottom).offset(15)
            maker.bottom.equalToSuperview().offset(-15)
            maker.right.equalToSuperview().offset(-15)
            maker.height.equalTo(44)
            maker.width.equalTo(125)
        }
    }
    
}
```

 封装一下变成：

```swift
//
//  ViewController.swift
//  Mail
//
//  Created by deng on 2020/9/4.
//  Copyright © 2020 deng. All rights reserved.
//

import UIKit
import RxSwift
import RxCocoa
import SnapKit

class ViewController: UIViewController {

    let disposeBag = DisposeBag()
    let mask = UIView()
    let alertView = UIView()
    let messageLabel = UILabel()
    let titleLabel = UILabel()
    let button1 = UIButton()
    let button2 = UIButton()

    override func viewDidLoad() {
        super.viewDidLoad()
        
//        let title = "账号已绑定"
//        let message = "该墨墨账号已绑定微信，可前往登录。"
//        buildMask(textColor: UIColor.init(named: "text")!,
//                  tintColor: UIColor.init(named: "highlight")!,
//                  title: title,
//                  message: message)
        
        let title = "提示"
        let message = "根据国家网络安全法要求，需要绑定手机再使用，如果点击跳过，将无法使用墨墨部分功能。"
        buildMask(textColor: UIColor.init(named: "alert")!,
                  tintColor: UIColor.init(named: "alert")!,
                  title: title,
                  message: message)
        
        button1.rx.tap
            .subscribe(onNext: { (Void) in
                self.mask.removeFromSuperview()
                print("取消")
            }).disposed(by: disposeBag)
        
        button2.rx.tap
        .subscribe(onNext: { (Void) in
            self.mask.removeFromSuperview()
            print("去登录")
        }).disposed(by: disposeBag)
    }

    @IBAction func openMail(_ sender: Any) {
        self.view.addSubview(self.mask)
        mask.snp.makeConstraints { (maker: ConstraintMaker) in
            maker.width.equalToSuperview()
            maker.height.equalToSuperview()
        }
    }
    
    func buildMask(textColor: UIColor, tintColor: UIColor, title: String, message: String) {
        alertView.addSubview(titleLabel)
        alertView.addSubview(messageLabel)
        alertView.addSubview(button1)
        alertView.addSubview(button2)
        mask.addSubview(alertView)
        
        mask.backgroundColor = UIColor(white: 0, alpha: 0.3)
            
        alertView.backgroundColor = .white
        alertView.layer.cornerRadius = 10
        alertView.clipsToBounds = true
        alertView.snp.makeConstraints { (maker: ConstraintMaker) in
            maker.width.equalTo(290)
            maker.centerX.equalTo(mask)
            maker.centerY.equalTo(mask).multipliedBy(0.8)
        }
        
        titleLabel.text = title
        titleLabel.font = UIFont.systemFont(ofSize: 16, weight: .bold)
        titleLabel.textColor = textColor
        titleLabel.textAlignment = .center
        titleLabel.snp.makeConstraints { (maker: ConstraintMaker) in
            maker.top.equalToSuperview()
            maker.left.equalToSuperview()
            maker.right.equalToSuperview()
            maker.height.equalTo(44)
        }
        
        // 添加下划线
        let underline = UIView()
        alertView.addSubview(underline)
        underline.backgroundColor = UIColor.init(named: "underline")
        underline.snp.makeConstraints { (maker: ConstraintMaker) in
            maker.bottom.equalTo(titleLabel.snp.bottom)
            maker.left.equalToSuperview()
            maker.right.equalToSuperview()
            maker.height.equalTo(1)
        }
        
        let paragraphStye = NSMutableParagraphStyle()//调整行间距
        paragraphStye.lineSpacing = 5
        paragraphStye.lineBreakMode = NSLineBreakMode.byWordWrapping
        
        messageLabel.attributedText = NSMutableAttributedString.init(string: message, attributes: [NSAttributedString.Key.paragraphStyle:paragraphStye])
        messageLabel.font = UIFont.systemFont(ofSize: 15)
        messageLabel.numberOfLines = 0
        messageLabel.textColor = textColor
        messageLabel.snp.makeConstraints { (maker: ConstraintMaker) in
            maker.top.equalTo(titleLabel.snp.bottom).offset(15)
            maker.left.equalToSuperview().offset(15)
            maker.right.equalToSuperview().offset(-15)
        }
            
        button1.backgroundColor = UIColor.init(named: "background")
        button1.setTitleColor(tintColor, for: .normal)
        button1.setTitle("取消", for: .normal)
        button1.layer.cornerRadius = 5
        button1.clipsToBounds = true
        button1.snp.makeConstraints { (maker: ConstraintMaker) in
            maker.top.equalTo(messageLabel.snp.bottom).offset(15)
            maker.bottom.equalToSuperview().offset(-15)
            maker.left.equalToSuperview().offset(15)
            maker.height.equalTo(44)
            maker.width.equalTo(125)
        }
            
        button2.backgroundColor = UIColor.init(named: "background")
        button2.setTitleColor(tintColor, for: .normal)
        button2.setTitle("去登录", for: .normal)
        button2.layer.cornerRadius = 5
        button2.clipsToBounds = true
        button2.snp.makeConstraints { (maker: ConstraintMaker) in
            maker.top.equalTo(messageLabel.snp.bottom).offset(15)
            maker.bottom.equalToSuperview().offset(-15)
            maker.right.equalToSuperview().offset(-15)
            maker.height.equalTo(44)
            maker.width.equalTo(125)
        }
    }
    
}
```

