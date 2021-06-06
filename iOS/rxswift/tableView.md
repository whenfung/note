---
title: TableView
---

使用 RxCocoa 时，需要将原先的代理设置为空，不然会报错。这个例子我们使用 MVVM 框架实现。

## 1. Model

采用 <http://jsonplaceholder.typicode.com/posts> 的数据，那么 Model 的结构如下：

```swift
import Foundation

struct Post: Codable {
    let userId: Int
    let id: Int
    let title: String
    let body: String
}
```

## 2. ViewModel

该模块主要实现业务逻辑：

```swift
import Foundation
import RxSwift

class ViewModel {
    
    var postsSub: BehaviorSubject<[Post]> = BehaviorSubject(value: [])
    
    func getPosts() {
        let jsonUrlString = "http://jsonplaceholder.typicode.com/posts"
        guard let url = URL(string: jsonUrlString) else { return }

        URLSession.shared.dataTask(with: url) {
            (data, response, error) in
            guard let data = data else { return }
            do {
                let posts = try JSONDecoder().decode([Post].self, from: data)
                self.postsSub.onNext(posts)
            } catch let jsonErr {
                print("json 解析出错 : ", jsonErr)
            }
        }.resume()
    }
    
    func getPost() {
        let jsonUrlString = "http://jsonplaceholder.typicode.com/posts/1"
        guard let url = URL(string: jsonUrlString) else { return }

        URLSession.shared.dataTask(with: url) {
            (data, response, error) in
            guard let data = data else { return }
            do {
                let post = try JSONDecoder().decode(Post.self, from: data)
                self.postsSub.onNext([post])
            } catch let jsonErr {
                print("json 解析出错 : ", jsonErr)
            }
        }.resume()
    }
}
```

## 3. Controller

把一个 UITableView 加入到页面中，然后使用 RxSwift 来实现数据的绑定和刷新。

```swift
import UIKit
import RxSwift
import RxCocoa

class ViewController: UIViewController {
    
    @IBOutlet weak var tableView: UITableView!
    
    let disposeBag = DisposeBag()
    let viewModel = ViewModel()
    
    override func viewDidLoad() {
        super.viewDidLoad()
        
    }
    
    override func viewDidAppear(_ animated: Bool) {
        
        // 绑定数据
        viewModel.postsSub.asObservable()
            .bind(to: tableView.rx.items(cellIdentifier: "cell", cellType: UITableViewCell.self)) {
                row, post, cell in
                cell.textLabel?.text = post.title
        }.disposed(by: disposeBag)
        
        // 监听点击事件
        tableView.rx.itemSelected
            .subscribe(onNext: { (indexPath: IndexPath) in
                print(indexPath)
            }).disposed(by: disposeBag)
        
        // 监听点击单元格获取模型
        tableView.rx.modelSelected(Post.self).subscribe(onNext: { (post: Post) in
            print(post.body)
            }).disposed(by: disposeBag)
        
        viewModel.getPost()
    }
    
    @IBAction func updateData(_ sender: Any) {
        viewModel.getPosts()
    }
    
}
```

## 补充

业务逻辑分离：TableView 监听数据源，数据源可以监听搜索框的文本变化，作出筛选，这样就形成了链式反应。

