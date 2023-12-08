
# SwiftUI Coordinator


## 생각
SwiftUI에서 화면이동을 좀 더 간단하게 할 수 있을까 생각을 해보고 조사를 해봤다.   
순수 SwiftUi를 사용하여 뷰들의 이동을 한곳에서 관리할 수 있을까 생각을 해보았다.   
그래서 구글링을 하던 도중 브랜디 블로그에서 좋은 예시를 발견하여 예제를 따라해 보았다.   

##. 기능
- 푸시
- 루트로 이동

##.개선사항
연관값을 사용하여 뷰를 이동할 떄 데이터를 전달 해줄수도 있고,   
앱이 커지면 Coordinator를 하나로 관리하는 것이 아니라 Coordinator도 분리하여 관리하면 더 좋겠다고 생각했다.  
이 부분 외에 여러부분을 더 개선하여 새로운 프로젝트를 진행할 때 적용해보려고 한다.   

## 예제 코드 

```swift
final class Coordinator: ObservableObject {
    @Published private var navigationTrigger = false
    @Published private var rootNavigationTrigger = false
    
    var destination: Destination = .aView
    private var isRoot: Bool
    private var cancellable: Set<AnyCancellable> = []
    
    init(isRoot: Bool = false) {
        self.isRoot = isRoot
        
        if isRoot {
          NotificationCenter.default.publisher(for: .popToRoot)
            .sink { [unowned self] _ in
              rootNavigationTrigger = false
            }
            .store(in: &cancellable)
        }
    }
    
    @ViewBuilder
    func navigationLinkSection() -> some View {
        NavigationLink(isActive: Binding<Bool>(get: getTrigger, set: setTrigger(newValue:))) {
            destination.view
        } label: {
            EmptyView()
        }
    }
    
    private func getTrigger() -> Bool {
        isRoot ? self.rootNavigationTrigger : self.navigationTrigger
    }
    
    private func setTrigger(newValue: Bool) {
        if isRoot {
            rootNavigationTrigger = newValue
        } else {
            navigationTrigger = newValue
        }
    }
    
    func push(destination: Destination) {
        self.destination = destination
        if isRoot {
            rootNavigationTrigger.toggle()
        } else {
            navigationTrigger.toggle()
        }
    }
    
    func popToRoot() {
        NotificationCenter.default.post(name: .popToRoot, object: nil)
    }
}

extension Notification.Name {
    static let popToRoot = Notification.Name("PopToRoot")
}

```

```swift
enum Destination {
    case aView
    case bView
    case cView
    
    @ViewBuilder
    var view: some View {
        switch self {
        case .aView:
            AView()
        case .bView:
            BView()
        case .cView:
            CView()
        }
    }
}
```

```swift
struct AView: View {
    @StateObject var coordinator = Coordinator(isRoot: true)
    
    var body: some View {
        NavigationView {
            VStack {
                Text("AAA")
                
                Button("go BBB") {
                    coordinator.push(destination: .bView)
                }
                
               Button("Pop to root") {
                	coordinator.popToRoot()
            	}
                
                coordinator.navigationLinkSection()
            }
        }
    }
}
```

## 참고
[브랜디 블로그](https://labs.brandi.co.kr//2022/12/12/leehs81.html)