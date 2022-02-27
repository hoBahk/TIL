## 프로젝트 하면서 배운 내용

### UIActivityViewController

공유를 할 때 많이 사용하는 액티비티뷰를 구현하는 방법

```swift
let activityViewController = UIActivityViewController(
            activityItems: data, // 필요한 데이터
            applicationActivities: nil
        )
        if let popoverController = activityViewController.popoverPresentationController { // 아이패드를 사용할 때는 popover를 사용해서 activityView를 보여줄 수 있다.
            popoverController.sourceView = view.view // popover를 띄울 뷰 지정
            popoverController.sourceRect = CGRect( // 위치 지정
                x: view.view.bounds.midX,
                y: view.view.bounds.midY,
                width: 0,
                height: 0
            )
            popoverController.permittedArrowDirections = []
        }
        self.present(activityViewController, animated: true, completion: nil)
```


### tableView 메서드 trailingSwipeActionsConfigurationForRowAt

테이블뷰의 셀을 오른쪽에서 왼쪽으로 스와이프 했을 때의 액션을 지정하는 테이블뷰 델리게이트 메서드

```swift
override func tableView(
        _ tableView: UITableView,
        trailingSwipeActionsConfigurationForRowAt indexPath: IndexPath
    ) -> UISwipeActionsConfiguration? {
        let deleteAction = UIContextualAction(style: .destructive, title: "delete") { _, _, completeHandeler in
            self.deleteCell(indexPath: indexPath)
            completeHandeler(true)
        }
        let shareAction = UIContextualAction(style: .normal, title: "share") { _, _, completeHandeler in
            guard let splitVC = self.splitViewController as? SplitViewController else {
                return
            }
            self.showActivityViewController(
                view: splitVC,
                data: MemoDataManager.shared.memoList[indexPath.row].body ?? ""
            )
            completeHandeler(true)
        }
        shareAction.backgroundColor = .systemBlue
        return UISwipeActionsConfiguration(actions: [deleteAction, shareAction])
    }
```