# 220228_TIL_Local User Notification
User Notification은 Local과 remote가 있다. Local은 앱 내부에서 설정하여 보내는 노티이고 remote는 서버에서 보내는 것을 의미한다.
아래는 Local일 경우 노티를 승인하고 받는 방법이다. 노티를 받았을 때의 작업은 여러 방법이 있다. 하기나름..
Remote는 서버에서 노티를 던져준다. 그러면 그에 맞게 데이터를 받아 원하는 방식으로 처리해주면 된다. 
Remote는 다음에..

##  노티 받는 것을 승인
```swift
func requestNotificationAuthorization() {
    let authOptions = UNAuthorizationOptions(arrayLiteral: .alert, .badge, .sound)
    
    userNotificationCenter.requestAuthorization(options: authOptions) { success, error in
        if let error = error {
            print("Error: \(error)")
        }
    }
}
```

## 노티를 발생
```swift
func sendNotification(seconds: Double) {
    let notificationContent = UNMutableNotificationContent()
    
    notificationContent.title = "알림 테스트"
    notificationContent.body = "알림 테스트 바디"
    notificationContent.userInfo = ["target_view" : "yellow_view"]
    notificationContent.sound = UNNotificationSound.default
    
    let trigger = UNTimeIntervalNotificationTrigger(timeInterval: 5000, repeats: false)
    let request = UNNotificationRequest(
        identifier: "testUserNotification",
        content: notificationContent,
        trigger: trigger
    )
    
    userNotificationCenter.add(request) { errror in
        if let error = errror {
            print("noti Error: \(error)")
        }
    }
}
```

## 노티를 터치할 시 수행할 작업
```swift
extension AppDelegate: UNUserNotificationCenterDelegate {
    func userNotificationCenter(_ center: UNUserNotificationCenter, didReceive response: UNNotificationResponse, withCompletionHandler completionHandler: @escaping () -> Void) {
        let target = response.notification.request.content.userInfo["target_view"] as? String ?? ""
        guard let rootViewController = (UIApplication.shared.connectedScenes.first?.delegate as?
                SceneDelegate)?.window?.rootViewController else {
            return
        }
        guard let tabBarController = rootViewController as? UITabBarController else {
            return
        }
        tabBarController.selectedIndex = 1
        guard let navigationController = tabBarController.selectedViewController as? UINavigationController else {
            return
        }
        
        navigationController.topViewController?.performSegue(withIdentifier: target, sender: nil)
    }
    
    
    func userNotificationCenter(_ center: UNUserNotificationCenter, willPresent notification: UNNotification, withCompletionHandler completionHandler: @escaping (UNNotificationPresentationOptions) -> Void) {
        completionHandler([.banner, .badge, .sound])
    }
}
```