# DispatchWorkItem

## 문제 상황
툴팁을 구현하였다.   
해당 툴팁은 발생 후 3초 뒤에 없어지고 툴팁을 누르거나 아이콘을 누르면 3초가 되지 않아도 없어져야한다.   

그래서 `asyncAfter`를 통해 3초 뒤에 없어지도록 했는데 도중에 툴팁을 눌러 없어지게 해도 3초 뒤에 없어지는 코드가 실행된다.   
그래서 툴팁을 켰다 껐다 하면 부자연스러운 상황이 발생하게 된다.   

## 문제 해결
이 상황을 해결하기 위해 `asyncAfter`를 통해 실행할 코드를 도중에 툴팁을 눌러 없어지도록 할 때 같이 task를 cancel 해야 했다.  

그래서 DispatchWorkItem를 사용하게 되었다.
DispatchQueue를 통해 실행할 코드(task)를 캡슐화 하는데 사용한다.   
DispatchWorkItem를 사용하면 wait, cancel과 같은 메서드를 사용할 수 있다.

아래와 같이 DispatchWorkItem의 인스턴스를 메서드가 아닌 뷰모델에서 가지도록 하여 툴팁이나 아이콘을 눌렀을 때 DispatchWorkItem를 cancel 하도록 하여 3초 뒤에 툴팁을 없어지도록 하는 코드를 실행시키지 않도록 하였다.

```swift
@Published var tooltipTimer: DispatchWorkItem?

func toggleRecommendedKcalTooltip() {
    self.isShowRecommendedKcalTooltip.toggle()
    
    if self.isShowRecommendedKcalTooltip {
        let deadline: DispatchTime = .now() + 3
         let dispatchWorkItem = DispatchWorkItem {
             self.isShowRecommendedKcalTooltip = false
         }
         DispatchQueue.main.asyncAfter(deadline: deadline, execute: dispatchWorkItem)
         tooltipTimer = dispatchWorkItem
    } else {
        tooltipTimer?.cancel()
    }
}
```