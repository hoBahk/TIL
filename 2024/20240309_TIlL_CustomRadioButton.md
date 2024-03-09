# Custom RadioButton

RadioButton을 만들어보았다. 

```swift

struct RadioButton: View {
    @Binding private var isSelected: Bool
    private let label: String
    
    init(isSelected: Binding<Bool>, label: String = "") {
        self._isSelected = isSelected
        self.label = label
    }
    
    var body: some View {
        HStack(alignment: .top, spacing: 0.0) {
            labelView
            
            Spacer()
            
            circleView
        }
        .contentShape(Rectangle())
        .onTapGesture { isSelected = true }
    }
}

private extension RadioButton {
    @ViewBuilder var labelView: some View {
        if !label.isEmpty {
            Text(label)
                .font(.system(size: 12))
                .foregroundColor(.black)
        }
    }
    
    @ViewBuilder var circleView: some View {
        Circle()
            .fill(innerCircleColor)
            .padding(4)
            .overlay(
                Circle()
                    .stroke(outlineColor, lineWidth: 2)
            )
            .frame(width: 20, height: 20)
    }
}


private extension RadioButton {
    var innerCircleColor: Color {
        return isSelected ? Color.black : Color.clear
    }
    
    var outlineColor: Color {
        return isSelected ? Color.black : Color.gray
    }
}

extension RadioButton {
    init<V: Hashable>(tag: V, selection: Binding<V?>, label: String = "") {
        self._isSelected = Binding(
            get: { selection.wrappedValue == tag },
            set: { _ in selection.wrappedValue = tag }
        )
        self.label = label
    }
}


```