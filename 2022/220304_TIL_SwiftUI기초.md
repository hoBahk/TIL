# SwiftUI 기초

## List (UIKIt: Table View)


```swift
List {
    Section(header: Text("Doing").font(.title2)) { // 섹션
        ForEach(viewModel.doingTaskList) { task in // for문을 통해서 반복문을 돌려주면 그만 큼 셀이 생김.
            TaskListCellView(task: task)
        }
    }
}

```


## Button

```swift
Button(action: { // 버튼을 누를 때 필요한 로직 구현
    isSheetPresented = true
}, label: { // View관련 보여주는 부분
    Image(systemName: "plus")
    .sheet(isPresented: $isSheetPresented) { // 모달을 보여줄 때 사용할 수 있다.
        TaskDetailView()
    }
})

```

## TextView
```swift
TextField("title", text: $text) // 텍스트필드 설정
    .frame(height: 30, alignment: .leading) // 프레임 설정
```


## DatePicker

![https://s3.ap-northeast-2.amazonaws.com/media.yagom-academy.kr/resources/usr/6152fa89ccd9ef11a51aee7d/20220304/622213b4b373ef1054ca631f.png](https://s3.ap-northeast-2.amazonaws.com/media.yagom-academy.kr/resources/usr/6152fa89ccd9ef11a51aee7d/20220304/622213b4b373ef1054ca631f.png)

```swift
DatePicker("마감기한", selection: $deadline, displayedComponents: .date)
    .datePickerStyle(WheelDatePickerStyle()).labelsHidden()
```


## Gesture


```swift
// 길게 누르는 제스처
TaskListCellView(task: task)
    .onLongPressGesture(minimumDuration: 1, maximumDistance: 50) {
        print("onLongPressGesture")
    }
    
// 한 번 터치하는 제스처
TaskListCellView(task: task)
    .onTapGesture() {
        print("onTapGesture")
    }
```