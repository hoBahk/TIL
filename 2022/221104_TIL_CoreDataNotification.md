# CoreData

## CoreDataNotification
- 코어데이터에도 노티피케이션을 설정할 수 있다.

코어데이터에 저장이 변경이 이루어 졌을 때, 이벤트가 발생되며 insert, update, delete등 어떤 이벤트인지 분별할 수 있으며 그에 따라 로직을 변경할 수 있다.

노티피케이션을 Combine을 활용하여 사용할 수 있다.

1. Publisher 생성

```swift
var didSaveCoreDataPublisher: AnyPublisher<Notification, Never> {
    let didSaveNotification = NSManagedObjectContext.didSaveObjectsNotification
    let notificationPublisher = NotificationCenter.default.publisher(for: didSaveNotification, object: nil).eraseToAnyPublisher()
    return notificationPublisher
}
```

2. Subcriber 등록

```swift
func addDidSaveCoreDataSubscribers(key: NSManagedObjectContext.NotificationKey) {
    didSaveCoreDataPublisher
        .sink { [weak self] notification in
            switch key {
            case .insertedObjects:
                self?.didInsert(notification)
            case .deletedObjects:
                self?.didDelete(notification)
            default:
                return
            }
        }
        .store(in: &self.cancellable)
}
```

2. 호출되는 메서드 생성

```swift
private func didInsert(_ notification: Notification) {
    guard let inserted: Set<NSManagedObject> = notification.userInfo?.value(for: .insertedObjects),
          let PhotoModel: PhotoModel = inserted.first as? PhotoModel,
          let image = UIImage(data: PhotoModel.image ?? Data()) else {
        return
    }
    // code...
}

private func didDelete(_ notification: Notification) {
    guard let inserted: Set<NSManagedObject> = notification.userInfo?.value(for: .deletedObjects),
          let PhotoModel: PhotoModel = inserted.first as? PhotoModel,
          let image = UIImage(data: PhotoModel.image ?? Data()) else {
        return
    }
    // code...
}
```