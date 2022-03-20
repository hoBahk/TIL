# SwiftUI - error시 Alert 띄우시

SwiftUI로 에러 발생 시 얼럿을 띄우는 방법에 대해 공부해보았다.

먼저 ErrorModel을 만들어야 한다.
에러를 식별할 수 있는 id와 에러 메시지 두개를 프로퍼티로 놓는다.

```swift
struct ErrorModel: Identifiable {
    var id = UUID()
    var message: String
}
```

그리고 뷰 모델에 Published wrapper로 프로퍼티를 만들어준다.

```swift
class TaskListViewModel: ObservableObject {
    @Published var errorAlert: ErrorModel?
    
    // ...
}
```

그렇게 하고 나서 에러 발생시 catch문에서 위에 선언한 프로퍼티에 에러 메시지를 errorModel에 담아 할당해준다.

```swift
func createTask(_ task: Task) {
    do {
        try taskListManager.createRealmTask(task)
        historyManager.appendHistory(taskHandleType: .create(title: task.title))
    } catch {
        errorAlert = ErrorModel(message: error.localizedDescription)
        print(error.localizedDescription)
    }
}
```

그리고 뷰에서 뷰모델의 Published로 래핑한 프로퍼티가 변경되면 얼럿이 발생하도록 구현해준다.

```swift
private struct TaskDetailTrailingButton: View {
    @EnvironmentObject private var taskListViewModel: TaskListViewModel
    @ObservedObject var taskDetailViewModel: TaskDetailViewModel
    var body: some View {
        Button("Done".localized()) {
            let task = Task(
                title: taskDetailViewModel.title,
                description: taskDetailViewModel.description,
                deadline: taskDetailViewModel.deadline
            )
            taskListViewModel.createTask(task)
        }
        // 에러 발생시 얼럿으로 에러메시지를 사용자에게 보여준다.
        .alert(item: $taskListViewModel.errorAlert) { error in
            Alert(title: Text("Error".localized()), message: Text(error.message))
        }
}
```



