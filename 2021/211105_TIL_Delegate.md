TIL(Today I Learned) - 호박
# Delegate 패턴

오늘은 Delegate패턴에 대해 보겠습니다.

Delegate를 알기 위해서는 Protocol에 대해 알아야하는데요.

## Protocol
Protocol은 간단하게 말하면 ***프로토콜은 서로간의 지켜야할 규약***입니다.
프로토콜은 특정 기능 수행에 필수적인 요구를 정의한 청사진입니다.     
프로토콜은 다음에 자세히 다루겠습니다.   


## Delegate

### Delegate 설명
- Delegate는 클래스 또는 구조물이 책임의 일부를 다른 유형의 인스턴스로 양도(또는 위임)할 수 있는 디자인 패턴입니다.   
- Delegate는 프로토콜을 정의하여 구현됩니다.
- Delegate는 특정 액션에 응답하거나 해당 소스의 기본 유형을 알 필요 없이 외부 소스에서 데이터를 검색하는데 사용할 수 있습니다.

### Delegate 예제

먼저 두개의 화면이 있고 첫번째 화면에서 버튼을 클릭하면 두번째 화면으로 이동합니다.   
두번째 화면에서 TextField에 입력을 하고 버튼을 누르면 두번째 화면이 사라지면서 첫번째 화면의 레이블에 입력한 값이 출력되도록 합니다.
![](/Users/bagbyeongho/Desktop/스크린샷 2021-11-05 오전 2.21.07.png)

먼저 프로토콜을 정의해줍니다.   
프로토콜에 구현할 함수를 선언해 줍니다. 프로토콜에서 구현은 따로 해주지 않습니다.   
라벨에 입력된 값을 보여주는 함수입니다.
```swift
protocol DelegateProtocol {
    func showLabel(text: String)
}
```

그리고 첫번째 화면에 대한 ViewController입니다.   
첫번째 화면은 데이터를 받는 화면, receiver, 일을 하는 클래스 입니다.    
두번째 화면에서 할 일을 위임 받았습니다.

```swift
//1. 정의해준 DelegateProtocol을 채택합니다.
class ViewController: UIViewController, DelegateProtocol {
    @IBOutlet weak var defaultLabel: UILabel!
    
    override func viewDidLoad() {
        super.viewDidLoad()
    }
    
    //2. 프로토콜에 정의해준 함수를 구현해 줍니다. showLabel(text: String)
    func showLabel(text: String) {
        defaultLabel.text = text
    }
    
    override func prepare(for segue: UIStoryboardSegue, sender: Any?) {
        guard let secondVC = segue.destination as? SecondViewController else {
            return
        }
        //3. 두번 째 화면의 위임받은자, 대리자가 자신이라고 말해줍니다.
        secondVC.delegate = self
    }
 
    @IBAction func didTabButton(_ sender: Any) {
        performSegue(withIdentifier: "GoToSecondView", sender: nil)
    }
    
}
```

1. 정의해준 DelegateProtocol을 채택합니다.
2. 프로토콜에 정의해준 함수를 구현해 줍니다. showLabel(text: String)
3. 두번 째 화면의 위임자, 대리자가 자신이라고 말해줍니다.


다음 두번째 화면을 보겠습니다.
두번째 화면은 데이터를 전달하는 화면, 일을 시키는 클래스입니다.

```swift
class SecondViewController: UIViewController {
    //1.  타입이 protocol인 프로퍼티를 생성해줍니다.   
    var delegate: DelegateProtocol?
    
    @IBOutlet weak var textField: UITextField!
    
    override func viewDidLoad() {
        super.viewDidLoad()

    }
    
    @IBAction func exitDidTap(_ sender: Any) {
        guard let text = textField.text else {
            return
        }
        //2. 프로토콜에 선언된 메서드를 호출합니다.
        delegate?.showLabel(text: text)
        dismiss(animated: true, completion: nil)
    }
}
```

1. 타입이 protocol인 프로퍼티를 생성해줍니다. 프로토콜을 통해 두 클래스를 연결시키는 것입니다.
2. 프로토콜에 선언된 메서드를 호출합니다. 메서드를 호출하여 일을 시키게 됩니다.

이렇게 해주면 두번째 화면에서 입력한 값이 첫번째 화면에 출력되게 됩니다.

### Delegate 장단점
***장점***  
1. 로직의 흐름을 따라가기 쉽다.
2. 프로토콜에 필요한 메서드가 명확하게 명시된다.
3. 커뮤니케이션 과정을 유지하고 모니터링하는 제 3의 객체(ex: NotificationCenter 같은 외부 객체)가 필요없다.

***단점***   
1. 많은 줄의 코드가 필요하다.
2. 1:N으로 이벤트를 알려주는 것이 복잡하고 비효율적이다.

제가 생각했을 때는 1:1의 상황에서는 Delegate가 더 이점이 있고, 1:N 이나 N:M의 이벤트를 알려야할 때는 NotificationCenter를 사용하는 것이 좋아보입니다.


