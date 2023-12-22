# OrientationOrientation

## 설명
기존 UIKit에서 쓰던 CMMotionmanager()를 사용하여 X,Y,Z를 구해 디바이스의 방향을 알아낸다. 
onRecive로 초마다 업데이트 하도록 했다.   

## 예제코드

```swift

enum Orientation: Double {
    case up
    case down
    case left
    case right
}

let timer = Timer.publish(every: 1, on: .main, in: .common).autoconnect()

struct DeviceOrientation: ViewModifier {
    let action: (UIDeviceOrientation) -> Void
    @State var rotateAngle: Double = 0.0
    
    var motionManager = CMMotionManager()
    @State private var gravityX: Double = 0
    @State private var gravityY: Double = 0
    @State private var gravityZ: Double = 0
    
    @State private var previousOrientation: Orientation = .up

    func body(content: Content) -> some View {
        content
            .onAppear()
            .rotationEffect(.degrees(self.rotateAngle))
            .animation(.default, value: self.previousOrientation)
            .onReceive(timer) { input in
                if motionManager.isDeviceMotionAvailable {
                    motionManager.deviceMotionUpdateInterval = 0.3
                    
                    motionManager.startDeviceMotionUpdates(to: OperationQueue.main) { data,error in
                        gravityX = data?.gravity.x ?? 0
                        gravityY = data?.gravity.y ?? 0
                        gravityZ = data?.gravity.z ?? 0
                        if gravityX < -0.75 {
                            self.rotateAngle = 90
                            self.previousOrientation = .left
                        } else if gravityX > 0.75 {
                            self.rotateAngle = self.previousOrientation == .down ? 270 : -90
                            self.previousOrientation = .right
                        } else if gravityY < -0.75 {
                            self.rotateAngle = 0
                            self.previousOrientation = .up
                        } else if gravityY > 0.75 {
                            self.rotateAngle = self.previousOrientation == .right ? -180 : 180
                            self.previousOrientation = .down
                        }
                    }
                }
            }
    }
}

extension View {
    func onOrientation2(perform action: @escaping (UIDeviceOrientation) -> Void) -> some View {
        self.modifier(DeviceOrientation(action: action))
    }
}

```