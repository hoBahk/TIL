## Any
- Any는 함수타입을 포함하여 모든 타입의 인스턴스를 나타낼 수 있다.
- value 타입(구조체, 열거형), Reference 타입(클래스, 클로저)이건 상관 없이 저장할 수 있다.
- 한마디로 정리하면 Any는 모든 타입 포함할 수 있는 범용 타입이다.


## AnyObject
- AnyObject는 모든 클래스 타입의 인스턴스를 나타낼 수 있는 프로토콜이다.
- AnyObject로 선언 시, 클래스 타입만 저장할 수 있다.
- 따라서 클래스 타입이 아닌 구조체, 열거형, Reference Type인 클로저는 AnyObject에 해당하지 않는다.


## 장점
- Any, AnyObject는 모든 타입, 모든 클래스 타입을 저장할 수 있다.


## Any, AnyObject의 타입 캐스팅 ( 단점 )

- 런타임 시점에 타입이 결정나기 때문에, 만약 오류가 난다면 런타임 오류로 빠지게 됨
- 매번 타입 체크 및 형변환을 해야하기 때문에 필요에 의한 것이 아니라면 사용하지 않는 것이 좋다.
- 그럼에도 불구하고 사용한다면 switch문을 활용하여 각 타입마다 as를 통해 캐스팅이 성공하면 case문이 실행되게 할 수 있다.


### as를 이용한 타입 매칭(switch)

```swift
for thing in things {
    switch thing {
    case _ as Int:
        print("Int Type!")
    case _ as Double:
        print("Double Type!")
    case _ as String:
        print("String Type!")
    case _ as Human:
        print("Human Type")
    case _ as () -> ():
        print("Closure Type")
    default:
        print("something else")
    }
}
```