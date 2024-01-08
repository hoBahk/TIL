# UIViewRepresentable_TextField

TextField를 SwiftUI에서 사용하려면 iOS15나 되어야 포커스가 됐는지 확인할 수 있는데 포커스 여부는 보통 필요한 것 같다.   
그래서 UIViewRepresentable를 통해서 TextField를 만들어야한다.    

## 예제

```swift

struct CustomTextField: UIViewRepresentable {
    @Binding var text: String
    @Binding var isFocused: Bool
    var font:  UIFont.SFProFontComponent
    var textColor: Color

    func makeUIView(context: UIViewRepresentableContext<CustomTextField>) -> UITextField {
        let tf = UITextField(frame: .zero)
        tf.autocorrectionType = .no
        tf.autocapitalizationType = .none
        tf.isUserInteractionEnabled = true
        tf.delegate = context.coordinator
        tf.font = self.font.font
        tf.textColor = self.textColor.uiColor()
        tf.textAlignment = .center

        return tf
    }

    func makeCoordinator() -> CustomTextField.Coordinator {
        return Coordinator(self, text: $text, isFocused: $isFocused)
    }

    func updateUIView(_ uiView: UITextField, context: Context) {
        uiView.text = self.text
    }

    class Coordinator: NSObject, UITextFieldDelegate {
        var parent: CustomTextField
        @Binding var text: String
        @Binding var isFocused: Bool

        init(_ parent: CustomTextField, text: Binding<String>, isFocused: Binding<Bool>) {
            self.parent = parent
            _text = text
            _isFocused = isFocused
        }

        func textFieldDidChangeSelection(_ textField: UITextField) {
            text = textField.text ?? ""
        }

        func textFieldDidBeginEditing(_ textField: UITextField) {
            DispatchQueue.main.async {
               self.isFocused = true
            }
        }

        func textFieldDidEndEditing(_ textField: UITextField) {
            DispatchQueue.main.async {
                self.isFocused = false
            }
        }
    }
}
```

다음 프로젝튼 	iOS15를 사용할 수 있겠지..