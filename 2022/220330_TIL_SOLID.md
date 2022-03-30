# SOLID 원칙

## SOLID란?
SOLID란 객체 설계에 필요한 5가지 원칙으로써 유지보수가 쉽고, 유연하고, 확장이 쉬운 소프트웨어를 만들기 위한 원칙입니다.
`Clean Agile`, `Clean Achitecture`, `Clean Code`, `Clean Software`등의 클린코드에 대한 책을 많이 쓰신 로버트 C.마틴이 제시한 객체지향설계 5가지 원칙입니다.


### SOLID는 5가지 원칙의 약자 입니다!

- S (Single Responsibility Principle) : 단일 책임 원칙  
- O (Open-Close Principle) : 개방-폐쇄 원칙  
- L (Liscov Substitution Principle) : 리스코브 치환 원칙  
- I (Interface Segregation Principle) : 인터페이스 분리 원칙  
- D (Dependency Inversion Principle) : 의존성 역전 원칙  


## 결합도와 응집도
좋은 코드가 되기 위해서는 응집도는 높고 결합도는 낮아야 한다는 말을 많이 들어보셨을 것 같습니다!   
그럼 응집도와 결합도가 뭘까요?   
먼저 응집도와 결합도에 대해 간단히 알아보겠습니다~

### 결합도(Coupling)
결합도는 서로 다른 모듈간의 상호 의존하는 정도 또는 연관된 관게를 의미합니다.   
모듈관의 관련성을 나타내는 척도라고 생각하시면 될 것 같아요.   
그래서 결합도가 높게 되면 모듈간의 의존하는 정도가 크기 때문에 코드를 수정할 때 우리도 모르게 다른 모듈에 영향(Side Effect)을 끼칠 수 있습니다.   
또한, 오류가 발생 했을 때도 다른 모듈에도 영향을 끼칠 수 있고 다른 모듈의 영향을 받아 오류가 발생했을 수도 있게 됩니다.
그래서 결합도는 낮은 것이 좋습니다!!

### 응집도(Cohesion)
응집도는 모듈 내부의 요소들 간의 기능적 연관성을 나타내는 척도입니다.   
모듈이 얼마나 독립적으로 되어있는 정도를 나타냅니다.   
응집도가 높다면 수정이나 오류가 발생했을 때 하나의 모듈 안에서 처리를 할 수 있습니다.   
그렇게 되면 유지보수가 용이해지는 장점이 있습니다! 


## SRP(Single Responsibility Principle) 단일 책임 원칙

### 하나의 클래스는 단 한가지의 책임만을 가져야한다!

- 여기서 말하는 `책임`은 간단히 말하면 `변경의 이유`를 말합니다.
- 하나의 책임이 여러 클래스에 나뉘어 있어도 안됩니다.    
- 책임이 같은 것은 같은 클래스에 모아 놓는 것이 좋다는 말로 보입니다!   
- 수정이 필요할 때 수정이 한 클래스에서만 이루어 진다면 수정이 쉬워집니다.
- 그렇게 되면 응집도는 높고 결합도는 낮은 코드가 될 수 있습니다.

아래와 같이 하나의 클래스에서 여러가지의 책임을 가지고 있으면 안됩니다.
후에 코드의 확장이 어려워지겠죠!
```swift
class Market {
    func manage() {
        stock()
        sell()
        deliver()
    }
    
    private func stock() {
        // stock
    }
    
    private func sell() {
        // sell
    }
    
    private func deliver() {
        // deliver
    }
}
```

그래서 각각의 역할을 하는 클래스를 만들어줍니다.

```swift 
class Market {
    let stockManager: StockManager
    let sellManager: SellManager
    let deliveryManager: DeliveryManager
    
    init(stockManager: StockManager, sellManager: SellManager, deliveryManager: DeliveryManager) {
        self.stockManager = stockManager
        self.sellManager = sellManager
        self.deliveryManager = deliveryManager
    }
    
    func manage() {
        stockManager.stock()
        sellManager.sell()
        deliveryManager.deliver()
    }
}

class StockManager {
    func stock() {
        // stock
    }
}

class SellManager {
    func sell() { 
        // sell
    }
}

class DeliveryManager {
    func deliver() {
        // deliver
    }
}
```

이렇게 되면 단일 책임 원칙(SRP)은 해결이 되지만 의존성역전 원칙(DIP)에 위반하게 됩니다..!

그래서 프로토콜로 의존성역전을 해주도록하겠습니다.
지금은 보고 넘어가셔도 됩니다.
밑에 의존성 역전 법칙(DIP)에 대해 설명이 있으니 나중에 보고 다시 오셔도 됩니다~!

