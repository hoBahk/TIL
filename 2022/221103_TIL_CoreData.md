# CoreData

## CRUD

1. NSManagedObjectModel 만들기
-  다른 프레임워크에서 사용하기 위해서 프레임워크의 번들을 지정하여 `NSManagedObjectModel`를 만들어 준다.


```swift

final class PersistentManager {
    static let shared = PersistentManager()
    var cancellable: Set<AnyCancellable> = []
    private init() {}
    
    let frameworkBundleID   = "com.hobahk.coredata"
    let modelName           = "Model"
    let PhotoEntity     = "PhotoModel"
    
    var managedObjectModel: NSManagedObjectModel {
        guard let frameworkBundle = Bundle(identifier: self.frameworkBundleID),
              let modelURL = frameworkBundle.url(forResource: self.modelName, withExtension: "momd"),
              let managedObjectModel =  NSManagedObjectModel(contentsOf: modelURL) else {
            return NSManagedObjectModel()
        }
        return managedObjectModel
    }
```

2. Persistent container 만들기

```swift
    
    lazy var persistentContainer: NSPersistentContainer = {
        let container = NSPersistentContainer(name: self.modelName, managedObjectModel: self.managedObjectModel)
        container.loadPersistentStores(completionHandler: { (_, error) in
            if let error = error as NSError? {
                fatalError("Unresolved error \(error), \(error.userInfo)")
            }
        })
        return container
    }()
    
    func saveContext () {
        let context = persistentContainer.viewContext
        if context.hasChanges {
            do {
                try context.save()
            } catch {
                let nserror = error as NSError
                fatalError("Unresolved error \(nserror), \(nserror.userInfo)")
            }
        }
    }
}

```

3. CRUD
- 여러 엔티티를 가져오는데에 편하게 제네릭으로 만들었다.

```swift
 
extension PersistentManager {
    private func insert(entityName: String = "PhotoModel", items: [String: Any]) {
        let context = persistentContainer.viewContext
        let managedObject = NSEntityDescription.insertNewObject(forEntityName: entityName, into: context)
        update(managedObject, items: items)
    }
    
    private func fetch<T>(
        entityName: String = String(describing: T.self),
        predicate: NSPredicate? = nil,
        sortDescriptors: [NSSortDescriptor]? = [NSSortDescriptor(key: "id", ascending: false)]
    ) -> [T]? {
        let context = persistentContainer.viewContext
        let request = NSFetchRequest<NSManagedObject>(entityName: entityName)
        request.predicate = predicate
        request.returnsObjectsAsFaults = false
        request.sortDescriptors = sortDescriptors
        guard let newData = try? context.fetch(request) as? [T] else {
            return nil
        }
        return newData
    }
    
    private func update(_ managedObject: NSManagedObject, items: [String: Any]) {
        let keys = managedObject.entity.attributesByName.keys
        for key in keys {
            if let value = items[key] {
                managedObject.setValue(value, forKey: key)
            }
        }
        saveContext()
    }
    
    private func delete<T>(_ item: T) {
        let context = persistentContainer.viewContext
        guard let managedObject = item as? NSManagedObject else {
            return
        }
        context.delete(managedObject)
        saveContext()
    }
}

```