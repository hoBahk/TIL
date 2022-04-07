# 메모리 참조 (strong, weak, unowned)

## 1. 강한참조 (Stong)

객체를 강하게 참조한다고 하여 강한참조라고 부르게 됩니다.   
강한참조를 하게 되면 자신이 참조하는 인스턴스의 소유권을 가지게 됩니다.   
또한 강한참조를 하게 되면 RC가 증가하게 됩니다.   
우리가 사용할 떄 아무것도 적어주지 않으면 강한참조로 선언이 됩니다.   
Default가 strong으로 되어 있기 때문입니다.   

아래 예제처럼 아무것도 적어주지 않는다면 강한참조이며, RC가 증가하게 됩니다!

```swift
class Person {
    let name: String
    
    init(name: String) {
        self.name = name
        print("init")
    }
    
    deinit {
        print("deinit")
    }
}
```
강한참조의 문제점은 순환참조가 발생할 수 있다는 것입니다.  
순환참조는 아래에서 설명하도록 하겠습니다~


## 2. 약한참조(weak)

약한참조는 해당 인스턴스의 소유권을 가지지 않고, 주소값만 가지고 있는 포인터 개념이라고 생각하시면 좋을 것 같아요!   
약한참조는 RC를 증가시키지 않습니다.   
그러면 약한참조를 쓰면 순환참조가 일어나지 않는거 아니야? 라고 생각하고 약한참조를 쓰려고 생각하시면 안됩니다!    
약한참조는 강한참조의 문제점을 해결하기 위해 사용됩니다.

아래처럼 약한 참조로 객체를 생성하게 되면 객체가 생성은 되지만 바로 객체가 해제되어 nil이 됩니다!

```swift
weak var person = Person(name: "hoBahk")
```

약한참조로 선언하게 되면 참조하던 인스턴스가 메모리에서 해제되면 자동으로 nil이 할당되어 메모리가 해제됩니다.   
nil이 할당되려면 당연히 옵셔널 타입이겠습니다!

weak을 사용할 때는 소유자에 비해서 짧은 생명주기를 가진 인스턴스를 참조할 때 사용해야합니다.   
대상 인스턴스를 참고하고 있는 중에 언제든지 메모리에서 해제될 수 있기 때문입니다!



## 3. 미소유참조(unowned)

미소유 참조도 해당 인스턴스의 소유권을 가지지 않으며 자신이 참조하는 인스턴스의 RC를 증가시키지 않습니다.   
그런데 미소유참조는 nil이 될 수 없습니다. 그렇게 때문에 옵셔널 타입이 될 수 없습니다! 아니 없었습니다.!
무슨말이냐면.. 원래는 옵셔널 타입이 아니었는데 Swift 5.0 부터는 옵셔널 타입이라고 합니다..!   
nil이 되지 않는데 옵셔널 타입인 이유가 있을지..모르겠습니다. 이 부분은 나중에 공부해서 따로 포스팅 남기겠습니다..!

**그럼 약한참조(weak)와 미소유참조(unowned)의 차이가 어떤 것이 있을까요?**

약한참조는 객체를 계속 추적하면서 객체가 사라지게 되면 nil로 바꾸게 됩니다.   
하지만 미소유참조는 객체가 사라지게 되면 댕글링 포인터가 남게 됩니다.   
댕글링 포인터를 참조하게 되면 크래쉬가 나면서 앱이 종료될것입니다!   
그렇기 때문에 미소유참조는 사라지지 않음이 보장되는 객체에만 사용해야합니다!!  

>댕글링 포인터(Dangling Pointer)   
>-> 이미 해제된 메모리를 가리키고 있는 포인터를 말합니다.

저는 아직 unowned를 사용해 본적은 없습니다.   

**아주 위험하기 때문입니다..!!**  

weak을 사용해야하는 것중 개발자가 객체의 수명을 완전히 제어할 수 있을 경우에 unowned를 사용하게 되면 옵셔널처리를 하지 않아도 되어서 코드가 간결해질 수 있을 것 같습니다.   
**하지만** 개발자가 객체의 수명을 완전히 제어한다는 것은 쉬운일이 아니고 제어가 가능하다고 해도 어느시점에 제어가 되지 않을 수 있으며 크래쉬가 날 수 있는 위험한 상황이기 떄문에 weak을 사용하는 것이 바람직 하다고 생각합니다!   


