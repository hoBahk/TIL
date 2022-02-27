# CollectionView

## CollectionView 메서드
### UICollectionViewDataSource 메서드
```swift
extension CollectionViewController: UICollectionViewDataSource {
	// item 개수를 반환하는 함수
  func collectionView(_ collectionView: UICollectionView, numberOfItemsInSection section: Int) -> Int {
    return 10
  }
  
	// item에 보여줄 데이터를 셀에 지정 후 셀 반환
  func collectionView(_ collectionView: UICollectionView, cellForItemAt indexPath: IndexPath) -> UICollectionViewCell {
    guard let cell = collectionView.dequeueReusableCell(withReuseIdentifier: "ProductCollectionViewGridCell", for: indexPath) as? ProductCollectionViewGridCell else {
      return UICollectionViewCell()
    }
    // ...
    return cell
  }
}
```

### UICollectionViewDelegateFlowLayout 메서드
UICollectionViewFlowLayout 객체와 상호작용하여 레이아웃을 조정할 수 있는 메서드가 정의되어있다. 셀의 크기와 셀간의 간격을 정의하는 메서드가 있다.

```swift
// 지정된 셀의 크기를 반환하는 메서드
func collectionView(_ collectionView: UICollectionView, 
                   layout collectionViewLayout: UICollectionViewLayout, 
            sizeForItemAt indexPath: IndexPath) -> CGSize
            
// 지정된 섹션의 여백을 반환하는 메서드
func collectionView(_ collectionView: UICollectionView, 
                   layout collectionViewLayout: UICollectionViewLayout, 
                   insetForSectionAt section: Int) -> UIEdgeInsets

// 지정된 섹션의 행 사이 간격 최소 간격을 반환하는 메서드. 
// scrollDirection이 horizontal이면 수직이 행이 되고 vertical이면 수평이 행이 됨
func collectionView(_ collectionView: UICollectionView, 
                   layout collectionViewLayout: UICollectionViewLayout,
                   minimumLineSpacingForSectionAt section: Int) -> CGFloat

//지정된 섹션의 셀 사이의 최소간격을 반환하는 메서드
func collectionView(_ collectionView: UICollectionView, 
                   layout collectionViewLayout: UICollectionViewLayout,
                   minimumInteritemSpacingForSectionAt section: Int) -> CGFloat

// 지정된 섹션의 헤더뷰의 크기를 반환하는 메서드 
// 크기를 지정하지 않으면 화면에 보이지 않는다.
func collectionView(_ collectionView: UICollectionView, 
                   layout collectionViewLayout: UICollectionViewLayout,
                   referenceSizeForHeaderInSection section: Int) -> CGSize

// 지정된 섹션의 푸터뷰의 크기를 반환하는 메서드
// 크기를 지정하지 않으면 화면에 보이지 않는다.
func collectionView(_ collectionView: UICollectionView, 
                   layout collectionViewLayout: UICollectionViewLayout,
                   referenceSizeForFooterInSection section: Int) -> CGSize

```
