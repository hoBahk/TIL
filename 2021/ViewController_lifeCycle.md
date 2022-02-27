## ViewController Life Cycle

오늘은 ViewController의 생명주기에 대해 알아보겠습니다.

ViewController의 생명주기는 아래와 같습니다.<br>

![](/Users/bagbyeongho/Desktop/순서도(Flow Chart)/ViewControllerLifeCycle.jpg)


오늘은 ViewController가 두 개 있고 뷰가 바뀔 때 어떻게 진행이 되는지 테스트 해보겠습니다.    
해당 실험은 NavigationController입니다.
<br><br>
먼저 각 단계 별 설명을 하겠습니다.
***init()***     
storyborad를 통해 viewController를 만들 경우 객체가 생성될 때 초기화 하는 작업을 하는 메소드입니다.

***loadView()***   
화면에 띄워질 View에 대한 데이터를 불러오는 메소드입니다.

***viewDidLoad()***  
뷰의 컨트롤러가 메모리에 로드된 후에 호출됩니다.   
viewDidLoad메소드는 뷰의 로딩이 완료 되었을 때 시스템에 의해 자동으로 호출되기 떄문에 리소스를 초기화 하거나 초기화면을 구성하는 용도로 주로 사용합니다.

***viewWillAppear()***     
viewWillAppear로 보면 뷰가 나타날것이다.   
즉, 뷰가 나타나기 직전에 호출되는 메소드입니다.

***viewDidAppear()***     
이것은 뷰가 나타났다.   
즉, 뷰가 나타난 직후 호출되는 메소드입니다.

***viewWillDisappear()***    
뷰가 사라지기 직전에 호출되는 메소드입니다.
 
***viewDidDisappear()***     
 뷰가 사라지고난 직후 호출되는 메소드입니다.
 
 ***deinit()***
 뷰가 메모리에서 해제될 때 호출되는 메소드입니다.  

<br>
실험할 코드는 아래와 같습니다.   

***1번 ViewController***

```swift
class ViewController: UIViewController {

    override func loadView(){
        super.loadView()
        print("1번: loadView")
    }
    override func viewDidLoad() {
        super.viewDidLoad()
        print("1번: viewDidLoad")
        // Do any additional setup after loading the view.
    }
    
    override func viewWillAppear(_ animated: Bool) {
        print("1번: viewWillAppear")
    }
    
    override func viewDidAppear(_ animated: Bool) {
        print("1번: viewDidAppear")
    }
    
    override func viewWillDisappear(_ animated: Bool) {
        print("1번: viewWillDisappear")
    }
    
    override func viewDidDisappear(_ animated: Bool) {
        print("1번: viewDidDisappear")
    }
    
    deinit {
        print("1번: deinit")
    }
}

```
<br><br>
***2번 ViewController***

```swift
class SecondViewController: UIViewController {

    override func loadView(){
        super.loadView()
        print("2번: loadView")
    }
    override func viewDidLoad() {
        super.viewDidLoad()
        print("2번: viewDidLoad")

        // Do any additional setup after loading the view.
    }
    
    override func viewWillAppear(_ animated: Bool) {
        print("2번: viewWillAppear")
    }
    
    override func viewDidAppear(_ animated: Bool) {
        print("2번: viewDidAppear")
    }
    
    override func viewWillDisappear(_ animated: Bool) {
        print("2번: viewWillDisappear")
    }
    
    override func viewDidDisappear(_ animated: Bool) {
        print("2번: viewDidDisappear")
    }
    
    deinit {
        print("2번: deinit")
    }
}

```


#### 첫 번째 화면

![](/Users/bagbyeongho/Desktop/viewConVLC1.png)

첫 번째 화면이 뜰 때 loadView -> viewDidLoad -> viewWillAppear -> ViewDidAppear로 호출되는 것을 볼 수 있습니다. 

#### 첫 번째 화면 -> 두 번째 화면

![](/Users/bagbyeongho/Desktop/viewConVLC2.png)

먼저 두 번째 화면이 loadView, ViewDidLoad 됩니다.      
그리고 첫 번째 뷰에서 "나 사라질거야"를 말합니다. 그러면 두 번째 뷰에서 "나는 나타날거야"라고 말합니다.     
그리고 나서 첫 번째 뷰에서는 "나 사라졌어"를 말하고 사라집니다. 그리고 두 번째 뷰에서 "나 나타났어" 하고 나타나게 됩니다.   


#### 두 번째 화면 -> 첫 번째 화면

![](/Users/bagbyeongho/Desktop/viewConVLC3.png)

두 번째 화면에서 첫번째 화면으로 다시 돌아올 때는 반대로,    
두 번째 화면에서 "나 사라질거야"를 말하고, 첫 번째 화면에서는 "나 나타날거야" 라고 말합니다.   
그 뒤에 두 번째 화면에서 "나 사라졌어"하고 사라지게 됩니다. 그러면 첫 번째 화면에서 "나 나타났어"를 말하고 나타납니다.   
그리고 나서는 두 번째 화면의 메모리가 해제되어 deini을 호출하게 됩니다.


이 실험을 통해 ViewController의 생명주기에 대해 개념을 잡을 수 있었습니다.   
다른 분들에게도 도움이 됐으면 좋겠습니다.
