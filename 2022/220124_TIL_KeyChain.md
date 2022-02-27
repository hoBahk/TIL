## KeyChain

### 간단 설명

- 애플이 제공하는 보안 프레임워크
- 키체인은 디바이스 안에 암호화된 데이터 저장공간을 의미한다.
- 사용자는 암호화된 공간에 데이터를 안전하게 보관할 수 있다.

### 주로 저장 하는 데이터

- 로그인 정보(패스워드 해시)
- 결제 정보
- 암호화를 위한 키
- 등등..

### 특징

- 사용자가 직접제거하지 않으면 앱을 종료하고 켜는 것은 물론 제거했다가 다시 설치해도 데이터는 남는다.
- 디바이스를 Lock하게 되면 키체인도 잠기게 된다.
- UserDefaults는 탈옥을 하게 되면 그 정보를 볼 수가 있는데 키체인은 탈옥하여도 그 정보를 볼 수 없어 더 안전하다.

### kSecClass 종류

- kSecClassInternetPassword - 인터넷용 로그인 정보 (WIFI 등등)
- kSecClassCertificate - 인증서 정보
- kSecClassGenericPassword - 일반적인 비밀번호
- kSecClassIdentify
- kSecClassKey

구현은 좀 더 해봐야할 것 같다.