```swift
class Market {
    let stockManager: Storable
    let sellManager: Salable
    let deliveryManager: Deliverable
    
    init(stockManager: Storable, sellManager: Salable, deliveryManager: Deliverable) {
        self.stockManager = stockManager
        self.sellManager = sellManager
        self.deliveryManager = deliveryManager
    }
    
    func manage() {
        stockManager.stock()
        sellManager.sell()
        deliveryManager.deliver()
    }
}

class StockManager {
    func stock() {
        // stock
    }
}

class SellManager {
    func sell() { 
        // sell
    }
}

class DeliveryManager {
    func deliver() {
        // deliver
    }
}

protocol Storable {
    func stock()
}

protocol Salable {
    func sell()
}

protocol Deliverable {
    func deliver()
}
```

## OCP(Open-Closed Principle) 개방-폐쇄 원칙

### 확장에는 열려있으나 변경에는 닫혀있어야 한다!

- `확장에 열려있다`는 말은 기능을 확장할 때 힘들지 않게 확장할 수 있도록 하는 것입니다.
- `변경에 닫혀있다`는 말은 기능을 확장할 때 기존의 코드를 최대한 수정하지 않고 확장하는 것입니다.
- 만약에 기존의 코드를 수정하게 된다면 연쇄적인 수정을 하지 않도록 해야합니다.
- 개방-폐쇄 원칙을 잘 지킨다면 수정이 연쇄적으로 이루어지지 않고 새로운 코드를 추가할 수 있는 구조가 되기 때문에 기존의 코드의 수정이 없어 재검증이 필요없이 소프트웨어를 확장해 나갈 수 있습니다!
- 수정이 될 때 해당 객체만 바꿔도 동작이 잘 된다면 OCP를 잘 지킨 것이라고 볼 수 있을 것 같습니다!

아래와 같이  코드를 작성하게 되면 Drink에 다른 음료가 추가되면 기존의 코들르 수정하게 됩니다.
그렇게 되면 변경에 열려있게 되어 OCP를 위반하게 됩니다.

```swift
enum Drink {
    case cola
    case ade
    case juice
    
    var name: String {
        switch self {
        case .cola:
            return "cola"
        case .ade:
            return "ade"
        case .juice:
            return "juice"
        }
    }
}

class VendingMachine {
    let drink: [Drink] = []    
    
    func sell(drink: Drink) {
        print(drink.name)
    }
}
```

이를 구현해 내려면 `추상화`를 활용하면 좋습니다!

아래처럼 구현해 준다면 새로운 음료가 나오더라도 새로운 음료에 대한 객체만 만들어주면 되기 때문에 기존의 코드를 수정하지 않고 확장할 수 있습니다. 

```swfit
protocol Drink {
    var name: String { get }
}

class Cola {
    let name = "cola"
}

class Ade {
    let name = "ade"
}

class Juice {
    let name = "juice"
}

class VendingMachine {
    let drink: [Drink] = []    
    
    func sell(drink: Drink) {
        print(drink.name)
    }
}
```

디자인 패턴 중 Strategy 패턴과 유사합니다. (위의 예제는 같다고도 할 수 있을 것 같아요..!)
Strategy 패턴을 사용하면 OCP를 잘 지킬 수 있습니다.
Strategy 패턴에 대해서는 추후에 포스팅 하겠습니다..!


## LSP(Liskov Substitution Principle) 리스코프 치환 원칙

### 자식클래스는 부모클래스의 역할을 완벽히 할 수 있어야한다!

- 자식클래스는 부모클래스의 동작을 바꾸지 않아야 합니다.
- 상속을 사용했을 때 자식클래스는 부모클래스 대신 사용이 되어도 같은 동작을 해야 합니다.


리스코프 법칙은 제일 유명한 사각형과 졍사각형을 예로 들어보도록 하겠습니다.
아래와 같이 코드를 작성하게 되면 (정사각형)의 넓이를 구하는 것이 잘못 작동 되게 됩니다.   
그렇게 되면 자식클래스인 Square(정사각형)이 부모클래스인 Rectangle(직사각형)의 동작을 제대로 실행하고 있지 못한것이 됩니다..!

```swift
class Rectangle {
    var width: Float = 0
    var height: Float = 0
    
    var area: Float {
        return width * height
    }
}

class Square: Rectangle {
    override var width: Float {
        didSet {
            height = width
        }
    }
}

func printArea(of rectangle: Rectangle) {
	rectangle.height = 9
	rectangle.width = 16
	print(rectangle.area)
}
```

그래서 Quadrangle이라는 프로토콜로 area만 추상화를 시켜 각 객체에서 Quadrangle 프로토콜을 채택하고 area를 구하는 로직을 구현하도록 합니다.

