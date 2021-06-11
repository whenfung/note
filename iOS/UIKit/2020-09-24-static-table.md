---
title: static table
---

如果想要实现表格圆角的效果，可以使用下面的拓展：

```swift
override func tableView(_ tableView: UITableView, willDisplay cell: UITableViewCell, forRowAt indexPath: IndexPath) {
    // 圆角
    let cornerRadius = 10.0
    // 大小
    let bounds = CGRect(x: 15, y: 0, width: cell.bounds.width - 30, height: cell.bounds.height)
    // 行数
    let numberOfRows = tableView.numberOfRows(inSection: indexPath.section)
    // 绘制曲线
    var bezierPath: UIBezierPath!
    if indexPath.row == 0 && numberOfRows == 1 {
        // section 中只有一个单元格，四个角都是圆角
        bezierPath = UIBezierPath(roundedRect: bounds,
                                  byRoundingCorners: .allCorners,
                                  cornerRadii: CGSize(width: cornerRadius, height: cornerRadius))
    } else if indexPath.row == 0 {
        // section 中的第一行，左上、右上角为圆角
        bezierPath = UIBezierPath(roundedRect: bounds,
                                  byRoundingCorners: [.topLeft, .topRight],
                                  cornerRadii: CGSize(width: cornerRadius, height: cornerRadius))
    } else if indexPath.row == numberOfRows - 1 {
        // section 中的最后一行，左下，右下角为圆角
        bezierPath = UIBezierPath(roundedRect: bounds,
                                  byRoundingCorners: [.bottomLeft, .bottomRight],
                                  cornerRadii: CGSize(width: cornerRadius, height: cornerRadius))
    } else {
        bezierPath = UIBezierPath(rect: bounds)
    }
    
    // cell 的背景色透明
    cell.backgroundColor = .clear
    // 新建一个图层
    let layer = CAShapeLayer()
    // 图层边框路径
    layer.path = bezierPath.cgPath
    // 图层填充色，也就是 cell 的底色
    layer.fillColor = UIColor.white.cgColor
    
    // 将图层添加到 cell 的图层中，并插到最底层
    var removeArr = [CAShapeLayer]()
    if let layers = cell.layer.sublayers {
        for item in layers {
            if let item = item as? CAShapeLayer {
                removeArr.append(item)
            }
        }
    }
    
    if removeArr.count > 0 {
        for item in removeArr {
            item.removeFromSuperlayer()
        }
    }
    
    cell.layer.insertSublayer(layer, at: 0)
    
}
```

设置 footerView 的颜色

```swift
override func tableView(_ tableView: UITableView, willDisplayFooterView view: UIView, forSection section: Int) {
    view.tintColor = .black
}
```

设置 footerView 的高度

```swift
override func tableView(_ tableView: UITableView, heightForFooterInSection section: Int) -> CGFloat {
    return 10
}
```

在某些表格页面不需要导航栏，隐藏即可

```swift
extension ViewController: UINavigationControllerDelegate {
    func navigationController(_ navigationController: UINavigationController, willShow viewController: UIViewController, animated: Bool) {
        if viewController is MineViewController {
            navigationController.setNavigationBarHidden(true, animated: true)
        } else {
            navigationController.setNavigationBarHidden(false, animated: true)
        }
    }
}
```

## 单元格

调整单元格两边的间距

```swift
class Cell: UITableViewCell {

    override var frame: CGRect {
        get {
            super.frame
        }
        set {
            var frame = newValue
            frame.origin.x += 15
            frame.size.width -= 30
            super.frame = frame
        }
    }
    
    override func awakeFromNib() {
        super.awakeFromNib()
        // Initialization code
    }

    override func setSelected(_ selected: Bool, animated: Bool) {
        super.setSelected(selected, animated: animated)

        // Configure the view for the selected state
    }

}
```

