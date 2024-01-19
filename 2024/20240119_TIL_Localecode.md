# LocalCode

디바이스에서 설정된 언어 코드와 지역 코드를 불러오는 방법

## 언어 코드

```swift
    var countryCodeString: String {
        guard let localeId = Locale.preferredLanguages.first,
              let countryCode = (Locale(identifier:localeId).languageCode) else {
            return "ko"
        }
        return countryCode
    }
```

## 지역 코드

```swift
    var countryCodeString: String {
        guard let localeId = Locale.preferredLanguages.first,
              let countryCode = (Locale(identifier:localeId).countryCode) else {
            return "KR"
        }
        return countryCode
    }
```