```swift
protocol Quadrangle {
    var area: Float { get }
}

class Rectangle: Quadrangle {
    let width: Float
    let height: Float
    
    var area: Float {
        return width * height
    }
    
    init(width: Float,
         height: Float) {
        self.width = width
        self.height = height
    }
}

class Square: Quadrangle {
    let length: Float
    
    var area: Float {
        return length * length
    }
    
    init(length: Float) {
        self.length = length
    }
}
```


## ISP(Interface Segregation Principle) 인터페이스 분리 법칙

### 클라이언트가 불필요한 자신이 사용하지 않는 인터페이스에 의존하지 않아야한다!

- 상속 받은 메소드를 퇴화시켜야하는 경우가 발생할 수 있습니다.
- 불필요한 인터페이스에 의존하면 불필요한 빌드가 유발될 수 있습니다.
- 인터페이스가 거대해지는 경우 SRP를 위반하는 경우가 생길 수 있습니다.
- 큰 인터페이스를 작은 인터페이스들로 분리하고, 필요한 부분만 클라이너트가 선택하여 사용할 수 있도록 해야 합니다.


아래와 같이 fly 메서드는 사용하지 않게 된다.
이렇게 된다면 ISP를 위반하게 된다.

``` swift
protocol Movable {
    func run()
    func fly()
    func ride()
}

class Person: Movable{
    func run() { }
    func ride() { }
    
    // 사용하지 않음
    func fly() { }
}

class Bird: Movable {
    func fly() { }
    
    // 사용하지 않음
    func run() { }
    func ride() { }
}
```

그래서 프로토콜을 분리하여 필요한 것만 채택하여 사용할 수 있도록 하는 것이 좋다.
아래와 같이 분리하여 사용합니다!

```swift
protocol Running {
    func run()
}

protocol Flying {
    func fly()
}

protocol Ridable {
    func ride()
}

class Person: Running, Ridable {
    func run() { }
    func ride() { }
}

class Bird: Flying {
    func fly() { }
}
```

## DIP(Dependency Inversion Principle) 의존성 역전 원칙
### 상위 레벨의 모듈은 하위 레벨의 모듈에 의존해서는 안됩니다!

- 두 모듈은 추상화된 인터페이스(프로토콜)에 의존해야 합니다.
- 구체적인 사항은 추상화에 의존해야합니다.
- 의존관계를 맺을 때 자주 변화하는 것(클래스)보다는 변화하기 어렵거나 거의 변화가 없는 것(인터페이스)에 의존해야 합니다.
- 상위레벨 모듈이 하위 레벨 모듈을 참조하는 것은 좋지 않으며, 그런 경우는 제네릭 또는 Associate를 사용하는 것이 좋습니다.
- 의존성 역전 원칙을 지키기 위해서는 `의존성 주입`으로 해결 할 수 있습니다.

#### 의존성
먼저 의존성이라는 것은 간단히 말하면 다른 객체(클래스) 사이에 의존 관계가 있다는 것을 말합니다.   
의존하는 객체가 수정되면 다른 객체 또한 영향을 받게 됩니다.   
예로들면 아래와 같이 B 클래스의 인스턴스를 A 클래스가 프로퍼티로 가지고 있게 되면 A 클래스가 B 클래스에 의존성이 생기게 됩니다.

```swift
class Order {
    let menu = Menu()
}

class Menu {
    var price: Int = 1000
}

let order = Order()
print(order.menu.price) // 1000
	
```
#### 의존성 주입
의존성 주입을 통해 내부에서 인스턴스를 생성하는 것이 아닌 외부에서 인스턴스를 생성 후 넣어줄 수 있습니다.

```swift
class Order {
    let menu: Menu
    init(menu: Menu) {
        self.menu = menu
    }
}

class Menu {
    var price: Int = 1000
}

let menu = Menu()
let order = Order(menu: menu)
print(order.menu.price)
```

그리고 여기서 더 나아가 의존성 역전 원칙에 따라 의존성 또한 분리 해야합니다.   
위의 코드 중 A 클래스의 생성자에는 여전히 B 클래스 타 입만 받을 수 있게 되어있습니다.   
이것도 분리를 해주어야 합니다.     
두 개의 클래스가 추상화된 프로토콜에 의존함으로서 두 클래스는 의존 관계가 독립적이게 됩니다.

```swift
protocol Price {
    var price: Int { get set }
}

class Order {
    let menu: Price
    init(menu: Price) {
        self.menu = menu
    }
}

class Menu: Price {
    var price: Int = 1000
}

let menu = Menu()
let order = Order(menu: menu)
print(order.menu.price)
```