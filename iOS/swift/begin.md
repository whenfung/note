---
title: 快速开始
---

本教程适合有编程基础的人（会一门编程，会数据结构即可）。

- 输出 hello world

```swift
print("Hello, world!")
```

## 1. 简单值

- 常量

```swift
let myConstant = 42
```

- 变量

```swift
var myVariable = 42
```

- 声明变量类型

```swift
let explicitDouble: Double = 70
```

- 显式转换

```swift
let label = "The width is"
let width = 94
let widthLabel = label + String(width)
```

- 值转字符串

```swift
let apples = 3
let oranges = 5
let appleSummary = "I have \(apples) apples."
let fruitSummary = "I have \(apples + oranges) pieces of fruit."
```

- 多行字符串内容

```swift
let quotation = """
I said "I have \(apples) apples."
And then I said "I have \(apples + oranges) pieces of fruit."
"""
```

- 数组和字典

```swift
var shoppingList = ["catfish", "water", "tulips", "blue paint"]
shoppingList[1] = "bottle of water"

var occupations = [
    "Malcolm": "Captain",
    "Kaylee": "Mechanic",
]
occupations["Jayne"] = "Public Relations"
```

- 空数组或者空字典

```swift
let emptyArray = [String]()
let emptyDictionary = [String: Float]()
```

- 类型可被推断的空数组和空字典

```swift
shoppingList = []
occupations = [:]
```

## 2. 控制流

- 循环 + 控制

```swift
let individualScores = [75, 43, 103, 87, 12]
var teamScore = 0
for score in individualScores {
    if score > 50 {
        teamScore += 3
    } else {
        teamScore += 1
    }
}
print(teamScore)
```

- 可选值（`?` 符的使用）

```swift
var optionalString: String? = "Hello"
print(optionalString == nil)

var optionalName: String? = "John Appleseed"
var greeting = "Hello!"
// name 的作用域在大括号中，若 optionalName 为 nil，则不执行
if let name = optionalName {
    greeting = "Hello, \(name)"
}
```

- 默认值（`??` 符的使用），如果可选值缺失，提供默认值

```swift
let nickName: String? = nil
let fullName: String = "John Appleseed"
// 若 nickName 为空则使用 fullName
let informalGreeting = "Hi \(nickName ?? fullName)"
```

- `switch` 支持任意类型的数据以及各种比较操作（必须要有 `default` 语句）

  运行 `switch` 中匹配到的 `case` 语句之后，程序会退出 `switch` 语句，并不会继续向下运行，所以不需要在每个子句结尾写 `break`。

```swift
let vegetable = "red pepper"
switch vegetable {
case "celery":
    print("Add some raisins and make ants on a log.")
case "cucumber", "watercress":
    print("That would make a good tea sandwich.")
case let x where x.hasSuffix("pepper"):
    print("Is it a spicy \(x)?")
default:
    print("Everything tastes good in soup.")
}
```

- `for-in` 遍历字典。

```swift
let interestingNumbers = [
    "Prime": [2, 3, 5, 7, 11, 13],
    "Fibonacci": [1, 1, 2, 3, 5, 8],
    "Square": [1, 4, 9, 16, 25],
]
var largest = 0
// 需要一对变量 (kind, numbers) 来表示每个键值对
for (kind, numbers) in interestingNumbers {
    for number in numbers {
        if number > largest {
            largest = number
        }
    }
}
print(largest)
```

- `while` 循环

```swift
var n = 2
while n < 100 {
    n *= 2
}
print(n)

var m = 2
repeat {
    m *= 2
} while m < 100
print(m)
```

- 不包含上界的下标循环

```swift
var total = 0
for i in 0..<4 {
    total += i
}
print(total)
```

- 包含上界的下标循环

```swift
var total = 0
for i in 0...4 {
    total += i
}
print(total)
```

## 3. 函数和闭包

- 使用 `func` 来声明一个函数，使用名字和参数来调用函数。使用 `->` 来指定函数返回值的类型。

```swift
// person 是参数标签，String 是参数
func greet(person: String, day: String) -> String {
    return "Hello \(person), today is \(day)."
}
greet(person:"Bob", day: "Tuesday")
```

- 使用 `_` 表示不使用参数标签。

```swift
func greet(_ person: String, on day: String) -> String {
    return "Hello \(person), today is \(day)."
}
greet("John", on: "Wednesday")
```

- 使用元组来生成复合值，比如让一个函数返回多个值。该元组的元素可以用名称或数字来获取。

```swift
func calculateStatistics(scores: [Int]) -> (min: Int, max: Int, sum: Int) {
    var min = scores[0]
    var max = scores[0]
    var sum = 0

    for score in scores {
        if score > max {
            max = score
        } else if score < min {
            min = score
        }
        sum += score
    }

    return (min, max, sum)
}
let statistics = calculateStatistics(scores:[5, 3, 100, 3, 9])
print(statistics.sum)
print(statistics.2)
```

- 函数嵌套，被嵌套的函数可以访问外侧函数的变量，你可以使用嵌套函数来重构一个太长或者太复杂的函数。

```swift
func returnFifteen() -> Int {
    var y = 10
  	// add 函数可访问 y
    func add() {
        y += 5
    }
    add()
    return y
}
returnFifteen()
```

- 函数是第一等类型，这意味着函数可以作为另一个函数的返回值。

```swift
func makeIncrementer() -> ((Int) -> Int) {
    func addOne(number: Int) -> Int {
        return 1 + number
    }
    return addOne
}
var increment = makeIncrementer()
increment(7)
```

