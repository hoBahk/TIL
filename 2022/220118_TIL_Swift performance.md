# swift performance
Allocation, Reference Counting, Method Dispatch
세가지를 기준으로 하는 성능의 개선을 주재로 한다.

## Allocation
Stack에 저장하느냐 Heap에 저장하느냐인데,
성능적으로 stack에 저장하는 것이 좋다.
그래서 heap에 저장하는 String같은 것도 줄일 수 있으면 줄이는 것이 좋다.
보기가 정해져 있다면, 열거형으로 정의하여 사용할 수 있다.

## Reference Counting
참조카운드는 적을수록 좋다. 
class를 사용하게 되면 참조카운드가 하나씩 늘어난다. 
순수한 struct는 참조카운트가 없어 좋지만, struct안에 property가 참조값이라면 참조카운트는 증가한다.

## Method Dispatch
Method Dispatch는 정적인 것과 동적인 것이 있다.
물론 정적일수록 성능면에서는 좋다.
방법으로는 구조체에서는 상속되지 않기때문에 안전하다.
프로토콜에서 프로토콜 본문(no extension context)에서 함수를 선언하게 되면 어떤 곳에서 이 프로토콜을 채택하는지 런타임중에 확인해야하기 때문에 동적으로 작용한다. 프로토콜의 기본구현에서 함수를 정의한다면 모두 동일하기 때문에 정적이다.
프로토콜을 채택하여 사용하는 메서드는 동적으로 작용된다.
클래스의 extension에서 정의 된다면 동적이다.
개발자가 명시적으로 동적으로 작용할 수 있게 하려면 static이나 class를 붙여서 사용한다.