# Share Extension

- Share Extension은 공유 버튼을 눌렀을 때 해당 앱이 공유 앱 목록에 뜨고 해당 앱을 선택하면 연동할 수 있도록 하는 작은 앱을 말한다.

## Share Extension 생성
File - New - Target에 들어가서 Share Extension을 선택하여 생성한다.

## View
ShareExtension은 두가지 뷰로 구성할 수 있다.   
- 기본 제공 View  
- Custom View

### 기본제공 View
- Share Extension을 생성하면 파일이 생성되는데 주어진 메서드에 코드를 채워 넣으면된다.

```swift
class ShareViewController: SLComposeServiceViewController {

    // compose View가 나올 때 컨텐츠가 변경될 때마다 호출 ex) 텍스트 입력, 사진 변경 등
    // post(올리기) 버튼 활성화 할 때 사용
    override func isContentValid() -> Bool {
        self.textView.text.isEmpty ? false : true
    }

    // post(올리기) 버튼 눌렀을 떄 호출
    override func didSelectPost() {
        self.extensionContext!.completeRequest(returningItems: [], completionHandler: nil)
    }

    override func configurationItems() -> [Any]! {
        let item = SLComposeSheetConfigurationItem()!
        item.title = "Title"
        item.value = "value"
        item.tapHandler = {
            let viewController = ShareViewController()
            self.pushConfigurationViewController(viewController)
        }
        return [item]
    }

    // cancel(취소) 버튼 눌렀을 떄 호출
    override func didSelectCancel() {

    }
}
```

### Custom View
- Custom View는 ShareViewController 클래스를 생성하여 UIViewController를 상속하면 된다.
- Share Extension을 생성하면 스토리보드도 같이 만들어 지는데 스토리보드로 뷰를 구현하여도 되고 아니면 코드로 뷰를 구현하여도 된다.
- 아래의 코드를 사용하여 SwiftUI로 뷰를 구현할 수도 있다.

```swift
let childView = UIHostingController(rootView: ShareSwiftUIView( ))
self.addChild(childView)
childView.view.frame = self.view.bounds
self.view.addSubview(childView.view)
childView.didMove(toParent: self)
```


### 공유된 항목 가져오기
- 공유하려는 아이템을 가져오기 위해 아래와 같은 코드가 필요하다.
- loadItem 메서드는 sync/await 과 클로저 두가지 방법이 제공된다.

```swift
override func viewDidLoad() {
    guard let extensionItems = extensionContext?.inputItems as? [NSExtensionItem] else {
        return
    }
    
    for extensionItem in extensionItems {
        guard let itemProviders = extensionItem.attachments else {
            return
        }
        
        for itemProvider in itemProviders {
            // 공유할 item이 있는지 확인
            guard itemProvider.hasItemConformingToTypeIdentifier(kUTTypeImage as String) else {
                return
            }
            
            // 공유할 item 불러오기
            Task {
                let result: NSSecureCoding = try await itemProvider.loadItem(forTypeIdentifier: kUTTypeImage as String)
                guard let url = result as? URL else {
                    return
                }
                self.setUI(url: url)
            }
        }
    }
}
```


## Data Share
### Remote API
- 이 방식을 사용하면 메인 앱과 확장앱 사이에 데이터를 공유 하지 않고 각자 API를 호출하여 사용한다.


### UserDefaults
- UserDefaults를 사용하여 서로 데이터를 공유 한다.
- 앱 그룹을 활성화 하여야 한다.
``` swift
extension UserDefaults {
  static let group = UserDefaults(suiteName: "group.com.your.domain")!
}

// 데이터 set
UserDefaults.group.set("objectToShare", forKey: "keyForTheObject")

// 데이터 get
let object = UserDefaults.group.object(forKey: "keyForTheObject")
```

### Shared container
- 앱 그룹을 활성화 하여 파일 시스템의 전체 폴더인 공유컨테이너도 사용할 수 있다.
- 컨테이너에 대한 액세스는 스레드로부터 안전하지 않다.

### KeyChain
- 민감한 데이터를 공유하는 경우에 유용하다.


참고 - [Share Extension 데이터 공유](https://dmtopolog.com/ios-app-extensions-data-sharing/)
