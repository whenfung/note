虽然 `Xcode` 可以在创建项目的时候默认创建 ` Test Target`，但是我们可以自定义或者给没有 `Test Target` 的工程添加 `Test Target`。在导航栏中切换到测试导航菜单，然后点击左下角 `+` 添加 `Test Target`。

## 样例

创建一个计算个人所得税的类。

```swift
import UIKit

class TaxRevenueBL: NSObject {
    
    // 计算个人所得税
    func calculate(_ revenue: Double) -> Double {
        
        // 月应纳个人所得税税额
        var tax = 0.0
        
        // 月应纳税所得额
        let dbTaxRevenue = revenue - 3500
        
        // 月应纳税所得额不超过 1500 元
        if dbTaxRevenue <= 1500 {
            tax = dbTaxRevenue * 0.03
        } else if dbTaxRevenue > 1500 && dbTaxRevenue <= 4500 {
            tax = dbTaxRevenue * 0.1 - 105
        } else if dbTaxRevenue > 4500 && dbTaxRevenue <= 9000 {
            tax = dbTaxRevenue * 0.2 - 555
        } else if dbTaxRevenue > 9000 && dbTaxRevenue <= 35000 {
            tax = dbTaxRevenue * 0.25 - 1005
        } else if dbTaxRevenue > 35000 && dbTaxRevenue <= 55000 {
            tax = dbTaxRevenue * 0.3 - 2755
        } else if dbTaxRevenue > 55000 && dbTaxRevenue <= 80000 {
            tax = dbTaxRevenue * 0.35 - 5505
        } else if dbTaxRevenue > 80000 {
            tax = dbTaxRevenue * 0.45 - 13505
        }
        return tax
    }
}
```

然后在单元测试模块中做相应的测试，函数名以 `test` 开头，例如 `testExample.swift`

```swift
import XCTest
@testable import UnitTest

class UnitTestTests: XCTestCase {
    
    var bl: TaxRevenueBL!

    override func setUpWithError() throws {
        // Put setup code here. This method is called before the invocation of each test method in the class.
        
        super.setUp()
        self.bl = TaxRevenueBL()
    }

    override func tearDownWithError() throws {
        // Put teardown code here. This method is called after the invocation of each test method in the class.
        
        self.bl = nil
        super.tearDown()
    }

    func testExample() throws {
        // This is an example of a functional test case.
        // Use XCTAssert and related functions to verify your tests produce the correct results.
    }

    func testPerformanceExample() throws {
        // This is an example of a performance test case.
        self.measure {
            // Put the code you want to measure the time of here.
        }
    }
    
    func testCalculateLevel1() {
        let dbRevenue = 5000.0
        let tax = self.bl.calculate(dbRevenue)
        XCTAssertEqual(tax, 45.0, "用例 1 测试失败")
    }
    
    func testCalculateLevel2() {
        let dbRevenue = 8000.0
        let tax = self.bl.calculate(dbRevenue)
        XCTAssertEqual(tax, 345.0, "用例 1 测试失败")
    }
    func testCalculateLevel3() {
        let dbRevenue = 12500.0
        let tax = self.bl.calculate(dbRevenue)
        XCTAssertEqual(tax, 1245.0, "用例 1 测试失败")
    }
    func testCalculateLevel4() {
        let dbRevenue = 38500.0
        let tax = self.bl.calculate(dbRevenue)
        XCTAssertEqual(tax, 7745.0, "用例 1 测试失败")
    }
    func testCalculateLevel5() {
        let dbRevenue = 58500.0
        let tax = self.bl.calculate(dbRevenue)
        XCTAssertEqual(tax, 13745.0, "用例 1 测试失败")
    }
    func testCalculateLevel6() {
        let dbRevenue = 83500.0
        let tax = self.bl.calculate(dbRevenue)
        XCTAssertEqual(tax, 22495.0, "用例 1 测试失败")
    }
    func testCalculateLevel7() {
        let dbRevenue = 103500.0
        let tax = self.bl.calculate(dbRevenue)
        XCTAssertEqual(tax, 31495.0, "用例 1 测试失败")
    }

}
```

## 单元测试覆盖率

