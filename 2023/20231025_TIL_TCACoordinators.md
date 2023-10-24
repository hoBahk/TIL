# TCACoordinators

[TCACoordinators - Github](https://github.com/johnpatrickmorgan/TCACoordinators)

TCACoordinators는 TCA에서 Coordintor 패턴을 사용할 수 있도록 도와주는 라이브러리이다.   
해당 라이브러리를 사용하면 Coordinator 패턴을 쉽게 사용할 수 있게 해준다.   
여러 화면 전화의 메서드를 구현해 사용할 수 있도록 제공해준다.    

## 리드미

### 설명
TCA를 사용한 SwfitUI 환경에서 탐색에 대한 유연한 접근 방식을 제공한다.  
단일 상태로 복잡한 탐색 및 프레젠테이션 흐름을 관리할 수 있다.   
각 모튤별 격리된 화면 기능을 구현할 수 있다.    
ForEach, ifCaseLet 및 SwiftchStore와 같은 TCA의 기존 도구를 SwiftUI에서 탐색을 처리하는 새로운 접근방식과 결합하여 사용한다.   

### 장점
- 앱에 깊이 중첩된 탐색 경로에 대한 딥링크 지원
-  다양한 탐색 컨텍스트에서 손쉽게 화면 기능을 재사용
-  탐색 스택의 루트 화면이나 특정 화면으로 쉽게 돌아갈 수 있다.
-  모든 네비게이션 로직을 한곳에 보관
-  앱의 네비게이션을 여러 개의 재사용 가능한 코디네이터로 분할하여 함께 구성
-  단일 시스템으로 푸시 네비게이션과 모달 프레젠테이션을 통합


화면 배열을 중첩된 탐색 링크 및 프레젠테이션 호출의 계층 구조로 변환하여 작동하므로 다음과 같다.    

- UIKit에 전혀 의존하지 않는다.
- AnyView를 사용하여 erase 화면을 입력하지 않는다.
- NavigationView를 처음부터 다시 만들려고 하지 않는다.




