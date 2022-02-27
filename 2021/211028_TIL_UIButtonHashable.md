
# 

저희는 인스턴스의 identity를 비교하고 싶었어요. ===를 의도했습니다. 하지만, switch는 value를 통해 비교한다고 합니다. 그래서 의도와 다른 코드를 작성한 것이 되었네요.
하지만 기존 코드에서도 버튼이 식별이 잘 된 이유가 무엇인지 궁금하여 이런저런 테스트를 해 보았는데요,

let buttonA = UIButton()
let buttonB = UIButton()
이렇게 아무런 옵션도 안주고 동일한 방식으로 인스턴스를 생성했을 때, 두 인스턴스의 값이 다르다고 나오는점이 의아했습니다. 주소는 다르고 값은 같을 것이라고 예상했거든요.
인스턴스의 값이 왜 다른지에 대해 찾다 보니 UIButton은 Hashable하고, hashValue라는 프로퍼티가 각각 다르기에 다른 값으로 취급될 수도 있겠다는 생각도 해 봤습니다. 그렇다면 인스턴스마다 고유의 hashValue를 가질테니 값 비교를 통해서 버튼을 식별해 줘도 무방한 것 아닌가 하고 생각했지만, 인스턴스가 다르더라도 hashValue는 같을 수도 있다는 이야기를 봐서(공식문서는 아니고 Zedd님 블로그입니다), 결국 참조 비교(===)를 하는 것이 안전하다는 결론을 내렸습니다.

## 학습내용, 고민한 점
### UIButton

여러개의 `UIButton`을 입력 받아 switch문으로 입력에 따른 값을 리턴해주는 함수를 만들었다. 그런데 메이슨이 switch문에서 비교하는 것이 `==` 인지 `===` 인지 물어보셨다. 그래서 그것을 알아내기 위해 문서도 보고 구글링도하고 테스트를 해보았다.

그래서 찾아보고 갖은노력 끝에 switch는 ==로 비교를 한다는 것을 알아냈다.

그래서 switch로 해도 되는지 알아보았다.   
매개변수로 받아온 button과 UIButton으로 등록한 버튼들을 비교하였다.
`==`로 비교한 것은 값이 같은지를 비교하는 것인데, 모두 같았고, `===`은 참조하고 있는 것이 같은지를 보는 것인데 이것도 같았다.    
UIButton의 프로퍼티를 확인해보니 아주 많았다. 버튼의 속성도 있었고 여러가지가 있었는데 hashable, hashValue가 눈에 띄었다. UIButton은 Hashable하니까 고유한 hashValue를 인스턴스 마다 가지고 있겠구나 생각했다. 그래서 hashValue까지 비교해서 같은지 다른지 보기 때문에 값을 가지고 비교하는 `==`도 될거라고 생각했다. 그런데 다른인스턴스끼리 해시값이 같을 수도 있다는 글을 봤다. (Zeddios블로그에서) 그러면, hash값이 key값이 될 수 없다고 생각했다. 그래서 `==`는 정확하지 않아 ===로 비교할 수 있도록 if문으로 로직을 변경하였다.
```swift

private func matchJuiceMenu(with button: UIButton) throws -> JuiceMenu {
        switch button {
        case strawberryBananaJuiceOrderButton:
            return .strawberryBanana
        case mangoKiwiJuiceOrderButton:
            return .mangoKiwi
        case strawberryJuiceOrderButton:
            return .strawberry
        case bananaJuiceOrderButton:
            return .banana
        case pineappleJuiceOrderButton:
            return .pineapple
        case kiwiJuiceOrderButton:
            return .kiwi
        case mangoJuiceOrderButton:
            return .mango
        default:
            throw JuiceOrderError.invalidJuiceOrder
        }
    } 
    
    ```

그래서 아래와 같이 입력 받은 버튼(strawberryBananaJuiceOrderButton에 해당하는 버튼을 탭함)과 strawberryBananaJuiceOrderButton를 출력해보았다.

```swift
 private func matchJuiceMenu(with button: UIButton) throws -> JuiceMenu {
        print(button)
        print(button.hashValue)
        print(strawberryBananaJuiceOrderButton)
        print(strawberryBananaJuiceOrderButton.hashValue)
 }
 ```
 
 ```
 <UIButton: 0x15ff0a8b0; frame = (0 0; 277.667 66); opaque = NO; autoresize = RM+BM; layer = <CALayer: 0x600003c6f920>>
5904574640
Optional(<UIButton: 0x15ff0a8b0; frame = (0 0; 277.667 66); opaque = NO; autoresize = RM+BM; layer = <CALayer: 0x600003c6f920>>)
-3926359478096932525
```

보면 UIButton: 하고 나와 있는 값들은 모두 동일하다. 저 값들을 가지고 `==`연산을 할 때 하는 것이 아닌가 싶다.    
또 button의 경우 `UIButton: 0x15ff0a8b0` 과 해시값인 `5904574640`이 같은 값임을 알 수 있다. 16진수와 10진수일 뿐이다. 그런데 아래에 경우는 해시값과 달랐다. 그래서 혹시 옵셔널 때문인가? 라는 생각을 해서 옵셔널 바인딩 후에 비교를 해보았다.

```swift
    private func matchJuiceMenu(with button: UIButton) throws -> JuiceMenu {
        print(button)
        print(button.hashValue)
        if let strawBanana = strawberryBananaJuiceOrderButton{
            print(strawBanana)
            print(strawBanana.hashValue)
        }
}
```

```
<UIButton: 0x14bf25920; frame = (0 0; 277.667 66); opaque = NO; autoresize = RM+BM; layer = <CALayer: 0x600003996a60>>
5569141024
<UIButton: 0x14bf25920; frame = (0 0; 277.667 66); opaque = NO; autoresize = RM+BM; layer = <CALayer: 0x600003996a60>>
5569141024
```
옵셔널 바인딩을 하고나서 비교해보니 두가지 모두 해시값이 동일 했다.   
<UIButton: 하고 나오는 값들로 `==`연산을 비교하는 것으로 보이는데 나머지는 속성값으로 보이고 이 버튼들을 구별하는 키 값은 16진수로 표현된 해시값이 아닐까 생각이 든다.    

그런데 해시값이 같다고 같은 인스턴스인 것은 아니므로 100프로 보장할 수가 없다.   
물론 타입이 같고 해시값이 같고 속성값이 같을 확률은 너무나도 적지만 그 작은 확률이 중요한 상황에 일어날 수 있으므로 안전한 `===`연산자를 사용하기로 했다.
근데 같은 타입 안에서는 해시값을 겹치지 않게 하지 않았을까 하는 생각도 든다.. 이것은 더 알아봐야겠다.

