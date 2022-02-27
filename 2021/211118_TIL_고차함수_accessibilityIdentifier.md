# 학습내용

## 고차함수
오늘 학습활동에서 고차함수를 다루었다.
map, compactMap, forEach, filter는 저번주에 다뤄 보았고 오늘 새롭게 다뤄본 reduce, sorted, flatMap에 대해 공부해보았다.

### reduce 
reduce는 결합하는 함수이다. 배열의 요소들을 계산해서 연산결과가 리턴되거나 문자열 배열일 경우에는 문자열이 리턴 될 수 있다.

예제
```swift
let ages = [11, 22, 33, 44, 55]

//기본형
let ageSum = ages.reduce(0, { (initial: Int, next: Int) -> Int in return initial + next } )
//축약형
let ageSum2 = ages.reduce(0) { $0 + $1 }

print(ageSum) // 165
```

### sorted
sorted는 정렬할 때 쓰는 함수이다.   

예제
```swift
let planets = ["Mercurius", "Venus", "Earth", "Mars", "Jupiter", "Saturnus", "Uranus", "Neptunus"]

if let longPlanet = planets.sorted(by: {$0.count > $1.count}).first {
    print(longPlanet) //Mercurius
}
```


### flatMap
Swift 4.1부터는 flatMap이 compactMap함수로 변경되고 flatMap은 새로운 기능을 얻었다.
1차원 배열에서는 compactMap과 같은 기능을 한다. nil을 제거 하고 옵셔널 바인딩을 해준다.
2차원 배열부터는 다르다. flatMap은 n차원 배열부터는 해당 배열을 n-1차원 배열로 flat하게 만들어주는 기능을 한다. 

예제
```swift
let groupArray = [[1], [2, 2], [3, 3, 3], [4, 4, 4, 4], [5, 5, 5, 5, 5]]
let integratedArray = groupArray.flatMap{ $0 }
print(integratedArray) // [1, 2, 2, 3, 3, 3, 4, 4, 4, 4, 5, 5, 5, 5, 5]
```

## tag, accessibilityIdentifier
UI요소들은 태그라는 식별자를 가진다. 태그는 Int형이다. 이것을 통해서 뷰컨에서 따로 인스턴스를 생성해서 인스턴스 이름으로 식별하지 않아도 되고 태그로 식별할 수 있다.

button, label 등 작은 단위의 UI요소들은 태그도 있지만 `accessibilityIdentifier`도 가지고 있다. 같은 식별자이다. 하지만 `accessibilityIdentifier`은 String? 형이다. 옵셔널 스트링이지만 문자열이기 때문에 사람이 식별할 때 좀 더 편리할 것이라는 생각을 했다.

accessibility에는 `accessibilityIdentifier`말고도 `accessibilityLabel`도 있다.    
`accessibilityLabel`은 사용자를 대상으로 하는 값이다. 보이스오버 사용자들은 `accessibilityLabel`에 들어있는 값을 듣게 된다.
`accessibilityIdentifier`는 개발자 전용으로 사용하는 식별자이다. 개발자가 알기 쉬운 식별자를 사용해야한다.