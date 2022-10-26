# PHPickerViewController

- 앨범에서 사진을 가져올 때 사용한다.
- iOS14 이상 사용 가능


`info[.phAsset] as? PHAsset`을 사용하여 앨범에서 받아온 사진의 정보를 알아내려고 하였으나 `phAsset`은 이미 deprecated 되었다. iOS 11 - 14까지만 사용이 가능하다.

그래서  iOS14 부터 사용 가능한 PHPIckerViewController를 사용하게 되었다.   
SwiftUI로 구현하기 위해서 UIViewControllerRepresentable을 사용하여 구현 하였다.


Info.plist에서 `Privacy - Camera Usage Description` 추가해줘야한다.   
`UIImagePickerController`는 `Privacy - Photo Library Usage Description`를 추가 한다.


```swift
@available(iOS 14.0, *)
struct PHPicker: UIViewControllerRepresentable {
    @Binding var selectedImage: ImageModel
    
    @Environment(\.presentationMode) var presentationMode
    
    func makeUIViewController(context: Context) -> PHPickerViewController {
        var config = PHPickerConfiguration(photoLibrary: PHPhotoLibrary.shared())
        config.filter = .images
        config.selectionLimit = 1
        let controller = PHPickerViewController(configuration: config)
        controller.delegate = context.coordinator
        return controller
    }
    
    func updateUIViewController(_ uiViewController: PHPickerViewController, context: Context) {
        
    }
    
    func makeCoordinator() -> Coordinator {
        Coordinator(self)
    }
    
    // Use a Coordinator to act as your PHPickerViewControllerDelegate
    class Coordinator: PHPickerViewControllerDelegate {
        private let parent: PHPicker
        
        init(_ parent: PHPicker) {
            self.parent = parent
        }
        
        func picker(_ picker: PHPickerViewController, didFinishPicking results: [PHPickerResult]) {
            print(results)
            
            guard let imageResult = results.first else {
                self.parent.presentationMode.wrappedValue.dismiss()
                return
            }
            
            if let assetId = imageResult.assetIdentifier {
                let assetResults = PHAsset.fetchAssets(withLocalIdentifiers: [assetId], options: nil)
                DispatchQueue.main.async {
                    self.parent.selectedImage.date = assetResults.firstObject?.creationDate ?? Date()
                    print(assetResults.firstObject?.creationDate ?? Date())
                }
            }
            if imageResult.itemProvider.canLoadObject(ofClass: UIImage.self) {
                imageResult.itemProvider.loadObject(ofClass: UIImage.self) { (selectedImage, error) in
                    if let error = error {
                        print(error.localizedDescription)
                    } else {
                        DispatchQueue.main.async {
                            guard let selectedImage = selectedImage as? UIImage else {
                                return
                            }
                            self.parent.selectedImage.image = selectedImage
                            print(selectedImage)
                        }
                    }
                }
            }
            self.parent.presentationMode.wrappedValue.dismiss()
        }
    }
}

```