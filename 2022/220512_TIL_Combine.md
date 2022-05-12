# 공부한 것

# Combine

## Publisher

- 시간 흐름에 따라 값 시퀀스의 변화 흐름을 Subsciber에게 발행, 즉 전송해주는 역할
- 하나 혹은 여러개의 Subscriber에게 값 요소를 제공한다.

```swift
  // 1
  let publisher = (1...6).publisher
  
  // 2
  final class IntSubscriber: Subscriber {
    // 3
    typealias Input = Int
    typealias Failure = Never

    // 4
    func receive(subscription: Subscription) {
      subscription.request(.max(3))
    }
    
    // 5
    func receive(_ input: Int) -> Subscribers.Demand {
      print("Received value", input)
      return .none
    }
    
    // 6
    func receive(completion: Subscribers.Completion<Never>) {
      print("Received completion", completion)
    }
  }


let subscriber = IntSubscriber()

publisher.subscribe(subscriber)

```

## Subscriber

- Publisher를 구독하는 구독자라는 의미를 가진다.
- `CustomCombineIdentifierConvertible`프로토콜을 상속한다.
- `CustomCombineIdentifierConvertible`프로토콜은 Subscriber 개발도구가 앱에서 Publisher 체인을 고유하게 식별할 수 있도록 해준다.
- Publisher에서는 Output과 Failure을 방출한다.
- Output은 방출하는 값 타입, Failure은 방출할 수 있는 에러 타입을 나타낸다.

### Subscriber의 메서드
1. receive(subscription:)
- 구독권이라고 볼 수 있는 Subscription이라는 인스턴스를 Subscriber에게 전달해주는 메서드

2. receive(_:)
- Publisher에서 이제 방출된 값을 Subscriber에서 받기 위해 호출되는 메서드

3. receive(completion:)
- Publisher에서 정상적으로 값이 방출됐는지 에러가 났는지 노티를 주기 위해 호출된다.
- 이 메서드로 `정상` or `에러`를 Subsciber에게 전달

### Subscription 프로토콜
- Publisher와 Subsciber를 연결해주는 프로토콜
- 고유 식별자의 역할을 해주어야 하기 때문에 class로 되어있다.

### Subscription 메서드
1. request(_ demand: Subscriber.Demand)
- 구독권에 대한 정보 설정
- 어떤 값을 얼마나 받을지를 설정

2. cancel()
- 더는 값을 수신하지 않을 때 사용


## Subject

- Publisher의 일종인데, 차이점은 밖에서 값을 방출할 수 있다는 점이다.
- Publisher는 내부에서 생성되어 나오는 값들만 받아서 처리할 수 있지만 Subject는 `send(_ :)`메서드를 통해 외부의 값도 받아서 처리할 수 있다.

### Subject 종류

#### PassthroughSubject

- 다운스트림 Subsciber에게 값(이벤트)을 브로드캐스트(중계) 해주는 Subject
- 초기값과 버퍼값이 없다.
- `send`메서드를 통해 새로운 값을 해당 Subject를 구독하고 있는 모든 Subsciber에게 보낸다.


```swift
// 1
enum MyError: Error {
    case test
}

// 2
final class StringSubscriber: Subscriber {
    typealias Input = String
    typealias Failure = MyError
    
    func receive(subscription: Subscription) {
        subscription.request(.max(2))
    }
    
    func receive(_ input: String) -> Subscribers.Demand {
        print("Received value", input)
        // 3
        return input == "World" ? .max(2) : .none
    }
    
    func receive(completion: Subscribers.Completion<MyError>) {
        print("Received completion", completion)
    }
}

// 4
let subscriber = StringSubscriber()

// 5
let subject = PassthroughSubject<String, MyError>()

// 6
subject.subscribe(subscriber)

// 7
let subscription = subject
    .sink(
        receiveCompletion: { completion in
            print("Received completion (sink)", completion)
        },
        receiveValue: { value in
            print("Received value (sink)", value)
        }
    )

subject.send("Hello")
subject.send("World")
subscription.cancel()

// 9
subject.send("Still there?")
//subject.send(completion: .finished)
subject.send("How about another one?")
subject.send(completion: .failure(MyError.test))

//  결과
// Received value Hello
// Received value (sink) Hello
// Received value World
// Received value (sink) World
// Received value Still there?
// Received value How about another one?
// Received completion failure(PracticeCombine.MyError.test)
```


#### CurrentValueSubject

- 단일 값을 래핑하고 값 변경 시 마다 새 값(이벤트)를 게시(Publish)해주는 Subject
- 꼭 초기값을 줘야한다.
- 제일 최근에 방출된 값을 버퍼에 저장한다.

```swift
var subscriptions = Set<AnyCancellable>()

let subject = CurrentValueSubject<Int, Never>(0)

subject
  .sink(receiveValue: { print($0) })
  .store(in: &subscriptions)

subject.send(1)
print("Current Subject Value:", subject.value)

subject.send(2)
print("Current Subject Value:", subject.value)

subject.value = 3
print("Current Subject Value:", subject.value)

// 결과
// 0
// 1
// Current Subject Value: 1
// 2
// Current Subject Value: 2
// 3
// Current Subject Value: 3
```

#### send(completion:)
- 해당 메서드를 호출하면 Subscriber에게 끝났다라는 것을 알려준다. (완료신호)
- 완료 신호를 보내주고 그에대해서 Subscriber에서는 처리할 수 있다.
- 완료 신호를 보내고 나서 `send(_ :)로 값을 보내줘도 처리되지 않는다.


## Scheduler

- 클로저를 실행하는 시기와 방법을 정의하는 프로토콜
- 해당 작업을 어느 시간에 어떤 스레드에서 진행할 것인지 정해주는 역할


### Scheduler의 기본 속성
- 스케줄러를 설정해주지 않으면 기본 스케줄러를 따른다.
- 기본 스케줄러는 방출하는 작업이 이뤄지는 스레드가 기본 스케줄러의 스레드가 된다.

### Scheduler 사용
`recieve(on:) & subscribe(on:)` 두가지를 사용하여 구현
둘 다 사용법은 똑같다.

```swift
// main thread - receive
publisher
    .receive(on: DispatchQueue.main())

// main thread - subscribe
publisher
    . subscribe(on: DispatchQueue.main())

// global thread - receive
publisher
    .receive(on: DispatchQueue.global())
    
// global thread - subscribe
publisher
    . subscribe(on: DispatchQueue.global())
```


## Cancellable

### cancel()
- 작업에 대한 취소 메서드
- 호출 즉시 바로 취소가 적용된다.

### AnyCancellable
- `sink`메서드의 반환 값으로 쓰이면 내부적을 cancel()을 적절히 호출해준다.
- 해당 객체가 해제 될 때 cancel 메서드를 자동으로 호출해주어 해제된 스트림이 리소스를 잡아 먹고 있지 않도록 한다.

### store
- set에 취소 가능한 인스턴스를 저장할 집할을 뜻한다.
- sink를 통해 AnyCanellable이 반환된 것을 하나의 집합에 담아주는 것을 의미한다.
- 여러 서브젝트 취소를 담아 두었다가 한번에 처리할 수 있도록 구현 할 수 있다.