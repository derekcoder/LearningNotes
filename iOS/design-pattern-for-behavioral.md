## 行为型模式

> 在软件工程中，行为型模式是设计模式的一种类型，用来识别对象之间的常用通信模式并加以实现。通过这样做，可以增加通信的灵活性。



### 责任链模式（Chain of Responsibility）

意图：

使多个对象都有机会处理请求，从而能避免请求的发送者和接受者之间的**耦合关系**。将这些对象连成一条链，并沿着这条链传递该请求，直到有一个对象处理它为止。



适用性：

* 有多个对象可以处理一个请求，哪个对象处理该请求运行时刻自动确定。

* 你想在不明确接受者的情况下，向多个对象中的一个提交一个请求。

* 可处理一个请求的对象集合应被动态指定。

```swift
import Foundation

final class MoneyPile {
    private let value: Int
    private let quantity: Int
    private let nextPile: MoneyPile?
    
    init(value: Int, quantity: Int, nextPile: MoneyPile?) {
        self.value = value
        self.quantity = quantity
        self.nextPile = nextPile
    }
    
   	func canWithdraw(amount: Int) -> Bool {
        var amount = amount
        var quantity = self.quantity
        while amount >= self.value {
            if quantity == 0  { break }
            amount -= self.value
            quantity -= 1
        }
        
        guard amount > 0 else { return true }
        
        if let nextPile = nextPile {
            return nextPile.canWithdraw(amount: amount)
        }
        
        return false
    }
}

final class ATM {
    private let hundred: MoneyPile
    private let fifty: MoneyPile
    private let twenty: MoneyPile
    private let ten: MoneyPile
    
    init(hundred: MoneyPile, fifty: MoneyPile, twenty: MoneyPile, ten: MoneyPile) {
        self.hundred = hundred
        self.fifty = fifty
        self.twenty = twenty
        self.ten = ten
    }
    
    func canWithdraw(amount: Int) -> Bool {
        return hundred.canWithdraw(amount: amount)
    }
}

let ten = MoneyPile(value: 10, quantity: 6, nextPile: nil)
let twenty = MoneyPile(value: 20, quantity: 2, nextPile: ten)
let fifty = MoneyPile(value: 50, quantity: 2, nextPile: twenty)
let hundred = MoneyPile(value: 100, quantity: 1, nextPile: fifty)

let atm = ATM(hundred: hundred, fifty: fifty, twenty: twenty, ten: ten)
atm.canWithdraw(amount: 310)  // false
atm.canWithdraw(amount: 100)  // true
atm.canWithdraw(amount: 165)  // false
atm.canWithdraw(amount: 30)   // true
```



### 命令模式（Command）



### 解释器模式（Interpreter）



### 迭代器模式（Iterator）



### 调停者模式（Mediator）

### （Memento）



### 观察者模式（Observer）



### 状态模式（State）



### 策略模式（Strategy）



### 模板方法模式（Template）



### 访问者模式（Visitor）

### 
