# DateComponents

```swift
extension Date {
    func components(_ components: Set<Calendar.Component>) -> DateComponents {
        let calendar = Calendar.current
        return calendar.dateComponents(components, from: self)
    }
}

func setEatDate() {
    let onlyDateComponents = self.onlyDate.components([.year, .month, .day])
    let onlyTimeComponents = self.onlyTime.components([.hour, .minute])
    let dateComponents = DateComponents(year: onlyDateComponents.year,
                                        month: onlyDateComponents.month,
                                        day: onlyDateComponents.day,
                                        hour: onlyTimeComponents.hour,
                                        minute: onlyTimeComponents.minute)
    let calendar = Calendar.current
    let date = calendar.date(from: dateComponents)
    self.eatDate = date ?? Date()
}
```

이런식으로 Date의 Components를 구해서 년, 월, 일, 시, 분, 초를 꺼내올 수 있다.    
그것들을 조합해서 새로운 시간을 만들수도 있다.  

나 같은 경우는 날짜와 시간을 따로 받아 온전한? 하나의 Date로 합쳐야 했다.  
그래서 components로 쪼개고 하나의 Date에 합쳐 넣었다.   

