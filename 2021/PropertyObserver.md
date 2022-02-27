# Property Observers

Property Observers는 프로퍼티의 값이 바뀔 때 호출되며 다양한 행동을 취할 수 있습니다.
새 값이 현재의 값과 같더라도 Property Observers는 호출됩니다.

Property Observers를 사용할 수 있는 곳은,

- 사용자가 정의한 Stored properties
- 상속 받은 Stored properties
- 상속 받은 Computed properties

상속된 경우에는 하위클래스에서 해당 속성을 재정의 하여 Property Observer를 추가하면 됩니다.     
Computed property는 setter에서 값의 변화를 알 수 있어서 필요가 없습니다.

## observers

>- willSet: 값이 저장되기 바로 직전에 호출됩니다.   
- didSet:  값이 저장되고 난 직후에 호출됩니다. 

어려운 개념은 아니어서 바로 예제를 보겠습니다.  
예제는 공식문서에서 가져왔습니다.

```swift
class StepCounter {
    var totalSteps: Int = 0 {
        willSet(newTotalSteps) {
            print("About to set totalSteps to \(newTotalSteps)")
        }
        didSet {
            if totalSteps > oldValue  {
                print("Added \(totalSteps - oldValue) steps")
            }
        }
    }
}
let stepCounter = StepCounter()
stepCounter.totalSteps = 200
// About to set totalSteps to 200
// Added 200 steps
stepCounter.totalSteps = 360
// About to set totalSteps to 360
// Added 160 steps
stepCounter.totalSteps = 896
// About to set totalSteps to 896
// Added 536 steps
```

위의 예제 코드를 보시면 `totalSteps`의 값이 변경되기 직전에 willSet의 구문이 호출이 되고 변경되고 나서는 didSet의 구문이 호출되는 것을 볼 수 있습니다.