# Sendable

프로토콜, Concurrency 상황에서 안전하게 공유될 수 있는 타입
한 concurrency domain에서 다른 domain으로 sendable type을 안전하게 전달할 수 있다.

## 전송가능한 타입
- Value types

- Reference types with no mutable storage

- Reference types that internally manage access to their state

- Functions and closures (by marking them with @Sendable)

## Value types
Sendable 프로토콜의 요구사항을 충족하려면 열거형이나 구조체에 Sendable 멤버 및 연관값만 있어야한다.
frozen 래퍼가 달린 구조체나 열거형은 Sendable을 암묵적으로 준수한다.
nonsendable한 저장 프로퍼티가 있는 구조체, nonsendable한 연관값이 있는 열거형은 Sendable 프로토콜의 요구사항을 충족하는지 수동으로 확인한 후 @unchecked Sendable로 표시하여 컴파일 시간 정확성 검사를 비활성화 할 수 있다.

## Actor
모든 Actor는 mutable state에 대한 isolation을 보장하기 때문에 Sendable을 암묵적으로 conform하고 있다.
 
## class
 아래 요구사항을 충족하면 Sendable을 채택할 수 있다.
 - final 표시
 - immutable하고 sendable한 저장 프로퍼티만 포함되어 있다.
 - 슈퍼클래스가 없거나 NSObject를 슈퍼클래스로 가진다.

## Functions and closures
Sendable 프로토콜을 준수하는 대신 Sendable 함수 및 Closure를 @Sendable 속성으로 표시한다. 함수 또는 클로저가 캡처하는 값은 모두 Sendable이어야 한다. 
@Sendable을 클로저의 파라미터 앞에 써서 사용할 수 있다.

```swift
let sendableClosure = { @Sendable (number: Int) -> String in
    if number > 12 {
        return "More than a dozen."
    } else {
        return "Less than a dozen"
    }
}
```

## Tuples
튜플의 모든 요소가 Sendable로 되어 있으면 튜플은 암묵적으로 Sendable을 준수한다.

## Metatypes
Int.Type과 같은 메타타입은 Sendable을 암묵적으로 준수한다.


[apple developer](https://developer.apple.com/documentation/swift/sendable)