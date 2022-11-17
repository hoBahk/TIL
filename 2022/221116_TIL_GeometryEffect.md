# GeometryEffect 

view가 path에 따라서 이동하도록 한다.

```swift

struct ActivityIconRingCap: View {
    var progress: Double
    let ringRadius: Double
    let ringLineWith: Double
    
    var body: some View {
        GeometryReader { proxy in
            let widthAndHeight: Double = self.ringRadius * 2
            let rect: CGRect = .init(x: .zero, y: .zero, width: widthAndHeight, height: widthAndHeight)
            
            Image("cal_intake")
                .resizable()
                .frame(width: self.ringLineWith, height: self.ringLineWith)
                .modifier(
                    FollowEffect(progess: self.progress, path: CirclePath().path(in: rect))
            )
        }
    }
}


struct FollowEffect: GeometryEffect {
    var pct: CGFloat = 0
    let path: Path
    
    var animatableData: CGFloat {
        get {
            return pct
        }
        set {
            pct = newValue
        }
    }
    
    func effectValue(size: CGSize) -> ProjectionTransform {
        let pt = percentPoint(pct)
        
        return ProjectionTransform(CGAffineTransform(translationX: pt.x, y: pt.y))
        
    }
    
    func percentPoint(_ percent: CGFloat) -> CGPoint {
        let diff: CGFloat = 0.001
        let comp: CGFloat = 1 - diff
        
        let pct = percent > 1 ? percent.decimal : percent
        
        let f = pct > comp ? comp : pct
        let t = pct > comp ? 1 : pct + diff
        let tp = path.trimmedPath(from: f, to: t)
        
        return CGPoint(x: tp.boundingRect.midX, y: tp.boundingRect.midY)
    }
}
```