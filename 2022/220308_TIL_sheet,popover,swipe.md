# 프로젝트 중 SwiftUI

## Sheet

```swift
 Text("Sheet")
        .onTapGesture { // 탭 한번 제스처
            self.isPopoverPresentedForUpdateTask = true // self.isPopoverPresentedForUpdateTask를 @State하여 true, false를 조정
        }
        .sheet(isPresented: $isPopoverPresentedForUpdateTask) {
            TaskDetailView(viewModel: viewModel, task: task) // sheet으로 보여줄 뷰
        }
```


## popover

```swift
 Text("popover")
     .onLongPressGesture { // 길게 누르는 제스처
        self.isPopoverPresentedForUpdateTaskState = true // self.isPopoverPresentedForUpdateTaskState를 @State하여 true, false를 조정
    }
    .popover(isPresented: $isPopoverPresentedForUpdateTaskState) {
        VStack {
            Button("Move to \(firstMoveStatus.rawValue)") {
                viewModel.updateState(firstMoveStatus, to: task)
                self.isPopoverPresentedForUpdateTaskState = false
            }
            .padding()
            Button("Move to \(secondMoveStatus.rawValue)") {
                viewModel.updateState(secondMoveStatus, to: task)
                self.isPopoverPresentedForUpdateTaskState = false
            }
            .padding()
            .onAppear {
                setButtonTitle()
            }
        } // 이렇게 바로 뷰를 그릴 수 있지만 따로 그리고 호출하는 것이 좋음
    }
```

## Swipe

```swift
	// wipeAction
    .swipeActions(edge: .trailing, allowsFullSwipe: true) { // edge: leading에 action을 만들것인지 trailing에 만들것인지 결정, allowsFullSwipe: 따로 클릭하지 않고 스와이프를 끝까지 했을 때 실행되게 할 것인지 여부
        Button {
            viewModel.deleteTask(task.id)
        } label: {
            Label("Delete", systemImage: "trash")
        }
        .tint(.red)
        // 스와이프하면 보여줄 버튼 구현
    }

```
