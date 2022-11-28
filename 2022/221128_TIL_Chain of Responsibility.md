# Chain of Responsibility Pattern

참고 - [refactoring guru - Chain of Responsibility ](https://refactoring.guru/ko/design-patterns/chain-of-responsibility)

## 설명
Chain of Responsibility 패턴은 Handler chain에 따라 요청을 전달할 수 있는 디자인 패턴이다.  
요청을 수신한 각 핸들러는 요청을 처리하거나 다음 핸들러로 전달할지 결정하게 됩니다.

여러개의 핸들러가 연결되어 있고 이렇게 연결된 핸들러들을 chain이라고 한다. 요청이 들어왔을 떄 chain은 요청을처리할수도 있고 다음 핸들러로 넘길 수도 있다.


## 언제 사용할까?
- 다양한 방식으로 다양한 종류의 요청들을 처리할것으로 예상되지만 정확한 요청 유형들과 순서들을 미리 알 수 없는 경우
- 특정 순서로 여러 핸들러를 실행해야할 때
- 핸들러들의 집홥과 그들의 순서사 런타임에 변경되어야 할 때


## 장단점
### 장점
- 요청의 처리 순서를 제어할 수 있다.
- 단일 책임 원칙. 당신은 작업을 호출하는 클래스들의 작업을수행하는 클래스들과 분리할 수 있습니다.
- 개방/폐쇠 원칙. 기존 클라이언트 코드를 손상하지 않고 앱에 새 핸들러를 도입할 수 있습니다.

### 단점
- 일부 요청들은 처리되지 않을 수도 있다.


## 예시

```swift

import XCTest

/// The Handler interface declares a method for building the chain of handlers.
/// It also declares a method for executing a request.
protocol Handler: class {

    @discardableResult
    func setNext(handler: Handler) -> Handler

    func handle(request: String) -> String?

    var nextHandler: Handler? { get set }
}

extension Handler {

    func setNext(handler: Handler) -> Handler {
        self.nextHandler = handler

        /// Returning a handler from here will let us link handlers in a
        /// convenient way like this:
        /// monkey.setNext(handler: squirrel).setNext(handler: dog)
        return handler
    }

    func handle(request: String) -> String? {
        return nextHandler?.handle(request: request)
    }
}

/// All Concrete Handlers either handle a request or pass it to the next handler
/// in the chain.
class MonkeyHandler: Handler {

    var nextHandler: Handler?

    func handle(request: String) -> String? {
        if (request == "Banana") {
            return "Monkey: I'll eat the " + request + ".\n"
        } else {
            return nextHandler?.handle(request: request)
        }
    }
}

class SquirrelHandler: Handler {

    var nextHandler: Handler?

    func handle(request: String) -> String? {

        if (request == "Nut") {
            return "Squirrel: I'll eat the " + request + ".\n"
        } else {
            return nextHandler?.handle(request: request)
        }
    }
}

class DogHandler: Handler {

    var nextHandler: Handler?

    func handle(request: String) -> String? {
        if (request == "MeatBall") {
            return "Dog: I'll eat the " + request + ".\n"
        } else {
            return nextHandler?.handle(request: request)
        }
    }
}

/// The client code is usually suited to work with a single handler. In most
/// cases, it is not even aware that the handler is part of a chain.
class Client {
    // ...
    static func someClientCode(handler: Handler) {

        let food = ["Nut", "Banana", "Cup of coffee"]

        for item in food {

            print("Client: Who wants a " + item + "?\n")

            guard let result = handler.handle(request: item) else {
                print("  " + item + " was left untouched.\n")
                return
            }

            print("  " + result)
        }
    }
    // ...
}

/// Let's see how it all works together.
class ChainOfResponsibilityConceptual: XCTestCase {
 
    func test() {

        /// The other part of the client code constructs the actual chain.

        let monkey = MonkeyHandler()
        let squirrel = SquirrelHandler()
        let dog = DogHandler()
        monkey.setNext(handler: squirrel).setNext(handler: dog)

        /// The client should be able to send a request to any handler, not just
        /// the first one in the chain.

        print("Chain: Monkey > Squirrel > Dog\n\n")
        Client.someClientCode(handler: monkey)
        print()
        print("Subchain: Squirrel > Dog\n\n")
        Client.someClientCode(handler: squirrel)
    }
}
```