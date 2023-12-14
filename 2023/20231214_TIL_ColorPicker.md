# ColorPicker


Color Picker를 통해서 컬러를 선택할 수 있다.    
생각보다 잘되어 있어서 놀랐다.   
역시 iOS 

```swift
struct ColorPicker: View {
    @Binding var selectedColor: Color
    
    var body: some View {
        ColorPicker("컬러 설정", selection: self.$selectedColor)
            .padding(10)
            .background(self.selectedColor)
            .opacity(0.8)
            .cornerRadius(8)
    }
}
```