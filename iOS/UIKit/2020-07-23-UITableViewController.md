---
title: UITableViewController
---

表格视图，是 iOS 中最复杂的视图之一，学会了表格视图，也就入门 UIKit 了。

## 1. 数据源

- 节里面的单元格个数

```swift
func tableView(_ tableView: UITableView, numberOfRowsInSection section: Int) -> Int {
    return 9
}
```

- 单元格设置

```swift
func tableView(_ tableView: UITableView, cellForRowAt indexPath: IndexPath) -> UITableViewCell {
    let cell: UITableViewCell
    if indexPath.row % 3 == 0 {
        cell = tableView.dequeueReusableCell(withIdentifier: "cell1", for: indexPath)
    } else if indexPath.row % 3 == 1 {
        cell = tableView.dequeueReusableCell(withIdentifier: "cell2", for: indexPath)
    } else {
        cell = tableView.dequeueReusableCell(withIdentifier: "cell3", for: indexPath)
    }
    return cell
}
```

- 删除一行后的刷新

```swift
tableView.deleteRows(at: [indexPath], with: .fade)
```

- 整体刷新

```swift
tableView.reloadData() 
```

## 2. 滑动菜单

- `commit editingStyle` : 系统自带的滑动按钮

- `leadingSwipeActionsConfigurationForRowAt` : 右滑按钮

- `trailingSwipeActionsConfigurationForRowAt` : 左滑按钮，函数内的实现例子如下：

```swift
// 删除按钮及其功能
let delAction = UIContextualAction(style: .destructive, title: "删除") { (_, _, completion) in
	self.weapons.remove(at: indexPath.row)
  	tableView.deleteRows(at: [indexPath], with: .automatic)
}

// 分享按钮及其功能
let shareAction = UIContextualAction(style: .normal, title: "分享") { (_, _, completion) in
	let text = "这是绝地求生的帅气\(self.weapons[indexPath.row].name)!"
	let image = UIImage(named: self.weapons[indexPath.row].image)!
	// 设置系统自带的分享服务控制器
	let ac = UIActivityViewController(activityItems: [text, image], applicationActivities: nil)
            
	// 平板气泡弹出
	if let pc = ac.popoverPresentationController {
		if let cell = tableView.cellForRow(at: indexPath) {
			pc.sourceView = cell
			pc.sourceRect = cell.bounds
  		}
	}
            
	self.present(ac, animated: true)
            
	completion(true)
}
shareAction.backgroundColor = .orange
let config = UISwipeActionsConfiguration(actions: [delAction, shareAction])
return config
```

- `UIContextualAction` : 滑动菜单里的按钮类型

## 3. 样式

- 背景亮度

- 单元格透明

- 分割线亮度

## 4. 单元格

- 单元格高度（也可以故事板中直接设置）

```swift
// 协议中设置
func tableView(_ tableView: UITableView, heightForRowAt indexPath: IndexPath) -> CGFloat {
    return 44
}
```

- 点击单元格高亮后立即消失

```swift
override func tableView(_ tableView: UITableView, didSelectRowAt indexPath: IndexPath) {
    print("点击了 \(indexPath)")
    // 取消高亮选中
    tableView.deselectRow(at: indexPath, animated: true)
}
```

- 单元格原型绘制（推荐故事板）

  - 在故事板中的 prototype cells 中选择单元格原型个数

  - 在生成的多个单元格中绘制样本，并给每个单元格不同的 identifier

  - 中 cellForRowAt 中使用 tableview 的 dequeueReusableCell 方法获取不同 identifier 的单元格。

  - 给原型里面的组件设置不同的 `tag`，通过 `tag` 找到对应的组件

    ```swift
    let label = cell.viewWithTag(1001) as? UILabel
    ```

    

