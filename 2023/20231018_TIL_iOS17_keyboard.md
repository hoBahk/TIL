# iOS17 Keyboard

## 겪은 문제
기존에 키보드가 올라올 때 키보드의 높이를 구해서 높이에 맞게 뷰의 오프셋을 조정해 키보드가 텍스트필드나 텍스트뷰를 가리지 못하게 구현해놓았다.   
그런데 iOS17에서는 키보드가 올라갈 때는 뷰의 오프셋이 잘 올라 가는데 키보드가 내려갈 때는 내려갈 듯 하더니 계속 다시 올라왔다.   

## 원인
기존에 어떻게 되어 있냐면 Notification의 `UIResponder.keyboardWillShowNotification`와 `UIResponder.keyboardWillHideNotification`를 사용하여 키보드가 올라가고 내려가는 것을 관찰하도록 등록 하였다.   

```swift
NotificationCenter.default.addObserver(forName: UIResponder.keyboardWillShowNotification,
                                       object: nil,
                                       queue: .main) { (notification) in
    guard let userInfo = notification.userInfo,
          let keyboardRect = userInfo[UIResponder.keyboardFrameEndUserInfoKey] as? CGRect else { return }

    self.keyboardHeight = keyboardRect.height
}

NotificationCenter.default.addObserver(forName: UIResponder.keyboardWillHideNotification,
                                       object: nil,
                                       queue: .main) { (_) in
    self.keyboardHeight = 0
}
```
     

디버깅을 시작했다.  iOS17에서는 이상하게도 키보드가 내려갈 때 노티피케이션이 내려가기, 올라가기, 내려가기, 올라가기...   
각 두번 씩 총 4번 실행되며 다시 제자리로 돌아온 것이다.   
아직 자료가 없어 정확한 이유는 모르겠지만 iOS17의 버그인 것으로 생각이 되었다.   

## 해결
기존에는 @Published 프로퍼티래퍼로 되어 있던 `keyboardHeight`를 `onRecieve`로 트래킹 했었다 그래서 바뀐 값을 그대로 다 받고 있었다.   
그래서 첫번째 값만 받아야겠다고 생각해서 일단 퍼블리셔를 다시 만들었다.   

```swift
var keyboardHeightPublisher: AnyPublisher<CGFloat, Never> {
    self.$keyboardHeight
        .first()
        .eraseToAnyPublisher()
}
```

왜 값이 안들어오지... 이상하군...
그럼 일단 throthle이 첫번쨰만 가져올 수 있다고 했던거 같은데??

```swift
var keyboardHeightPublisher: AnyPublisher<CGFloat, Never> {
    self.$keyboardHeight
        .throttle(for: 0.1, scheduler: RunLoop.main, latest: false)
        .eraseToAnyPublisher()
}
```

그랬더니 된다!!!!

first가 왜 안되는지는 공식문서를 살펴 봤는데...   

```
Use first() to publish just the first element from an upstream publisher, then finish normally. The first() operator requests unlimited from its upstream as soon as downstream requests at least one element. If the upstream completes before first() receives any elements, it completes without emitting any values.
```

이런말이 있다. 
```
업스트림이 어떤 요소라도 받기 전에 완료되면 값을 표시하지 않고 완료됩니다.
```

요소를 받기 전에 완료 됐다는 이야기이다.  
왜 완료됐는지는 더 알아봐야겠다...

