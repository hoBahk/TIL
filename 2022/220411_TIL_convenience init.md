

# convenience init


우리가 일반적으로 쓰는 init은 Designated init라고 합니다.

하지만 그냥 init으로 부르겠습니다.

init은 모든 프로퍼티가 초기화 될 수 있도록 해주어야합니다.

그럼 convenience init은 뭘까요?

convenience는 사전적 의미로 편의, 편리라는 뜻을 가지고 있습니다.

편리하게 쓸 수 있는 이니셜라이저?라고 생각할 수도 있을 것 같습니다.

convenience init은 그냥 init의 파라미터 중 일부를 기본값으로 설정할 수 있습니다.

어떻게 하냐면 convenience init안에서 그냥 init을 호출해주는 방식입니다.


## 예제

예제로 보시죠!!

convenience init은 아래 처럼 사용합니다!
기존에 있던 initi을 호출해서 값을 기본값처럼 애초에 지정해줄 수 있습니다.

```swift
class Person {
    let name: String
    let age: Int
    let height: Double
    
    init(name: String, age: Int, height: Double) { 
        self.name = name 
        self.age = age
        self.height = height
    }
    
    convenience init(name: String) {
        self.init(name: "hoBahk", age: 24, height: 178.2)
    }
}
```
