# UIGraphicsImageRenderer

기존에 `UIGraphicsBeginImageContextWithOptions` API를 사용하여 이미지를 렌더링 할 수 있었다.

iOS10 부터는 `UIGraphicsImageRenderer`를 사용할 수 있다.

이미지 포멧은 여러가지가 있고 포멧마다 사용하는 용량이 다른데 기존 방법으로 하면 1pexel 당 8byte가 잡혀있기 때문에 메모리 공간을 매우 비효율적으로 사용하게 된다.

그래서 새로운 방법이 나왔고 이것을 사용하면 포멧을 iOS에서 1pixel당 1byte만 사용한다.   
이것은 1pixel 당 1byte만 메모리에 할당하여 그린 후, imageView에 Color 정보를 적용하는 방식이다.   


## 이미지 메모리

이미지를 구성하고 있는 pixel 관점에서 1 pixel을 이룰 때 RGB요소에 의해 각 1바이트씩 3개가 필요하고 alpha 채널까지 합치면 1 pixel 당 4바이트가 필요하다.   

그래서 1024 x 1024 하면 총
1024 x 1024 x 4 하기 때문에 4MB가 필요하게 된다.  


흐음.. 쉽지 않구만  
`UIGraphicsImageRenderer`로 리사이징 해서 서버에 보내면 AI 서버에 보내면 잘 인식하지 못한다.  
색 요소가 없어서 그런거 같다.   
그럼 이건 iOS에서만 처리할 때 쓸 수 있는건가?