- 函数也可以当做参数传入另一个函数

```swift
func hasAnyMatches(list: [Int], condition: (Int) -> Bool) -> Bool {
    for item in list {
        if condition(item) {
            return true
        }
    }
    return false
}
func lessThanTen(number: Int) -> Bool {
    return number < 10
}
// 7 < 10，返回 true
var numbers = [20, 19, 7, 12]
hasAnyMatches(list: numbers, condition: lessThanTen)
```

- 函数实际上是一种特殊的闭包：它是一段能之后被调取的代码。闭包中的代码能访问闭包作用域中的变量和函数，即使闭包是在一个不同的作用域被执行的 - 你已经在嵌套函数的例子中看过了。你可以使用 `{}` 来创建一个匿名闭包。使用 `in` 将参数和返回值类型的声明与闭包函数体进行分离。

```swift
var numbers = [20, 19, 7, 12]
numbers.map({
    // number: Int 表示参数标签:参数
    (number: Int) -> Int in
    // 接下来的就是闭包函数体
    let result = 3 * number
    return result
})
```

- 有很多种创建更简洁的闭包的方法。如果一个闭包的类型已知，比如作为一个代理的回调，你可以忽略参数，返回值，甚至两个都忽略。单个语句闭包会把它语句的值当做结果返回。

```swift
// 忽略了参数和返回类型
let mappedNumbers = numbers.map({ number in 3 * number })
print(mappedNumbers)
```

- 你可以通过参数位置而不是参数名字来引用参数，这个方法在非常短的闭包中非常有用。当一个闭包作为最后一个参数传给一个函数的时候，它可以直接跟在括号后面。当一个闭包是传给函数的唯一参数，你可以完全忽略括号。

```swift
// 忽略小括号，其中 $0 表示第一个参数
let sortedNumbers = numbers.sorted { $0 > $1 }
print(sortedNumbers)
```

## 4. 对象和类

- 使用 `class` 和类名来创建一个类。类中属性的声明和常量、变量声明一样，唯一的区别就是它们的上下文是类。同样，方法和函数声明也一样。

```swift
class Shape {
    var numberOfSides = 0
    func simpleDescription() -> String {
        return "A shape with \(numberOfSides) sides."
    }
}
```

- 要创建一个类的实例，在类名后面加上括号。使用点语法来访问实例的属性和方法。

```swift
var shape = Shape()
shape.numberOfSides = 7
var shapeDescription = shape.simpleDescription()
```

- 这个版本的 `Shape` 类缺少了一些重要的东西：一个构造函数来初始化类实例。使用 `init` 来创建一个构造器。

```swift
class NamedShape {
    var numberOfSides: Int = 0
    var name: String

    init(name: String) {
        self.name = name
    }

    func simpleDescription() -> String {
        return "A shape with \(numberOfSides) sides."
    }
}
```

注意 `self` 被用来区别实例变量 `name` 和构造器的参数 `name`。当你创建实例的时候，像传入函数参数一样给类传入构造器的参数。每个属性都需要赋值，无论是通过声明（就像 `numberOfSides`）还是通过构造器（就像 `name`）。

如果你需要在对象释放之前进行一些清理工作，使用 `deinit` 创建一个析构函数。

子类的定义方法是在它们的类名后面加上父类的名字，用冒号分割。创建类的时候并不需要一个标准的根类，所以你可以根据需要添加或者忽略父类。

子类如果要重写父类的方法的话，需要用 `override` 标记——如果没有添加 `override` 就重写父类方法的话编译器会报错。编译器同样会检测 `override` 标记的方法是否确实在父类中。

```swift
class Square: NamedShape {
    var sideLength: Double

    init(sideLength: Double, name: String) {
        self.sideLength = sideLength
        super.init(name: name)
        numberOfSides = 4
    }

    func area() ->  Double {
        return sideLength * sideLength
    }

    // 重写父类方法
    override func simpleDescription() -> String {
        return "A square with sides of length \(sideLength)."
    }
}
let test = Square(sideLength: 5.2, name: "my test square")
test.area()
test.simpleDescription()
```

- 除了存储简单的属性之外，属性可以有 getter 和 setter。

```swift
class EquilateralTriangle: NamedShape {
    var sideLength: Double = 0.0

    init(sideLength: Double, name: String) {
        self.sideLength = sideLength
        super.init(name: name)
      
        // numberOfSides 是根类的变量
        numberOfSides = 3
    }

    var perimeter: Double {
        get {
            return 3.0 * sideLength
        }
        set {
            sideLength = newValue / 3.0
        }
    }

    override func simpleDescription() -> String {
        return "An equilateral triangle with sides of length \(sideLength)."
    }
}
var triangle = EquilateralTriangle(sideLength: 3.1, name: "a triangle")
print(triangle.perimeter)
triangle.perimeter = 9.9
print(triangle.sideLength)
```

在 `perimeter` 的 `setter` 中，新值的名字是 `newValue`。你可以在 `set` 之后显式的设置一个名字。

注意 `EquilateralTriangle` 类的构造器执行了三步：

1. 设置子类声明的属性值
2. 调用父类的构造器
3. 改变父类定义的属性值。其他的工作比如调用方法、getters 和 setters 也可以在这个阶段完成。

如果你不需要计算属性，但是仍然需要在设置一个新值之前或者之后运行代码，使用 `willSet` 和 `didSet`。写入的代码会在属性值发生改变时调用，但不包含构造器中发生值改变的情况。比如，下面的类确保三角形的边长总是和正方形的边长相同。