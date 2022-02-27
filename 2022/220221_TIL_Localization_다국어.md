
## 코드로 다국어 처리하기

먼저 Strings File로 이름이 Localizable인 파일을 만들어 주세요.
(파일 이름을 Localizable로 해야지만 된다고 합니다..!)

![](https://images.velog.io/images/qudgh849/post/28c7eb59-5548-4aa6-9ca4-2f53b98ff15a/image.png)

그리고 해당 파일의 인스펙터에서 `Localize..` 버튼을 클릭해주세요!

![](https://images.velog.io/images/qudgh849/post/1746e37c-7f62-46ec-920d-1d1bcf4ab759/image.png)

그러면 기본으로 English만 설정되어 있습니다.

다른 언어도 넣어보겠습니다.

Project의 info에서 Localizations에서 + 버튼을 눌러 언어를 추가해줍니다.

![](https://images.velog.io/images/qudgh849/post/0c092e9e-c847-41f6-b2ff-824290945bcf/image.png)

코드로 해줄것이기 때문에 Localizable.strings만 선택하고 Finish버튼을 눌러줍니다.

![](https://images.velog.io/images/qudgh849/post/6abd2498-82fd-4ee5-86b2-4830a9f0ae19/image.png)

그러면 추가해준 언어가 보입니다!
![](https://images.velog.io/images/qudgh849/post/3a500beb-2d7d-4c99-a0a0-d63d94061020/image.png)

디렉터리 목록에도 파일이 추가되어 있습니다.
![](https://images.velog.io/images/qudgh849/post/64e4c435-3b53-4b10-b578-4b83162c97a2/image.png)

파일을 보면 비어있을거에요!
여기에 어떻게 다국어 처리를 할지 적어주면 됩니다.
아래의 형식으로 적어줍니다!

**"key" = "value";**


저는 이렇게만 적어주겠습니다.
![](https://images.velog.io/images/qudgh849/post/fa7c8a94-6885-434a-ac4a-040f4605822d/image.png)


viewController에서 label의 텍스트로 확인을 해보겠습니다.

다국어를 적용하려면 `NSLocalizedString`를 사용합니다.
매번 써주기 귀찮으니까 Extension으로 만들어 주겠습니다.

comment는 key에 대한 설명을 써놓는 곳입니다.
메서드에 기본값을 주어서 생략할 수 있도록 했습니다.

```swift
extension String {
    func localized(comment: String = "") -> String {
        return NSLocalizedString(self, comment: comment)
    }
}
```

그리고 이제 localized 함수를 호출해서 적용해보겠습니다.

```swift
override func viewDidLoad() {
    super.viewDidLoad()
    label.text = "TV".localized()
}
````
그러면 아래와 같이 "TV"가 아니라 "테레비"가 나오네요 ㅎㅎ
![](https://images.velog.io/images/qudgh849/post/19908dfb-f35d-4dc3-be91-7f0b04dd6b52/image.png)



### 참고 자료
[개발자 소들이 블로그 - Swift) Localizing - 다국어 처리하기](https://zeddios.tistory.com/1199?category=682195)