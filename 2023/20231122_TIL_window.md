# 최상위 윈도우 띄우기

팝업이나 토스트뷰 등을 뷰의 최상위에 띄우기 위한 코드


```swift
var toastWindow: UIWindow?
    
func showToastWindow<Content: View>(_ content: Content) {
    DispatchQueue.main.async {
        let windowScene = UIApplication.shared
            .connectedScenes
            .filter { $0.activationState == .foregroundActive
                || $0.activationState == .foregroundInactive }
            .first
        
        if let windowScene = windowScene as? UIWindowScene {
            self.toastWindow = ToastWindow(windowScene: windowScene)
            self.toastWindow?.backgroundColor = .clear
            self.toastWindow?.rootViewController
            = UIHostingController(rootView: content)
            self.toastWindow?.frame = UIScreen.main.bounds
            self.toastWindow?.rootViewController?.view.backgroundColor = .clear
            self.toastWindow?.isUserInteractionEnabled = false
            self.toastWindow?.makeKeyAndVisible()
            self.toastWindow?.isHidden = false
        }
    }
}

private class ToastWindow: UIWindow { }
```



