# SwiftUI - Property Wrapper

## @State

### 설명
- 구조체는 값타입이기 때문에 구조체의 프로퍼티의 값을 변경할 수 없지만  @State를 이용하여 구조체 내의 값을 변경할 수 있습니다.
-  SwiftUI의 뷰는 구조체로 되어있어서 언제든 소멸되고 재생성 됩니다. 그래서 @State를 사용해서 계속해서 변경 가능한 변수를 만듭니다.
-  String, Int, Double, Bool 등과 같이 단순한 타입에만 사용하는 것이 좋습니다.
-  보통 @State는 private로 선언되고, 다른 뷰와 공유되지 않고 한정된 용도로만 사용하는 것이 좋습니다. 다른 뷰와 공유하려면 @ ObservedObject나 @EnvironmentObject를 사용하는 것이 좋습니다.

### 예시
```swift
struct ContentView: View {
    @State private var isShowSecondaryView = false
    
    var body: some View {
        Text("Hello, world!")
            .padding()
            .onTapGesture {
                self.isShowSecondaryView = true
            }
            .sheet(isPresented: $isShowSecondaryView, onDismiss: nil) {
                SecondaryView(isShowTaskDetailView: $isShowSecondaryView)
            }
    }
}
```

## @Binding

### 설명
- 부모 뷰의 @State로 래핑된 값을 바인딩하여 값을 변경하고 싶을 때 사용합니다.
- 아래의 예시를 보면 부모뷰의 `isShowTaskDetailView`을 바인딩하여 값을 `false`로 바꾸어 뷰를 닫습니다.

### 예시
```swift
struct SecondaryView: View {
    @Binding var isShowTaskDetailView: Bool
    
    var body: some View {
        Button("닫기") {
            self.isShowTaskDetailView = false
        }
    }
}
```


## @ObservedObject vs @StateObject

### ObservableObject 프로토콜
- Combine 프레임워크의 일부입니다.
- ObservableObject 프로토콜을 준수한 클래스는 objectWillChange.send() 메서드를 사용할 수 있습니다.
- send() 메서드는 변경된 사항이 있음을 알려주는 메서드 입니다.
- 사용하는 변수가 적다면 send() 메서드를 직접 호출하여 사용할 수도 있겠지만, 변수가 많아지고 복잡해질 때 @Published를 사용하면 send() 메서드를 자동으로 호출하여 뷰를 다시 그리도록 해줄 수 있습니다.


### ObservedObject 설명
- 보통은 ViewModel을 선언할 때 사용하는 프로퍼티입니다.
- ObservableObject 프로토콜을 준수하는 타입에서 사용할 수 있습니다.
- 간단히 정리하자면 값이 변경되면 뷰에게 값이 변경되었음을 알려주고 뷰를 다시 그리도록 하는 기능을 gkqslek.
- @ObservedObject로 선언하게 되면 그 값의 수명은 뷰의 생명주기를 따르게 되어서 뷰가 해제되면 값 또한 초기화 됩니다..


### @StateObject 설명
- 하는 일은 @ObservedObject와 같습니다.
- iOS 14 이상부터 사용가능합니다.
- ObservableObject 프로토콜을 준수하는 타입에서 사용할 수 있습니다.
- @ObservableObject와 달리  @StateObject는 하나의 객체로 만들어지고 별도의 메모리 공간에 저장되어 보관되기 때문에 뷰의 생명주기와 상관 없이 값이 최기화 되지 않고 유지 됩니다.



## @EnvironmentObject
### 설명
- 앱 공통으로 공유되는 데이터에 사용됩니다. (앱의 많은 뷰에서 공유해야하는 데이터일  때 사용하면 좋습니다.)

### 코드
```swift
// SceneDelegate.swift
func scene(_ scene: UIScene, willConnectTo session: UISceneSession, options connectionOptions: UIScene.ConnectionOptions) {
    guard let windowScene = scene as? UIWindowScene else {
        return
    }
    // 공유할 인스턴스 생성
    let taskViewModel = TaskListViewModel()
    // 위에서 생성한 인스턴스를 첫 뷰의 environmentObject로 설정
    let contentView = ProjectManagerMainView().environmentObject(taskViewModel)
    let window = UIWindow(windowScene: windowScene)
    
    // root뷰 설정
    window.rootViewController = UIHostingController(rootView: contentView)
    self.window = window
    window.makeKeyAndVisible()
}

// @EnvironmentObject를 사용하여 선언
struct ProjectManagerMainView: View {
	@EnvironmentObject private var taskListViewModel: TaskListViewModel
}
```