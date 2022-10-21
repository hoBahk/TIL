# ShareExtension

## UserDefaults

1. App Grouop 설정

Target - Signing & Capabilities - `+`button - App Groups 추가
App Group 설정

참고 - [Data Sharing Between App and App Extensions ](https://medium.com/macoclock/data-sharing-between-app-and-app-extensions-4ffd95bf87e0)


2. set UserDefaults

```swift
let savedata =  UserDefaults.init(suiteName: "group.aa.ShareExtension-Example")

func setUserDefaults(url: URL) {
    let urlString = String(describing: url)
    guard let data = try? Data(contentsOf: url) else {
        return
    }

    savedata?.set(urlString, forKey: "urlData")
    savedata?.set(data, forKey: "imageData")
    savedata?.synchronize()
}
```


3. get UserDefaults

```swift
func userDefault() -> String {
    let savedata =  UserDefaults.init(suiteName: "group.aa.ShareExtension-Example")
    
    if let url =  savedata?.string(forKey: "urlData") {
        text = String(describing: url)
        return url
    }
    return ""
}

func userDefaultImage() -> Data {
    let savedata =  UserDefaults.init(suiteName: "group.aa.ShareExtension-Example")
    if let data =  savedata?.data(forKey: "imageData") {
        return data
    }
    return Data()
}
```


## 팁
1. 이미지 공유할 때만 공유될 수 있도록 할 수 있는지?
	- 이미지만 공유하는 것은 찾지 못하고 텍스트를 지원하지 않도록 할 수는 있다.
	- Info.Plist에서 `NSExtensionActivationSupportsText`를 false로 해준다.

```
<key>NSExtensionActivationRule</key>
<dict>
    <key>NSExtensionActivationSupportsWebURLWithMaxCount</key>
    <integer>1</integer>
    <key>NSExtensionActivationSupportsImageWithMaxCount</key>
    <integer>5</integer>
    <key>NSExtensionActivationSupportsWebPageWithMaxCount</key>
    <integer>1</integer>
    <key>NSExtensionActivationSupportsText</key>
    <false/>
</dict>
```


2. 사파리에 있는 사진도 공유할 수 있는가?
	- 사진앱의 사진은 kUTTypeImage 타입으로 받을 수 있다.
	- 사파리의 사진은 kUTTypeURL 타입으로 받을 수 있다.