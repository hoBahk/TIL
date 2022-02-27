
# 학습내용

## ErrorMessage 처리

에러타입을 사용할 때 enum을 사용하도록 권장되고 있다.   
에러 메시지를 정의 할 때 rawValue를 사용하는 것 보다 localizedDescription을 통해 switch문을 사용하면 좋다.    
명확인 이름을 사용할 수 있고, 메서드를 구현하여 부가적인 정보도 사용할 수 있기 때문이다.


```swift
enum VendingMachine: Error {
    case noMoney
    case soldOut
    
}

extension VendingMachine: LocalizedError {
    var errorDescription: String? {
        switch self {
        case .noMoney:
            return "돈이 부족합니다."
        case .soldOut:
            return "품절입니다."
        }
    }
}
```


### LocalizedError
에러메시지를 관리할 때 사용하는 프로토콜이다.
오류설명, 오류가 발생한 원인 등을 설명할 수 있는 메시지를 제공한다.

[LocalizedError_공식문서](https://developer.apple.com/documentation/foundation/localizederror)


