## 공부한 것

1. View Modifier
    - 스유에서 사용
    - .customModifier() 이런식으로 만들어서 커스텀한 스타일 등을 정의하여 필요할 때 사용할 수 있다.
    - [https://eunjin3786.tistory.com/334 ]()
2. View Builder
    - HStack, VStack처럼 괄호를 열고 뷰에 관한 것을 정의한다.
    - 개발자가 원하는대로 동작하는 ViewBuilder를 만들 수 있다.
3. AppStorage
    - iOS 14+
    - 저장소 개념
    - 앱 내에서 범용적으로 쓸 수도 있고, 앱을 껏다 켜도 데이터가 보존된다.
    - UserDefaults의 SwiftUI 버전
4. AsyncImage
    - iOS 15+
    - AsyncImage(url: url)
    - url만 넣어 주면 알아서 비동기로 가져온다.
5. Material
    - iOS 15+
    - Background blur 효과
    - [https://seons-dev.tistory.com/entry/SwiftUI-Material-background-blur-%ED%9A%A8%EA%B3%BC ]()
6. UIViewRepresentable
    - 스유로 UIKit을 완벽히 대처하지 못하는 부분을 UIKit을 사용하여 구현해야 할 때 사용한다.
    - UIKit을 SwiftUI에 맞게 wrapping해주는 기능을 하는 프로토콜
    - makeUIView,  updateUIView메서드 구현해서 사용한다. (UIViewRepresentable 프로토콜 필수 메서드) 
7. UIViewControllerRepresentable
    - UIViewRepresentable과 같다. 
    - 이건 뷰컨을 만든다.
8. preferenceKey
    - Key-Value 방식으로 하위뷰의 정보를 상위 뷰에 전달할 수 있는 수단
    - 프로토콜