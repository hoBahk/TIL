## 고민한 점


클래스의 인스턴스끼리 비교할 때는 `==`가 기본적으로는 사용할 수 없다. 구현이 되어 있지 않기 때문이다. `==`를 사용하려면 `Equatable` 프로토콜을 채택하여 `==`에 대해 정의해 주어야 한다.   
그런데 입력된 `UIButton`이 어떤 버튼인지 확인 하기 위해서 switch문을 사용하여 비교를 하는데  switch문은 `==`비교이다. 그럼 UIButton에 어떤 것이 `==`비교를 가능하게 했을까를 확인해보았다. UIButton에는 해당하는 `==`메서드가 없는 거 같아 `UIButton`의 부모클래스인 `UIControl`을 보고 `UIView`를 보아도 없었다.    

그래서 메이슨에게 질문을 하였는데 메이슨이 친절하게 알려주셨다.    
` UIButton > UIControl > UIView > UIResponder > NSObject ` 
`NSObject`의 `isEqual`메서드를 통해 `==`비교가 가능하게 된다는 것을 알게 되었다.

`isEqual` 메서드로 어떻게 `==`를 비교할 수 있는지 믿기지 않았다.    
내가 아는 지식으로는 == 메서드를 사용해야 한다고 생각했다.

테스트를 해보았다.

```swift
class Test: NSObject {
    
    var title: String
    var author: String
    
    init(title: String, author: String) {
         self.title = title
         self.author = author
    }
    
    override func isEqual(_ object: Any?) -> Bool {
        print(#function, "called")
        if let other = object as? Test {
            return self.title == other.title
        } else {
            return false
        }
    }
}

let test1 = Test(title: "title", author: "title")
let test2 = Test(title: "title", author: "title")

print(test1 == test2)
```

`Test`클래스가 `NSObject`를 상속하도록 하고 `isEqual`메서드를 `override`하여 재정의 하였다.    
그리고 `==`를 사용하여 비교를 해보았다. 결과는 아래와 같이 나왔다.

```
isEqual(_:) called
true
```

`==` 비교를 할 때에 `isEqual`메서드가 사용이 된다는 것을 알 수 있다.

이것이 어떻게 가능할까를 찾아보았는데 찾을 수 없었다..
차차 공부하면서 알아가야겠다.

