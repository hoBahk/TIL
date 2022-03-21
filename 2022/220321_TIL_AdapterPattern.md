## Adapter Pattern은 무엇인가?

- 두 개의 객체가 유사한 기능을 하지만 호환되지 않는 인터페이스를 가졌을 때 함께 작동할 수 있도록 해주는 구조설계 패턴입니다.
- 특정 객체의 인터페이스를 변환하여 다른 객체와 함께 사용할 수 있도록 해줍니다.

![](https://images.velog.io/images/qudgh849/post/4ef69402-f323-4776-b7f4-9451d00d0887/image.png)

### Client
- Protocol을 의존
- 기존에 사용하고 있던 로직을 포함하는 객체

### Protocol
- 기존의 로직을 프로토콜화 (Client가 사용중인 인터페이스)
- 다른 객체가 현재 로직에 포함 되려면 따라야하는 프로토콜

### Adapter
- Protocol을 준수하면서 Client, Adaptee 두 객체를 상호작용하기 위해 만들어진 객체
- Adaptee 객체를 Client 인터페이스로 래핑하거나 Client 객체를 Adaptee가 사용할 수 있는 형식으로 변환

### Adaptee
- 외부 라이브러리 및 외부 시스템 객체

<br>
<br>

## 코드 예제

### Adaptee
- 네이버 로그인을 위한 3rd party 클래스입니다.

```swift
public class NaverAuthenticator {
    public func login(
        email: String,
        password: String,
        completion: @escaping (NaverUser?, Error?) -> Void
    ) {
        let token = "token"
        let user = NaverUser(email: email,
                             password: password,
                             token: token)
        completion(user, nil)
    }
}

public struct NaverUser {
    public var email: String
    public var password: String
    public var token: String
}
```



### Protocol

- email, password를 필수로 받고 로그인에 성공할 경우 success를 호출해 User와 Token을 사용합니다.
- 3rd party객체를 사용하지 않고 해당 프로토콜을 사용하여 로그인을 인증합니다.
- 다른 3rd party 로그인도 해당 프로토콜로 단순화 할 수 있습니다.

```swift
protocol AuthenticationService {
    func login(
        email: String,
        password: String,
        success: @escaping (User, Token) -> Void,
        failure: @escaping (Error?) -> Void
    )
}

struct User {
    public let email: String
    public let password: String
}

struct Token {
    public let value: String
}
```

### Adapter

- Protocol을 채택하고 login() 메서드를 구현합니다.
- NaverAuthenticator의 인스턴스를 private으로 생성합니다.
- success, failure을 처리합니다.
- NaverAPI를 직접적으로 알지 않아도 됩니다.
- 유지보수를 할 때 변경사항이 생기면 adapter만 수정하면 됩니다.

```swift
class NaverAuthenticatorAdapter : AuthenticationService {
    private var authenticator = NaverAuthenticator()
    func login(
        email: String,
        password: String,
        success: @escaping (User, Token) -> Void,
        failure: @escaping (Error?) -> Void
    ) {
        authenticator.login(email: email, password: password) { (naverUser, error) in
            guard let naverUser = naverUser else {
                failure(error)
                return
            }
            let user = User(email: naverUser.email, password: naverUser.password)
            let token = Token(value: naverUser.token)
            success(user, token)
        }
    }
}
```


### 사용
- AuthManger
	- Protocol을 의존
	- authService.login() 호출
- API의 Adapter를 구현해 놓으면, AuthManager의 인스턴스를 생성할 때 Adapter만 갈아 끼우면 됩니다!

```swift
class AuthManager {
    var authService : AuthenticationService?
    
    init(authService: AuthenticationService) {
        self.authService = authService
    }
    
    func login() {
        authService?.login(
            email: "email",
            password: "password",
            success: { user, token in
                print("Auth succeeded: \(user.email), \(token.value)")
            },
            failure: { error in
                print("Auth failed")
            })
    }
}

let authManager = AuthManager(authService: NaverAuthenticatorAdapter())
authManager.login()
```

<br>

## 정리 & 결론
- Adapter Pattern을 사용하여 API를 통합하여 사용할 수 있기 때문에 다른 인터페이스를 가진 객체를 사용해야할 때 부담을 줄일 수 있습니다.
- 코드의 명확성과 단순성을 높여 외부 개체를 쉽게 통합할 수 있게 해줍니다.
- 하나의 코드로 Adapter만 바꿈으로서 유연한 대처가 가능합니다.
- 코드를 수정하는데에 부담이 줄어 후에 수정이 될 것 같은 코드라면 사용하는 것이 좋습니다.

<br>

## 참고
https://ko.wikipedia.org/wiki/%EC%96%B4%EB%8C%91%ED%84%B0_%ED%8C%A8%ED%84%B4 
https://haningya.tistory.com/265 
https://icksw.tistory.com/241 
https://medium.com/swiftcraft/swift-solutions-adapter-pattern-a2118a6a2910 