# Combine - Subsciber

## sink(receiveCompletion:receiveValue:)
- closure에서 새로운 값이나 종료 이벤트에 대해 처리한다.

```swift
// 1. Publisher의 한 종류인 Just를 생성합니다.
let just = Just("Hello world!")

// 2. .sink를 통해 publisher에 대한 subscription을 작성합니다.
_ = just
  .sink(
    receiveCompletion: {
      print("Received completion", $0)
    },
    receiveValue: {
      print("Received value", $0)
  })
```

## assign(to:on:)
- 새로운 값을 keyPath에 따라 주어진 인스턴스의 property에 할당한다.
- 주어지는 값이 무조건 있어야 하기 때문에 Failure타입이 Never일 때만 사용할 수 있다.

```swift 
// 1. didSet을 통해 value 값이 바뀌면 새 값을 print합니다.
class SomeObject {
  var value: String = "" {
    didSet {
      print(value)
    }
  }
}

// 2. 위에서 만든 class의 instance를 선언합니다.
let object = SomeObject()

// 3. String 배열로 이뤄진 publisher를 생성합니다.
let publisher = ["Hello", "world!"].publisher

// 4. publisher를 구독하면서 새롭게 받은 값을 object의 value에 할당합니다.
_ = publisher
  .assign(to: \.value, on: object)
```

>.assign(to: \.value, on: object)

- Key-Path 표현
- `\.타입.프로퍼티` 이지만 타입은 생략하고 `\.프로퍼티`로 사용할 수 있다.