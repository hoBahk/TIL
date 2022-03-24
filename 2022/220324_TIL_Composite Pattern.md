# Design Pattern - Composite Pattern

## 설명

복합객체와 단일객체를 동일하게 취급하고 전체-부분 객체 관계사이의 동일한 인터페이스 정의하여 하나의 타입처럼 사용할 수 있도록 하고 싶을 때 사용할 수 있는 패턴이다.

![](https://images.velog.io/images/qudgh849/post/f55c94c8-da4e-42f9-ae5e-79e7a7f7037f/image.png)
[출처: raywenderlich](https://www.raywenderlich.com/books/design-patterns-by-tutorials/v3.0/chapters/20-composite-pattern)

Leaf: 단일객체, 하위 객체가 없는 가장 기본 단위의 객체.  
Composite: 복합객체, container라고도 하며 단일객체 또는 복합객체를 하위 객체로 갖는 객체   
Protocol: 단일객체와 복합객체의 통합할 기능을 정의하는 프로토콜.  

복합객체는 단일객체를 컬렉션의 형태로 가지고 있고 복합객체의 메서드를 호출하면 반복문을 돌면서 단일객체의 메서드를 호출하도록 하여 복합객체에 있는 모든 단일객체의 메서드를 실행시킬 수 있다.

## 예제 코드
playground에서 실행

```swift 
// Component
protocol FileManageable {
    func open()
}

// Leaf
// 단일 객체
class File: FileManageable {
    var name: String
    
    init(name: String) {
        self.name = name
    }
    
    func open() {
        print("File:\(name)이 열렸습니다.")
    }
}

let musicFile = File(name: "music")
let documentFile = File(name: "document")

// Compostion
// 복합 객체
class Directory: FileManageable {
    var files: [FileManageable] = [musicFile, documentFile]
    
    func open() {
        for file in files {
            file.open()
        }
    }
}

let diretory = Directory()
diretory.open()
```

## 사용하면 좋을 때
- 전체 - 부분관계의 트리구조를 구현할 때 
- 단일객체와 복합객체의 처리방법이 동일할 때
-  단일객체와 복합객체를 동일하게 취급하고 싶을 때


## 장점
- 트리구조를 만들 수 있다.
- 클라이언트의 코드가 단순해진다.
- 정의한 protocol의 인터페이스를 따르는 새로운 객체를 쉽게 추가할 수 있다.
