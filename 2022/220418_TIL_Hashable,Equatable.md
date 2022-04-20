## Hashable

- 정수 해시값을 제공하여 그 자체로 유일하게 표현이 가능한 방법을 제공하는 프로토콜


**정수 Hash 값을 제공하는 타입**

같은 타입의 인스턴스인 a와 b의 경우, a==b이면 a.hashValue == b.hashValue

반대의 경우는 true가 아닐 수 있다. 동일한 hash 값을 가진 두개의 인스턴스가 서로 동일한 필요는 없다.

hash값은 프로그램 실행에 따라 동일하지 않을 수도 있다. 향후 실행에 사용할 hash값은 변경될 수 있기 때문에 저장하면 안된다.

구조체의 경우, 저장프로퍼티는 모두 Hashable을 준수해야 한다.

열거형의 경우, 모든 associated values은 모두 Hashable을 준수해야 한다.

표준 라이브러리의 많은 타입들 또한 hashable을 준수한다.

- string, integer, floating-point, boolean values, set 등

[Apple Developer Documentation](https://developer.apple.com/documentation/swift/hashable)

### HashValue(해시값)?

- 원본 데이터를 특정 규칙에 따라 처리하여 간단한 숫자로 만든 것
- 원본 데이터를 바탕으로 64bi의 int값으로 변환한 것

## Equatable

값의 비교가 가능함을 보장해주는 프로토콜

## Hashable은 왜 Equatable을 상속해야하는가

구별될 수 있는 값임으로 객체가 같다는걸 비교하기위해 Equatable 상속

Set, Dictionary의 key는 중복을 허용하지 않아야 함으로 Equatable을 상속하고 있는 Hashable을 준수해야 한다.

## 꼬리질문

- Hashable, Equatable 사용한 경험이 있는가?
    - 딕셔너리 키값을 hashable하게 만들어준다.
- Hashable을 따르는 타입은 무엇이 있을까요?
    - 표준 라이브러리의 많은 타입들 또한 hashable을 준수한다.
    - string, integer, floating-point, boolean values, set 등
- Hashable을 준수해야만 하는 것들이 무엇이 있을까?
    - 구조체의 경우, 저장프로퍼티는 모두 Hashable을 준수해야 한다.
    - 열거형의 경우, 모든 associated values은 모두 Hashable을 준수해야 한다.
    - Set, Dictionary의 key는 중복을 허용하지 않아야 함으로 Equatable을 상속하고 있는 Hashable을 준수해야 한다.