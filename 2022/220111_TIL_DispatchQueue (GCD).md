# DispatchQueue (GCD)

## Serial vs Concurrent
DispatchQueue는 크게 두가지가 있습니다.
SerialQueue와 ConcurrentQueue가 있습니다.
SerialQueue는 단일스레드에서 작업을 처리하고 ConcurrentQueue는 다중스레드에서 작업을 처리합니다.

간단히 설명하면,
단일스레드에서 작업을 한다는 것은 직렬큐에서 작업을 하는 것이고, 하나의 일을 하나의 쓰레드가 하는 것입니다.
다중스레드에서 작업 하는 것은 일을 여러 스레드가 하는 것을 말합니다.

동시성에 대해서 자세한 내용은 다음에 올리겠습니다.

## DispatchQueue의 종류
DispatchQueue의 종류는 크게 세가지가 있습니다.
	- Main Queue
	- Global Queue
	- Custom Queue


### Main Queue
- MainQueue는 메인쓰레드에서 동작합니다.
- 따로 조치를 취하지 않고 코드를 작성하게 되면 Main Queue에서 sync로 실행됩니다. 
- 메인큐에 작업을 추가하면 SerialQueue인 메인스레드에서 작업을 처리합니다. 

#### 실행 예시
```swift
DispatchQueue.main.sync { // 에러 발생!! main큐에서 sync를 직접적으로 호출하면 deadlock(교착상태)에 빠지게 됩니다.
// ...
}

DispatchQueue.main.async { // main큐의 비동기 작업
// ...
}

```



### Global Queue
- GlobalQueue는 ConcurrentQueue입니다.
- GlobalQueue에 작업을 추가하면 새로운 스레드를 만들어 그 위에서 작업을 처리합니다.

#### 실행예시
```swift
DispatchQueue.global().sync { // global큐의 동기 작업
// ...
}

DispatchQueue.global().async { // global큐의 비동기 작업
// ...
}
```



## CustomQueue
- CustomQueue는 DispatchQueue를 커스텀하여 사용하는 큐입니다.
- 초기화를 통해 커스텀할 수 있습니다.
- 커스텀큐는 따로 설정하지 않으면 serial큐 특성을 가집니다.

#### 커스텀큐 생성 예시
```swift
let customQueue = DispatchQueue(label: "customQueue", qos: .unspecified, attributes: [], autoreleaseFrequency: .inherit, target: nil)
```


자세한 내용들은 추후 업로드 하겠습니다.