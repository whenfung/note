---
title: delegation
---

协议作为代理：代理是一种常见的设计模式，可以让类或结构体把一部分职责，指派给非同类的实例去承担。

- 若要寻求代理，可以通过定义一个协议，打包要实现的职责于其中。
- 该代理协议的遵从者就可以实现这些打包的职责。
- 代理在 iOS 开发中，一般可以用于响应特定的操作，或从外部数据源取回数据但无需了解是何种数据源。

例如土豪玩家请代理刷级

```swift
import UIKit

struct Role {
    var name : String
}

// 角色
protocol Player {
    var role : Role { get }
    mutating func play()
}

// 代练
protocol GameDelegate {
    func start(with player: Player) -> Int
    func rolePK(with player: Player, armed: Bool) -> Int
    func end(with player: Player) -> Int
}

// 代练方法实现
struct GameAgent: GameDelegate {
    func start(with player: Player) -> Int {
        print(player.role.name, "开始进入游戏，获得经验 2000")
        return 2000
    }
    
    func rolePK(with player: Player, armed: Bool) -> Int {
        if armed {
            print(player.role.name, "开始 PK，您有武器，获得经验 5000")
            return 5000
        } else {
            print(player.role.name, "开始 PK，没有武器，获得经验 2500")
            return 2500
        }
    }
    func end(with player: Player) -> Int {
        print(player.role.name, "正常退出，获取经验 1000")
        return 1000
    }
}

struct MirPlayer: Player {
    var exp:Int
    var gameAgent:GameAgent?
    var role: Role
    mutating func play() {
        if let gameAgent = gameAgent {
            print("您使用了代练！")
            exp += gameAgent.start(with: self)
            exp += gameAgent.rolePK(with: self, armed: true)
            exp += gameAgent.end(with: self)
        } else {
            print("您没有使用任何代练，不能自动升级")
        }
    }
}

var role = Role(name: "deng")
var player1 = MirPlayer(exp: 0, gameAgent: nil, role: role)
player1.play()

var role2 = Role(name: "土豪玩家")
let agent = GameAgent()
var player2 = MirPlayer(exp: 0, gameAgent: agent, role: role2)
player2.play()
```