## 4. 강한순환참조(strong reference cycle)

이것 때문에 위에서 구구절절 설명을 했던 것 같습니다.   

강한참조만 사용한다면 서로 강한순환참조가 발생할 수 있습니다.  
그렇게 되면 [저번 포스팅](https://velog.io/@qudgh849/ARCAuto-Reference-Counting)에서 말했듯이! 영구적으로 메모리가 해제되지 않을 수 있습니다..!!  
그래서 순환참조가 일어나지 않도록 주의해야합니다..   

어떨때 순환참조가 일어날까요?    
애플 문서의 예제로 보겠습니다.

아래처럼 되면 우선 john 인스턴스와 Person은 강한참조를 하게되고, unit4A 인스턴스와 Apartment 또한 강한참조를 하게 됩니다.

```swift
class Person {
    let name: String
    init(name: String) { 
        self.name = name 
        print("Person init")
    }
    var apartment: Apartment?
    deinit { 
        print("\(name) is being deinitialized") 
    }
}

class Apartment {
    let unit: String
    init(unit: String) { 
        self.unit = unit 
        print("Apartment init")
    }
    var tenant: Person?
    deinit {
        print("Apartment \(unit) is being deinitialized") 
    }
}

var john: Person?
var unit4A: Apartment?

john = Person(name: "John Appleseed")
unit4A = Apartment(unit: "4A")
```

그러고 나서 john의 apartment에 unit4A를 지정해주고, unit4A의 tenant에 john을 지정하게 되면
서로를 강한참조하게 됩니다!   
이렇게 되는 것을 강한순환참조(strong reference cycle)라고 합니다.

```swift
john!.apartment = unit4A
unit4A!.tenant = john
```

먼저 인스턴스를 만들었기 때문에 둘가 RC가 1이 됩니다.  
그러고 나서 서로를 다시 참조하기 때문에 RC가 1이 증가해 둘다 2가 됩니다.
그렇게 된다면 아래와 같이 두 인스턴스에 nil을 할당하여도 메모리가 해제되지 않습니다..!

```swift
john = nil
unit4A = nil
```

실제로 실행을 해보게되면 init에 대한 프린트는 뜨지만 deinit에 대한 프린트는 뜨지 않습니다!

이유는 이미 둘의 RC는 2이며 각 변수에 nil을 할당하여도 RC는 1만 감소하기 떄문에 둘의 RC는 1이 되어 메모리가 해제되지 않고 계속 살아있게 됩니다!  
그런데 따로 RC를 감소시킬 수 없기 때문에 영구적으로 해제되지 않고 남게 되어 메모리 누수를 발생시킵니다.

어떻게 해결할 수 있을까요?

바로 weak을 사용하면 됩니다!!  

위의 코드를 수정해보겠습니다.
Person의 apartment 프로퍼티를 weak으로 선언해주었습니다.  
이렇게 하니까 두 객체 모두 메모리가 해제되어 deinit에 대한 프린트가 뜨게 됩니다~  
(참고로 deinit은 해당 객체가 메모리에서 해제될 때 호출되는 메서드입니다.)

```swift
class Person {
    let name: String
    init(name: String) { 
        self.name = name 
        print("Person init")
    }
    weak var apartment: Apartment?
    deinit { 
        print("\(name) is being deinitialized") 
    }
}

class Apartment {
    let unit: String
    init(unit: String) { 
        self.unit = unit 
        print("Apartment init")
    }
    var tenant: Person?
    deinit {
        print("Apartment \(unit) is being deinitialized") 
    }
}
```

이렇게 순환참조이지만 약한참조로 선언되어 RC를 증가시키지 않는 것을 약한순환참조라고 합니다.   


**weak을 어디에 선언해주어야 강한순환참조가 일어나지 않을까요?**

위에는 Person의 프로퍼티에 weak을 선언해주었습니다.  
위 코드에서는 Person, Apartment 둘의 수명주기가 똑같기 때문에 아무곳에 해주어도 됩니다.  
Apartment의 프로퍼티에 weak을 선언해 주어도 메모리가 잘 해제 됩니다.

그런데 둘의 수명주기가 다를 경우에는 수명이 더 짧은 인스턴스를 가리키는 것을 weak으로 선언해야합니다.   
대상 인스턴스를 참조하고 있는 중에 언제든지 메모리에서 사라질 수 있기 때문입니다.