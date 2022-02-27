
# 날짜, 숫자, 화폐를 지역화 하는 방법

## 날짜표기 지역화

날짜의 표기는 디바이스의 설정된 언어와 지역에 따라 변경됩니다.

```swift
@IBOutlet weak var dateLabel: UILabel!

func localizeDate() {
    let date = DateFormatter.localizedString(from: Date(), dateStyle: .medium, timeStyle: .short)
    
    dateLabel.text = date
}
```
**아래와 같이 언어와 지역에 따라 조금씩의 변화가 있습니다.**

ex) 한국어 & 대한민국
![](https://images.velog.io/images/qudgh849/post/3521360f-156f-4f3e-aaa5-632ddf630dc9/image.png)

ex) 한국어 & 미국
![](https://images.velog.io/images/qudgh849/post/988023b8-1564-4c88-9356-dd88c30422be/image.png)

ex) 영어 & 미국
![](https://images.velog.io/images/qudgh849/post/6cc7c0a3-5cb5-4fe8-93d8-f01d0ac97ef1/image.png)

ex) 영어 & 대한민국
![](https://images.velog.io/images/qudgh849/post/0c2db9f3-a43d-4820-ba53-0a4979ecfd94/image.png)



## 화폐 지역화

화폐는 디바이스의 설정된 지역에 따라 표기됩니다.

```swift
@IBOutlet weak var currencyLabel: UILabel!

func localizeCurrency() {
    let locale = Locale.current
    let price = 5743.85 as NSNumber
    let formatter = NumberFormatter()
    
    formatter.numberStyle = .currency
    formatter.currencyCode = locale.currencyCode
    formatter.locale = locale
    
    currencyLabel.text = formatter.string(from: price)
}
```

ex) 대한민국(원)
![](https://images.velog.io/images/qudgh849/post/6ba2669a-5bb1-43e0-b4ff-dc3f105be42f/image.png)

ex) 미국(달러)
![](https://images.velog.io/images/qudgh849/post/c51dd716-8f45-453e-9584-e869a85c142d/image.png)

ex) 영국(파운드)
![](https://images.velog.io/images/qudgh849/post/3dc035ee-2c77-4eff-80d0-7b20730457fb/image.png)


## 숫자표기 지역화

숫자표기는 디바이스의 설정된 지역에 따라 달라집니다.

```swift
@IBOutlet weak var numberLabel: UILabel!

func localizeNumbers() {
    let quantity = NumberFormatter.localizedString(from: 100000000000, number: .decimal)
    
    numberLabel.text = String.localizedStringWithFormat(quantity)
 }

```

그런데 대부분의 나라가 대한민국과 같이 1,000 단위로 쉼표를 찍습니다.
하지만 예외인 나라가 있습니다.
인도, 파키스탄, 방글라데시라고 합니다.

위의 나라들은 아래와 같이 표기 됩니다.

ex) 인도, 파키스탄, 방글라데시
![](https://images.velog.io/images/qudgh849/post/8b1786c9-7711-4aa2-9756-6b297354c9fc/image.png)

맨 오른쪽 세자리만 제외 하고 모두 두자리마다 쉼표를 찍습니다.