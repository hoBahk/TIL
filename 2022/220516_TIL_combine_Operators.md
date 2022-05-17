# Combine - Operators

## map
- standard Library의 map 과 같다


## tryMap
-map과 같지만 에러처리를 할 때 사용한다.
- 반환이 throws임

## flatMap
- upstream publisher의 모든 요소를 지정한 최대 publishers 수까지 새로운 publishers로 반환한다.
- 스트림 마다 publisher 하나씩?

```swift
public struct WeatherStation {
    public let stationID: String
}
var weatherPublisher = PassthroughSubject<WeatherStation, URLError>()
let cancellable = weatherPublisher.flatMap(maxPublishers: .max(1), { station -> URLSession.DataTaskPublisher in
    let url = URL(string:"http://localhost:8080/\(station.stationID)")!
    return URLSession.shared.dataTaskPublisher(for: url)
}).sink(
    receiveCompletion: { completion in
        print(completion)
    },
    receiveValue: {
        print($0.data, $0.response)
    }
)
weatherPublisher.send(WeatherStation(stationID: "KSFO")) // San Francisco, CA
weatherPublisher.send(WeatherStation(stationID: "EGLC")) // London, UK
weatherPublisher.send(WeatherStation(stationID: "ZBBB")) // Beijing, CN


```

## mapError
- upstream publisher의 모든 오류를 새로운 오류로 변환한다.

```swift
struct DivisionByZeroError: Error {}
struct MyGenericError: Error { var wrappedError: Error }
func myDivide(_ dividend: Double, _ divisor: Double) throws -> Double {
    guard divisor != 0 else { throw DivisionByZeroError() }
    return dividend / divisor
}
let divisors: [Double] = [5, 4, 3, 2, 1, 0]
divisors.publisher
    .tryMap { try myDivide(1, $0) }
    .mapError { MyGenericError(wrappedError: $0) }
    .sink(
        receiveCompletion: { print ("completion: \($0)") },
        receiveValue: { print ("value: \($0)", terminator: " ") }
    )
```

## replaceNill
- nil을 대채한다.

```swift
let numbers: [Double?] = [1.0, 2.0, nil, 3.0]
numbers.publisher
    .replaceNil(with: 0.0)
    .sink { print("\($0)", terminator: " ") }
```

## scan
- 클로저에서 반환 한 마지막 값과 함께 요소를 클로저에 제공하여 upstream publisher의 요소를 변환합니다.
- 계산 과정에서 생긴 값들을 계속 publish 한다.
- Reduce면 15만 반환하지만 scan은 중간 계산값까지 모두 다!

```swift
let publisher = (0...5).publisher
    .scan(0, { return $0 + $1 })
    .sink(receiveValue: { print ("\($0)", terminator: " ") })

// Prints "0 1 3 6 10 15 "
```

## tryScan
- scan중에 에러가 생길 수 있다.


## setFailureType
- Failure가 Never일 때, Failure 타입을 변경할 수 있다.

```swift
let pub1 = [0, 1, 2, 3, 4, 5].publisher
let pub2 = CurrentValueSubject<Int, Error>(0)
let cancellable = pub1
    .setFailureType(to: Error.self)
    .combineLatest(pub2)
    .sink(
        receiveCompletion: { print ("completed: \($0)") },
        receiveValue: { print ("value: \($0)")}
    )
// Prints: "value: (5, 0)".
```


## filter
- Standard Library의 filter와 같음


## tryFilter
- 앞에 try가 붙으면 에러를 던질 수 있는 closure를 넣을 수 있다.

## compactMap
- nil이 아닌 요소만 다운스트림에 publish 한다.

## tryCompactMap
- ㅎㅎ 알지요

## 	removeDuplicates
- 바로 이전 요소와 일치하지 않는 것만 publish 한다.

```swift
let numbers = [0, 1, 2, 2, 3, 3, 3, 4, 4, 4, 4, 0]
cancellable = numbers.publisher
    .removeDuplicates()
    .sink { print("\($0)", terminator: " ") }

// Prints: "0 1 2 3 4 0"
```

## removeDuplicates(by:)
- 클로저 안에 일치한다는 기준을 정할 수 있음

```swift
struct Point {
    let x: Int
    let y: Int
}
let points = [Point(x: 0, y: 0), Point(x: 0, y: 1),
              Point(x: 1, y: 1), Point(x: 2, y: 1)]
let cancellable = points.publisher
    .removeDuplicates { prev, current in
        prev.x == current.x
    }
    .sink { print("\($0)", terminator: " ") }
// Prints: Point(x: 0, y: 0) Point(x: 1, y: 1) Point(x: 2, y: 1)
```

## tryRemoveDuplicates(by:)
- try


## replaceEmpty(with:)
- 빈 스트림을 제공된 요소로 바꾸는 기능을 한다.

```swift
let numbers: [Double] = []
cancellable = numbers.publisher
    .replaceEmpty(with: Double.nan)
    .sink { print("\($0)", terminator: " ") }

// Prints "(nan)".

```


## replaceError(with:)

- 에러를 제공된 요소로 변경하는 기능

```swift
struct BadValuesError: Error {}
let numbers = [0, 0, 0, 0, 1, 2, 2, 3, 3, 3, 4, 4, 4, 4]
let cancellable = numbers.publisher
    .tryRemoveDuplicates { first, second -> Bool in
        if (first == 4 && second == 4) {
            throw BadValuesError()
        }
        return first == second
    }
    .replaceError(with: 10000)
    .sink(
        receiveCompletion: { print ("\($0)") },
        receiveValue: { print ("\($0)", terminator: " ") }
    )
// Prints: "0 1 2 3 4 10000 finished"
```
