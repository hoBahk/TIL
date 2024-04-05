# Orientation

## 문제
UI가 포함된 SDK를 만들고 있는 와중 문제가 생겼다.   
카메라 화면이 있었는데 카메라 화면은 OS단에서 돌리는 Orientation을 막아야 한다는 것이다.  

## 해결
그래서 여러 방면으로 생각해보고 검색도 해보았는데 많은 글들이 Info.plist에서 설정하거나 AppDelegate에서 설정하는 등 App의 라이프사이클을 이용해서 막으려고 한다.  
하지만 내가 만들고 있는 것은 SDK... App의 라이프 사이클을 건드리면 안된다.   

그러다가 UIViewContorller에서 `supportedInterfaceOrientations` 프로퍼티를 사용하여 지원하는 Orientation을 제한할 수 있다는 것을 알게 되었다.  
지금 SDK에서 UI를 띄우는 방식은 UIViewController를 받아서 UIHostingController로 변환하고 present를 해주는 방식이다. (SDK는 SwiftUI로 되어 있다.)  

UIHostingController는 UIViewContorller를 상속하고 있기 때문에 `supportedInterfaceOrientations`프로퍼티가 있어 이것을 조정해서 문제를 해결하였다.    


```swift

struct ViewControllerHolder {
    weak var value: UIViewController?
}

struct ViewControllerKey: EnvironmentKey {
    static var defaultValue: ViewControllerHolder {
        return ViewControllerHolder(value: UIApplication.shared.windows.first?.rootViewController)
    }
}

extension EnvironmentValues {
    public var viewController: UIViewController? {
        get { return self[ViewControllerKey.self].value }
        set { self[ViewControllerKey.self].value = newValue }
    }
}

extension UIViewController {
    public func present<Content: View>(style: UIModalPresentationStyle = .automatic, @ViewBuilder builder: () -> Content) {
        let toPresent = CutsomHostingViewController(rootView: AnyView(EmptyView()))
        toPresent.modalPresentationStyle = style
        toPresent.rootView = AnyView(
            builder()
                .environment(\.viewController, toPresent)
        )
        NotificationCenter.default.addObserver(forName: Notification.Name(rawValue: "dismissModal"), object: nil, queue: nil) { [weak toPresent] _ in
            toPresent?.dismiss(animated: true, completion: nil)
        }
        self.present(toPresent, animated: true, completion: nil)
    }
}


class CutsomHostingViewController: UIHostingController<AnyView> {
    override var supportedInterfaceOrientations: UIInterfaceOrientationMask {
        return .portrait
    }
}

```