# Subscripts

## 설명

![](https://velog.velcdn.com/images/qudgh849/post/c5e6510a-b4c5-48fc-a714-5e48dba8aea7/image.png)

[Swift Language Guide(Subscripts)](https://docs.swift.org/swift-book/LanguageGuide/Subscripts.html)

Swift Language Guide의 설명에 따르면..   
클래스, 구조체, 열거형은 콜렉션, 리스트, 시퀀스의 요소에 접근하기 위한 첨자를 정의할 수 있다.   
첨자를 사용하여 별도의 설정 및 검색 방법 없이 인덱스 별로 값을 설정하고 검색할 수 있다.   
라고 되어있네요!

저는 처음에 이 글을 읽고 무슨 소린지 이해하지 못했습니다.. ㅎ

그래서 쉽게 이해할 수 있었던 것이  딕셔너리를 사용할 떄 key 값을 통해 value 값을 접근할 수 있잖아요!  
이것이 서브스크립트 입니다!

아래와 같이 bird라는 key값을 통해 value에 접근할 수 있게 만드는 것이 서브스크립트입니다.

```swift
var numberOfLegs = ["spider": 8, "ant": 6, "cat": 4]
numberOfLegs["bird"] = 2
```

배열도 서브스크립트가 있습니다.

```swift
let numbers = [1, 3, 5, 7, 9]
print(numbers[1]) // 3
```

이렇게 기존의 배열이나 딕셔너리 등에도 서브스크립트가 정의되어 있습니다.

이제 서브스크립트가 무엇인지 대충 이해가 가시나요?

## 문법

서브스크립트의 기능이 필요하다면 우리가 만들어서 사용할 수 있습니다.

그럼 문법에 대해서 알아보겠습니다.

- 서브스크립트의 문법은 연산 프로퍼티와 비슷합니다.
- getter, setter를 사용합니다.
- 서브스크립트는 읽고-쓰기(read-write), 읽기만(read only)만 가능합니다.   
- 읽기만 하려면 setter를 사용하지 않고, getter만 사용하면 됩니다.
- set의 대한 인자 값을 따로 지정하지 않으면 default 값으로 newValue를 사용합니다.

```swift
subscript(index: Int) -> Int {
    get {
        // 반환값
    }
    set(newValue) {
        // set 액션
    }
}
```


## 사용

예제를 통해서 보겠습니다.

fruitList인스턴스로 내부의 fruits 프로퍼티에 접근할 수 있게 됩니다.

```swift
class FruitList {
    private var fruits = ["apple", "banana", "melon", "tomato"]
    subscript(index: Int) -> String {
        get {
            return self.fruits[index]
        }
        set {
            self.fruits[index] = newValue
        }
    }
}

let fruitList = FruitList()
print(fruitList[0]).  // apple

fruitList[3] = "strawberry"
print(fruitList[3])   // strawberry
```


다른 예제를 볼까요!

String은 기본적으로 서브스크립트가 구현되어 있지 않기 때문에 이렇게 extension으로 지정해주면 String의 인덱스를 통해 사용할 수 있습니다!

```swift
extension String {
    subscript(idx: Int) -> String? {
        guard (0..<count).contains(idx) else {
            return nil
        }
        let index = index(startIndex, offsetBy: idx)
        return String(self[index])
    }
}

let alphabet = "abcdefg"
print(alphabet[0])
```
