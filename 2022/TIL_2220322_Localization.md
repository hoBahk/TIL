# Localization - 다국어처리 (변수 있을 때)


## Strings 파일

Strings 파일에 넣는다.

![](https://images.velog.io/images/qudgh849/post/d70e992f-7213-43f7-8004-90c0e0410e65/image.png)

저기 %@, %d에 변수가 들어간다.

문자열 포맷에 맞게 넣어주어야 한다.
자주 사용하는 것들

%@ -> String
%% -> Character
%d -> Int
%u -> Unsigned Int (UInt)
%f -> Double

참고
[[애플문서] String Format Specifiers](https://developer.apple.com/library/archive/documentation/Cocoa/Conceptual/Strings/Articles/formatSpecifiers.html)


## 코드


```swift
let text = "My Name is %@, I`m from %@, I`m %d years old."
let localizedString = String(format: text, arguments: ["hoBhak", "Korea", 19])
label.text = localizedString
```

extension으로 따로 분리
```swift
extension String {
    func localized(comment: String = "") -> String {
        return NSLocalizedString(self, comment: comment)
    }
    
    func localized(with arguments: [CVarArg] = [], comment: String = "") -> String {
        return String(format: self.localized(comment: comment), arguments: arguments)
    }
}
```

호출
```swift
let text = "My Name is %@, I`m from %@, I`m %d years old."
label.text = text.localized(with: ["hoBhak", "Korea", 19])
```

## 실행화면

<p align="center"><img src=https://images.velog.io/images/qudgh849/post/356a5946-2146-4515-9f61-d16bba04c66e/image.png height="100px" width="300px"></p>
