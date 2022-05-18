# Combine - Operator

## collect()

- 받은 element를 모두 수집하고, upstream publisher가 완료되면 collection의 single array를 내보낸다.
- element를 저장했다가 한번에 배열로 뿌려주어야 하니 메모리를 사용한다.
- element를 무한대로 collect할 수 있다.
- element가 너무 많으면 메모리 부하가 발생할 수 있다.

```swift
let numbers = (0...10)
let cancellable = numbers.publisher
	.collect()
	.sink { print("\($0)") }
	
// Prints: "[0, 1, 2, 3, 4, 5, 6, 7, 8, 9, 10]"
```

### collect(_ count: Int)
-  파라미터 count에 배열 요소의 최대 수를 넣는다.
-  count에 4를 넣고 요소가 10개라면 4, 4, 2 수의 요소를 같는 배열이 만들어진다.

### collect(_ strategy: options:)
#### strategy 파라미터
- strategy 파라미터는 내가 지정한 Scheduler/Stride로 element 배열을 내보내는 것이다.
- `.byTime`과 `.byTimeOrCount` 두 가지가 있다.

```swift
let sub = Timer.publish(every: 1. on: .main. in: default)
	.autoconnect()
	.collect(.byTIme(RunLoop.main, .seconds(5))
	.sink { print("\($0)", terminator: "\n\n") }
```
예제 설명  
- Timer를 1초마다 Date를 내놓는다.
- stride를 5초를 주었다. (stirde는 주기를 나타는 것 같다. 5초마다 collect하겠다.)
- byTImeOrCount는 뒤에 `count`파라미터가 붙는데 이것은 위의 메서드와 같이 배열 요소의 최대 수를 나타낸다.

![](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2F9FIYy%2FbtqEChhYNk7%2Fsu6vLNXRUdUNAJF45JJ770%2Fimg.png)

#### optinos
- Scheduler의 옵션을 따른다.

## ignoreOutput()
- 출력을 무시하고 완료, 실패인지만 전달한다.

```swift
let numbers = [1, 2, 3, 4, 5, 0, 6, 7, 8, 9]
let cancellable = numbers.publisher
	.ignoreOutput()
	.sink(receiveCompletion: {print("completion: \($0)")},
receiveValue: { print("\($0)", terminator: " ")})

// completion: finished
```

## reduce(_:, _:)
- `scan`이라는 메서드는 연산 과중의 나오는 중간 값을 모두 publish하지만,
- `reduce`는 최종 결과 값만 publish한다.

## count
- 개수를 publiish 한다.

## max
- 가장 큰 값을 publish 한다.

## max(by:)
- Standard Libarary의 max(by:)와 같다.
- Bool, Closure 들어갈 수 있다.

## min
- 가장 작은 값


## min(by:)
- Standard Libarary의 min(by:)와 같다.
- Bool, Closure 들어갈 수 있다.

## drop(untilOuputFrom:)
- 두번째 publisher로 부터 요소를 받을 때 까지 업스트림 publisher(첫번쨰 publisher)의 요소를 무시한다.
- 아래 예제를 보면 first publisher를 send 해도 second publisher가 send 되기 전에는 무시된다.

```
let first = PassthroughSubject<Int,Never>()
let second = PassthroughSubject<String,Never>()
let cancellable = first
	.drop(untilOutputFrom: second)
	.sink { print("\($0)", terminator: " ") }
	
first.send(1)
first.send(2)
second.send("A")
first.send(3)
first.send(4)

// Prints "3 4"
```


## dropFirst(_ count:)
- `count` 파라미터에 넣은 갯수만큼 생략한다.

```swift
let numbers = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10]
let cancellable = numbers.publisher
	.dropFirst(5)
	.sink { print("\($0)", terminator: " ") }
	
// Prints: "6 7 8 9 10 "
```

## drop(while:)
- `while`파라미터에 들어간 클로저에서 false를 반환할 때까지 요소를 생략한다.

```swift
let numbers = [-62, -1, 0, 10, 0, 22, 41, -1, 5]
let cancellable = numbers.publisher
	.drop { $0 <= 0 }
	.sink { print("\($0)") }

// Prints: "10 0 22 41 -1 5"
```

## append(_:)
- output에 특정 요소를 뒤에 추가해준다.
- 숫자, 배열, 퍼블리셔 등 추가 가능하다.

```swift
let dataElements = (0...10)
let cancellable = dataElements.publisher
	.append(0, 1, 255)
	.sink { print("\($0)", terminator: " ") }
	
// Prints: "0 1 2 3 4 5 6 7 8 9 10 0 1 255"
```

## prepend(_:)
- append 반대, 앞에 붙힌다.


## prefix(_:)
- `maxLength` 파라미터를 받아 그 개수만큼 다시 republish한다.

```swift
let numbers = (0...10)
let cancellable = numbers.publisher
	.prefix(2)
	.sink { print("\($0)", terminator: " ") }

// Prints: "0 1"
```

## prefix(while:)
- `while`파라미터에서 flase 반환할 때 까지 republish 한다.

```swift
let numbers = (0...10)
numbers.publisher
	.prefix { $0 < 5 }
	.sink { print("\($0)", terminator: " ") }

// Prints: "0 1 2 3 4"
```

## prefix(untilOutputFrom:)
- 다른 publisher가 요소를 보낼 때까지 요소를 republish한다.
- drop(untilOutputFrom:)은 다른 publisher가 보낼 때까지 무시했다. (prefix와 반대임)

```swift
let first = PassthroughSubject<Int,Never>()
let second = PassthroughSubject<String,Never>()
let cancellable = first
	.prefix(untilOutputFrom: second)
	.sink { print("\($0)", terminator: " ") }
	
first.send(1)
first.send(2)
second.send("A")
first.send(3)
first.send(4)

// Prints "1 2"
```

참고
[Zedd 블로그 Combine - Operator](https://zeddios.tistory.com/1039?category=842493)


다음 스텝  
[combineOperator(6) - Applying Matching criteria to Elements](https://zeddios.tistory.com/1076?category=842493)
.... 9까지 있음..

