# Swift macro

## 특징
1. Swift 5.9 도입
2.  코드 조각을 정의 하여 여러 곳에서 재사용할 수 있도록 해줌
3.  컴파일 시간에 반복코드를 생성하고 보일러 플레이트 코드를 제거하여 코드의 가독성을 높힐 수 있다.
4.  다른 개발자들과 공유할 수 있다.


## 사용법

### 선언부
``` swift
@freestanding(expression)
public macro stringify<T>(_ value: T) -> (T, String) = #externalMacro(module: "MyMacro_exampleMacros", type: "StringifyMacro")
```

### 구현부
```swift
public struct StringifyMacro: ExpressionMacro {
    public static func expansion(
        of node: some FreestandingMacroExpansionSyntax,
        in context: some MacroExpansionContext
    ) -> ExprSyntax {
        guard let argument = node.argumentList.first?.expression else {
            fatalError("compiler bug: the macro does not have any arguments")
        }

        return "(\(argument), \(literal: argument.description))"
    }
}
```

### 사용
```Swift 
let a = 17
let b = 52

let (result, code) = #stringify(a + b)

print("The value \(result) was produced by the code \"\(code)\"")
```

## 느낀점
아직 제대로 알지 못하고 예제 정도만 본 정도라서 정확히는 알 수 없지만 잘 사용한다면 분명 좋은 기능이 될 것으로 보인다.
라이트한 라이브러리라고 봐도 되려나..
근데 자세히 보진 못했지만 쏘 디피컬트한 향기가 난다..
나머지 자세한 부분은 내일 마저 봐야겠다..
겁나기도 하고 기대되기도 하는군