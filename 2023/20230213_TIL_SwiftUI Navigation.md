# SwiftUI Navigation

## 문제

UIHostingController를 사용하여 PagerView를 구현하였으나 iOS 15 이하에서 네비게이션이 내려 앉는 이슈가 있었다.

## 원인
SwfitUI 내부에 UIKit의 NavigationController가 있었다,    
그래서 navigationController를 override 하여 nil로 만들어 주었다.

## 해결방법

```swift
override var navigationController: UINavigationController? {
    nil
}
```