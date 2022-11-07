# combine - Timer

combine을 통해 Timer Publisher를 만들어 보자

`Timer.publisher`를 통해서 쉽게 만들 수 있다.

```swift
class PollingTimer {
    private var cancellable = Set<AnyCancellable>()
    private let timer = Timer.publish(every: 1.0, on: .main, in: .common)
                            .autoconnect()
    
    // Timer Subscribe 시작
    func startTimer() {
        self.timer
            .sink { output in
                print(output)
            }
            .store(in: &cancellable)
    }
    
     // Timer Subscribe 종료
    func cancelTimer() {
        self.timer.upstream.connect().cancel()
    }
}
```

파라미터   
- every: TimeInterval로 몇초마다 반복할지에 대한 파라미터  
- on: 타이머가 어떤 쓰레드의 RunLoop에서 실행될지에 대해 묻는 파라미터  (main, current)  
- in: 타이머가 실행되는 RunLoop Mode를 결정하는 파라미터 (common, default, tracking)  
- tolerance: 허용 오차범위 지정, 오차범위를 어느정도 허용함으로서 시스템의 자원 사용량을 일정부분 낮출 수 있다. 하지만 오차범위를 허용하지 않는다고 완전히 오차가 없진 않다. (옵셔널)

## 문제점
- 처음 실행 할 때 설정한 간격이 지난 후 트리거가 되었다. 그래서 `prepend` 옵션을 사용하여 즉시 트리거 되도록 설정하였다.
- `merge(with: )` 옵션을 사용해도 된다.

```swfit
pollingTimer.timer
    .receive(on: DispatchQueue.main)
    .prepend(Just(Date()))
    .scan(0) { counter, _ in counter + 1 }
    .sink { [weak self] count in
        print(count)
        count == 3 ? self?.pollingTimer.cancelTimer() : ()
    }
    .store(in: &pollingTimer.cancellable)
```

# RunLoop

## 의미

[Apple Document - RunLoop](https://developer.apple.com/library/archive/documentation/Cocoa/Conceptual/Multithreading/RunLoopManagement/RunLoopManagement.html)

RunLoop 객체는 소켓, 파일, 키보드 마우스 등의 입력 소스를 처리하는 이벤트 처리 루프로,
쓰레드가 일해야 할 때는 일하고, 일이 없으면 쉬도록 하는 목적으로 고안되었다
RunLoop 입장에서 Timer는 입력이 아닌 특수한 유형이지만, Timer의 이벤트 또한 처리한다



## Thread 와 RunLoop

- Thread는 각자 RunLoop를 가질 수 있다.
- Thread를 생성할 때 RunLoop가 자동으로 생성된다.
- 그렇다고 RunLoop가 자동으로 실행되지는 않는다.
- RunLoop 실행에대한 관리는 프로그래머의 몫이다. (Main RunLoop는 예외)



## RunLoop 작동원리

### 2가지의 Event Source를 수신한다.
#### Input Source
- 다른 Thread나 Application으로부터 온 비동기 이벤트를 전달한다.

#### Timer
- 예약된 시간 또는 반복 간격으로 발생하는 동기 이벤트를 전달한다.

### 작동원리
- RunLoop는 한 번의 실행 동안 내 쓰레드에 도착한 이벤트를 받고, 이에 대한 핸들러를수행하는 객체
- RunLoop가 자체적으로 계속 이벤트가 들어오나 안들어오나 실행을 반복 한다고 생각할 수 있겠지만 RunLoop는 내부적으로 반복 실행을 하지 않는다.
- 한 번 Event Source를 읽고 전달하는 실행이 끝나면 그대로 대기 하고 반복하지 않는다.
- 그래서 프로그래머가 반복문을 통해 RunLoop를 반복해서 실행시켜주어야 한다.


## Main RunLoop

Global Thread에서 Timer를 실행시키면 실행되지 않는다.   
왜냐하면 RunLoop를 실행시키지 않았기 떄문이다.    
하지만 MainRunLoop는 따로 실행시키지 않았는데도 Timer를 실행시키면 동작한다.   
이유는 Main Thread는 애플리케이션이 실행될때 프레임워크 차원에서 자동으로 RunLoop를 설정하고 실행하기 때문이다.  


## RunLoop 실행 방법

### run( )
- reciever를 영구 루프에 넣고, 이 기간동안 모든 부착된 Input Source의 데이터를 처리한다.
- run( ) 메서드를 실행시키기 전에 부착된 Input Source 이벤트만 처리한다.

### run(mode:before:)
- 루프를 한 번 실행하여 지정된 모드에서 지정된 날짜까지 input을 blocking 한다.


### run(until:)
- 지정된 날짜까지 루프를 실행하고, 그 기간 동안 루프는 부착된 모든 Input Source들의 데이터를 처리한다.
- RunLoop를 실행할 때 가장 많이 사용하는 메서드
- 지정된 날짜까지 루프를 수행하기 때문에, 직접 루프의 실행 시간을 지정해줄 수 있다.
- 실행 시간 동안 부탁된, 모든 Input Source를 처리해준다.


### acceptInput(forMode:before:)
- 지정된 날짜까지 또는 지정된 모드에 대해서만 입력을 허용하여 루프를 한 번 실행한다.


## RunLoop를 사용할 떄
- Input Source를 통해 다른 Thread와 통신하는 경우
- Timer를 사용해야 하는 경우
- Perfrom Selector Source를 사용하는 경우
- 주기적인 일을 계속 수행해야 하는 경우





