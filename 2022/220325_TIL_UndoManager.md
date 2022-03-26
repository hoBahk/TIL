# UndoManager(Undo/Redo)


## UndoManager
스위프트에서 UndoManager를 지원한다.    
UndoManager에 Undo를 등록하면 스택에 저장해 놓고 Undo를 등록할 때 Redo에 해당되는(취소한 것을 되돌려 놓는) 작업을 Redo스택에 저장한다.

undo manager는 retain cycles을 방지하기 위해서 target을 unowned reference로 유지한다.




## 사용법 
- undo manager는 retain cycles을 방지하기 위해서 target을 unowned reference로 유지한다.
- registerUndo 메서드를 통해서 Redo스택에 Undo시 실행할 작업을 저장합니다.

```swift 
let undoManager = UndoManager()

undoManager.registerUndo(withTarget: self) { _ in
	// 취소 작업
    action()
}

Button {
    taskListViewModel.historyManager.undoManager.undo()
} label: {
    Image(systemName: "arrow.uturn.backward")
}

```


## 주의할 점
UndoManager를 사용하는데 변경을 할 때 한번에 하나만 가능하다고 한다.
프로젝트를 할 때 Task를 업데이트 할 때 제목 본문 일시 3가지를 한번에 업데이트 하려고 하니 되지 않았는데, 한번에 하나의 변경사항만 가능하다는 글을 보고 이전 TASK를 새로운 TASK로 변경하도록 Task타입 하나만 변경하도록 하니 잘 작동하는 것을 확인할 수 있었다.
