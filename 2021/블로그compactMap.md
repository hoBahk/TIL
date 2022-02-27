고차함수 (compactMap, flatMap)


안녕하세요

저번에는 고차함수 중  map, filter, reduce에 대해 다뤄 보았는데요.

이번에는 compactMap, flatMap에 대해서 알아보겠습니다.


## compactMap
기본적으로 map과 같은 기능을 하지만 compactMap은 map과 다르게 옵셔널이 포함된 1차원 배열에서 nil을 제거하고 옵셔널 바인딩을 해줍니다.    
```swift
let arrayCompactMap = [1, nil, 3, 4, nil, 6]

//축약형
let arrayCompactMapResult = arrayCompactMap.compactMap{ $0 }

//기본형
let arrayCompactMapResult = arrayCompactMap.compactMap( {
    (value: Int?) -> Int? in return value
} )

print(arrayCompactMapResult) // [1, 3, 4, 6]
```

## flatMap
Swift 4.1부터는 flatMap이 compactMap함수로 변경되고 flatMap은 새로운 기능을 얻었습니다.
1차원 배열에서는 compactMap과 같은 기능을 합니다. nil을 제거 하고 옵셔널 바인딩을 해줍니다.
2차원 배열부터는 다른 기능을 합니다. flatMap은 n차원 배열부터는 해당 배열을 n-1차원 배열로 flat하게 만들어주는 기능을 합니다. 

```swift
let arrayFlatMap = [[1] , [2, 3], [4, 5, 6]]

//축약형
let arrayFlatMapResult = arrayFlatMap.flatMap{ $0 }

print(arrayFlatMapResult) // [1, 2, 3, 4, 5, 6]
```