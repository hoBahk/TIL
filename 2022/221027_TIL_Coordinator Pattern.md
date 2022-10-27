# Coordinator Pattern

참고    
- [zeddios.medium.com/coordinator-pattern](https://zeddios.medium.com/coordinator-pattern-bf4a1bc46930)   
- [SwiftUI-Coordinator Pattern](https://betterprogramming.pub/an-introduction-to-coordinator-pattern-in-swiftui-38e5b02f031f)      



- Coordinator Pattern은 2015년, Soroush Khanlou가 The Coordinator라는 글을 쓰면서 소개된다.
- Coordinator는 하나 이상의 뷰 컨트롤러들에게 지시를 내리는 객체이며, 여기서 말하는 지시는 View의 트랜지션을 의미한다.
- Coordinator는 앱 전반에 있어 화면 전환 및 계충에 대한 흐름을 제어하는 역할을 한다.


## 기존의 문제점

- ViewControlle만을 사용하면 flow logic과 business logic이 얽혀있다.
- 간단한 앱에서는 문제가 없지만 복잡한 앱이 되면 하나의 VIewController가 새로운 방식 또는 새로운 위치에서 사용될 수 있다. (중복 사용될 수 있다.)
- ViewController가 사용자 흐름을 처리하는 것은 ViewController의 범위를 벗어난다.



## 해결

- 흐름을 관리할 수 있는 객체를 생성한다. (Coordinator)
- AppDelegate(SceneDelegate)는 AppCoordinator를 유지하며, 모든 Coordinator에는 일련의 하위 Coordinator가 있다.



## 장점

### 각 ViewController의 고립
- ViewController는 데이터를 표시하는 방법 외에는 아무것도 모르며, 어떤일이 발생할 때마다 Delegate에게 알라지만 Delegate가 누군지는 알 수 없다.
- 전에는 사용자가 iPad를 사용중인지 이런것들을 분기처리해야 했지만 Coordinator패턴을 사용하면 ViewController에서 이런 일을 하지 않아도 된다.

### ViewController의 재사용
- ViewController는 어떤 Context로 표시될지, 버튼이 어떤 용도로 사용될지에 대해 아무것도 가정하고 있지 않다.
- 앱의 iPad 버전을 작성하는 경우 Coordinator만 교체해야하며 모든 ViewController를 재사용할 수 있다.

### 앱의 모든 작업과 하위 작업은 전용 캡슐화 방법을 제공

### Coordinator는 display-binding을 side effects와 분리
- ViewController를 표시할 때 ViewController가 데이터를 엉망으로 만들지 여부에 대해 걱정할 필요가 없어진다.
- 읽기 및 디스플레이만 할 수 있으며 데이터를 쓰거나 손상시키지 않는다.

### Coordinator는 완전히 제어할 수 있는 객체
- ViewDIdLoad가 호출될 떄까지 기다리지 않고 전적으로 show를 제어할 수 있다.
- 호출을 받는 대신 호출을 시작한다.

