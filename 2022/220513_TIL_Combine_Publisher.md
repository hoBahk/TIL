# Combine - Publisher

## Just

- 가장 간단한 형태의 Publisher
- Error타입은 항상 Never이다.

```swift
let publisher = Just("Bahk")

let subscriber = publisher
    .sink { value in
        print(value)
    }
```

## Future

- callback 기반 completion Handler를 대체할 수 있다. (더 깔끔)
- Output(값), Failure(오류)를 가지고 있다.
- 한 번 사용하고나면 종료 된다.
- 이것만 클래스

```swift
let future = Future<Int, Never> { promise in
    promise(.success(1))
    promise(.success(2))
}

future.sink(receiveCompletion: { print($0) },
            receiveValue: { print($0) })
            
            
// 출력
// 1
// finished

// 1만 출력되고 종료된다.
```

## Fail

- 지정한 에러로 즉시 종료를 한다.

```swift
let failed = Fail<String, Error>(error: NSError(domain: "error", code: -1, userInfo: nil))
_ = failed.sink {
  print($0)
} receiveValue: {
  print($0)
}

// 결과 : failure(Error Domain=error Code=-1 "(null)")
```


## Deferred

- 대기하고 있다가 구독이 이루어 질 때 Publisher를 만든다.

```swift

 class ClubHouseHandsUp: Publisher {
     typealias Output = String
     typealias Failure = Never
     
     init() {
         NSLog("ClubHouseHandsUp init")
     }
     
     func receive<S>(subscriber: S) where S : Subscriber, Failure == S.Failure, Output == S.Input {
         DispatchQueue.global(qos: .utility).async {
             let dummy: [String] = ["jack","tom"]
             dummy.forEach {
                 _ = subscriber.receive($0)
             }
             subscriber.receive(completion: .finished)
         }
     }
 }

 let publisher = ClubHouseHandsUp()
 print("publisher init")
 _ = publisher.sink {
     print($0)
 }
 // 결과
 // ClubHouseHandsUp init
 // publisher init
 // jack
 // tom
 // finished
 
 
 
 let deferred = Deferred<ClubHouseHandsUp>{ () -> ClubHouseHandsUp in
     return ClubHouseHandsUp()
 }
 print("deferrd init")
 _ = deferred.sink {
     print($0)
 } receiveValue: {
     print($0)
 }
 // 결과
 // deferrd init
 // ClubHouseHandsUp init
 // jack
 // tom
 // finished

```


## Empty
- 이벤트 없이 종료되는 Publisher

```swift
let empty = Empty<String, Never>()
_ = empty.sink {
    print($0)
} receiveValue: {
    print($0)
}
// 결과 : finished
```


## Fail
- 오류와 함께 종료되는 publisher

```swift
let failed = Fail<String, Error>(error: NSError(domain: "error", code: -1, userInfo: nil))
_ = failed.sink {
  print($0)
} receiveValue: {
  print($0)
}
// 결과 : failure(Error Domain=error Code=-1 "(null)")
```

## Record
- 입력과 완료를 기록해 다른 subscriber에서 반복될 수 있는 publisher

```swift

let record = Record<String, Error> { recoding in
    print("make recording")
    recoding.receive("jack")
    recoding.receive("tom")
    recoding.receive(completion: .finished)
}
_ = record.sink {
    print($0)
} receiveValue: {
    print($0)
}
_ = record.sink {
    print($0)
} receiveValue: {
    print($0)
}
// 결과
// make recording
// jack
// tom
// finished
// jack
// tom
// finished
```