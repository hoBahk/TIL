# VoiceOver

장애인차별법 금지법으로 인한 대응을 해야한다.  
접근성을 지원해야한다.  

그래서 VoiceOver를 보게 되었다.   

VoiceOver는 시각 장애가 있거나 시력이 낮은 사용자가 iOS앱에 더 쉽게 접근할 수 있도록 지원해주는 애플의 접근성 기능 중 하나 이다.  
사람들이 화면을 보지 않아도 디바이스에서 인터페이스를 경험할 수 있도록 해주는 제스처 기반 화면 판독기이다.    

청각적으로 앱을 사용하는 동안 기능의 설명 등을 지원해준다.   


## 동작 방법

[VoiceOver 동작방법](https://support.apple.com/ko-kr/guide/iphone/iph3e2e2281/ios)

### 주요동작
- VoiceOver를 통해 해당 요소를 읽으려면 한 번 탭하거나 드래그로 그 위치에 올려두면 됩니다.  
- 이전이나 이후 UI 요소로 이동하려면 좌/우 스와이프
- 해당 요소를 일반적인 클릭 기능을 사용하려면 한 손가락으로 연속 두 번 탭
- 말하기를 중지하고 다시 시작하려면 두 손가락으로 탭
- 화면의 모든 보이스오버 설정된 내용들을 읽으려면 두 손가락으로 위로 스와이프
- 스크린 커튼(화면을 On/Off하는기능)을 켜거나 끄려면 세 손가락으로 세번 탭


## 구현 방법

UIKit은 VoiceOver를 모두 직접 추가해줘야한다.   
SwiftUI는 기본적으로 VoiceOver를 지원해 준다.   

[WWDC19 Accessibility in SwiftUI] (https://developer.apple.com/videos/play/wwdc2019/238/)

### SwfitUI VoiceOver Modifier
1. accessibilityLabel
- 읽혀질 해당 UI 요소의 설명을 지정할 수 있다.

2. accessibilityHidden
- 접근성을 하지 않도록 하는 모디파이어

3. accessibilityHint
- UI 요소의 기능 및 사용법을 설명해준다.

4. accessibilityValue
- UI 요소의 현재 값을 보이스오버로 나타내줄 수 있다.
