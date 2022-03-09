# SwiftUI

## 스와이프 (onDelete)

`LIst`에서 `onDelete`를 사용하여 스와이프를 구현할 수 있다.
클로저를 열어서 사용하는데 로우를 알아내는데에 IndexSet이라는 타입을 사용한다.

IndexSet  
A collection of unique integer values that represent the indexes of elements in another collection.

다른 컬렉션에 있는 요소의 인덱스를 나타내는 고유의 정수 값의 컬렉션이다.

```swift

extension IndexSet {
    var index: Int {
        return self[self.startIndex]
    }
}

List {
    ForEach(taskList) { task in
        TaskListCellView(task: task)
    }
    .onDelete { indexSet in
        viewModel.deleteTask(taskList[indexSet.index])
    }
}
```

## onAppear & onDisappear
### onAppear
UIKit의 ViewDidDisplay() 와 동등한 기능을 제공한다.
View가 나타날 때 실행될 action을 지정해줄 수 있다.

### onDisappear
onAppear와 반대로 View가 사라질 때 실행될 action을 지정해줄 수 있다.


```swift
var body: some View {
    VStack {
        headerView
        titleTextField
        deadLineView
        descriptionTextEditor
    }
    .padding(.horizontal)
    .onAppear {
        print("DetailView가 나타났습니다.")
        }
    .onDisappear {
    	print("DetailView가 사라졌습니다.")
    }
}
```