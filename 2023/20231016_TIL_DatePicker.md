# DatePicker


DatePicker는 날짜를 선택할 수 있는 SwiftUI에서 제공하는 UI 이다.   

- iOS 13.0+


```swift
init(selection: Binding<Date>, in: ClosedRange<Date>, displayedComponents: DatePicker<Label>.Components, label: () -> Label)
```

```swift
DatePicker(
    "DatePicker",
    selection: self.$tempDate,
    in: dateRange,
    displayedComponents: [.hourAndMinute, .date]
)
.datePickerStyle(.graphical)
.accentColor(.green_500)
```

## 파라미터

### LocalizedStringKey   
DatePicker의 이름? 제목? 이라고 보면 된다.   
스타일에 따라 UI에 표시된다.

### selection
선택된 날짜를 바이딩하는 변수 지정

### in
선택할 수 있는 날짜의 범위를 지정할수 있다.   
범위에 속하지 않는 날짜는 월 년도는 넘어가지 않도록 하여 보여주지 않으며, 일 단위는 선택되지 않도록 비활성화 된다.   

### displayedComponents
시간을 어느정도 까지 설정할 수 있을지 정할 수 있다.   
이름과 같이 당연히 date는 날짜, hourAndMinute는 시간을 설정 할 수 있다.   
배열로 받기 때문에 다 같이 받을 수 있다.   

### datePickerStyle
스타일을 정할 수 있다.   
-  compact: 기본 스타일  
- automatic: 맥, 아이폰 플랫폼에 따라 자동으로 달라지는 옵션, 아이폰은 compact  
- graphical: 달력 형식   
- wheel: 휠 방식으로 위아래로 굴린다.   

### accentColor
accentColor를 정해주면 포인트 색을 지정해줄 수 있다.  



참고   
[iOS Developer Document](https://developer.apple.com/documentation/swiftui/datepicker)
