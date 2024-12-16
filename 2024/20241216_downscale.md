# Image DownSampling

## 배운점
이미지를 리사이징 하는 과정에서 
`UIGraphicsBeginImageContextWithOptions`이나
`UIGraphicsImageRenderer `를 사용하고 있었는데 더 다운샘플링을 통해서 메모리와 CPU사용량을 더 절약할 수 있다.

### 장점
1. 원본 이미지를 모두 로드하지 않고, 화면에 필요한 크기만 읽기 때문에 메모리 사용량이 줄어든다.    
2. 대용량 이미지를 화면에 보여줄 때 사용하면 좋다. 


### 단점
1. 원본이미지를 축소하여 픽셀 정보를 줄이는 과정에서 디테일이 손실된다.   
2. 이미지가 저해상도로 보인다.   
3. 텍스트 같은 것들은 쉽지 적합하지 않을 듯


## 코드예시

```swift
func downsample(imageAt imageURL: URL, to pointSize: CGSize, scale: CGFloat) -> UIImage? {
    // 이미지 소스를 생성
    let options: [CFString: Any] = [
        kCGImageSourceShouldCache: false // 캐싱을 방지
    ]
    guard let imageSource = CGImageSourceCreateWithURL(imageURL as CFURL, options as CFDictionary) else {
        return nil
    }
    
    // 다운샘플링 옵션 설정
    let maxDimensionInPixels = max(pointSize.width, pointSize.height) * scale
    let downsampleOptions: [CFString: Any] = [
        kCGImageSourceCreateThumbnailFromImageAlways: true,  // 항상 썸네일을 생성
        kCGImageSourceShouldCacheImmediately: true,         // 메모리로 바로 로드
        kCGImageSourceCreateThumbnailWithTransform: true,   // 이미지 방향 고려
        kCGImageSourceThumbnailMaxPixelSize: maxDimensionInPixels
    ]
    
    // 썸네일 이미지 생성
    guard let downsampledImage = CGImageSourceCreateThumbnailAtIndex(imageSource, 0, downsampleOptions as CFDictionary) else {
        return nil
    }
    
    // UIImage로 변환하여 반환
    return UIImage(cgImage: downsampledImage)
}
```