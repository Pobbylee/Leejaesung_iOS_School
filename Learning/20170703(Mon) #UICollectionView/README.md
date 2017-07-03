# 패스트캠퍼스 강의 노트 38th ( 20170703 )

# 오늘의 Tip
 - 테이블 뷰에서 커스텀 `Cell`을 만들 때, `Xib(닙)`으로 만들 수 있다.


# UICollectionView

 - `Cell`
 - `Decoration view`
 - `Supplementary view`: Header, Footer를 포함하는 뷰.

![UICollectionView.png](UICollectionView.png)

![UICollectionView Story.png](UICollectionView Story.png)


## Presentation
 - `UICollectionReusableView`
 - `UICollectionViewCell`
	 - `UICollectionView`는 위 2가지로 모든게 해결된다.

## Layout
 - `UICollectionViewLayout`
 - `UICollectionViewLayoutAttributes`
 - `UICollectionViewUpdateItem`
	 - `UICollectionView`는 테이블 뷰와는 다르게 좌우로 자유롭게 움직일 수 있고, Layout 안에서도 스크롤이 가능하기 때문에 Layout 이 따로 빠져 있다.

### Flow Layout & Custom Layout
 - Flow Layout이 UICollectionView의 기본적인 구조.

![Layout.png](Layout.png)

### Cell Sapcing & Line Spacing & Section Inset

![Section Layout.png](Section Layout.png)


## Protocol
 - `UICollectionViewDelegate`
 - `UICollectionViewDataSourcePrefetching`: Deque 하기 전에 미리 데이터를 땡겨와서 성능 향상을 한다.


# DataSource & Delegate

## `UICollectionViewDataSource`

```swift
// MARK: UICollectionViewDataSource

override func numberOfSections(in collectionView: UICollectionView) -> Int {
    // #warning Incomplete implementation, return the number of sections
    return 1
}


override func collectionView(_ collectionView: UICollectionView, numberOfItemsInSection section: Int) -> Int {
    // #warning Incomplete implementation, return the number of items
    return DataCenter.center.friendList.count
}

override func collectionView(_ collectionView: UICollectionView, cellForItemAt indexPath: IndexPath) -> UICollectionViewCell {
    let myCell:BasicCollectionViewCell = collectionView.dequeueReusableCell(withReuseIdentifier: reuseIdentifier, for: indexPath) as! BasicCollectionViewCell
    
    myCell.set(title: DataCenter.center.friendList[indexPath.item])
    
    return myCell
}
```

## `UICollectionViewDelegate`

```
// Methods for notification of selection/deselection and highlight/unhighlight events.
// The sequence of calls leading to selection from a user touch is:
//
// (when the touch begins)
// 1. -collectionView:shouldHighlightItemAtIndexPath:
// 2. -collectionView:didHighlightItemAtIndexPath:
//
// (when the touch lifts)
// 3. -collectionView:shouldSelectItemAtIndexPath: or -collectionView:shouldDeselectItemAtIndexPath:
// 4. -collectionView:didSelectItemAtIndexPath: or -collectionView:didDeselectItemAtIndexPath:
// 5. -collectionView:didUnhighlightItemAtIndexPath:
```

# [ Sample ] Multiple-Selection

## 1. MultipleSelection 허용

```swift
self.collectionView?.allowsMultipleSelection = true
```

```obj-c
@property (nonatomic) BOOL allowsSelection; // default is YES

- (BOOL)collectionView:(UICollectionView *)collectionView
// called when the user taps on an already-selected item in multi-select mode

## 2. 삽입 / 삭제 / 이동

 - 두가지 스텝을 필수로 진행해야한다.
```

### `[Sample] Delete`

```swift
func deleteSelectedItem()
		//완료후 행동
}
```


# [Sample] LongPressGesture

 - LongPressGesture로 CollectionView의 요소들을 Touch-Drag 하기.

```swift
class ViewController: UIViewController, UICollectionViewDataSource, UICollectionViewDelegateFlowLayout {
  
 var longPressGesture:UILongPressGestureRecognizer?
  
 override func viewDidLoad() {
    super.viewDidLoad()
    data = CollectionData()
    
   longPressGesture = UILongPressGestureRecognizer(target: self, action: #selector(ViewController.handleLongGesture(gesture:)))
	}


func handleLongGesture(gesture: UILongPressGestureRecognizer) {
    switch(gesture.state) {
  
   case .began:
      guard let selectedIndexPath = self.collectionView.indexPathForItem(at: self.collectionView)) else { break }
      print(“began”)
      collectionView.beginInteractiveMovementForItem(at:selectedIndexPath)
   case .changed:
   collectionView.updateInteractiveMovementTargetPosition(gesture.location(in:gesture.view!))
      print(“move”)
    case .ended:
      collectionView.endInteractiveMovement()
      print(“end”)
    default:
      collectionView.cancelInteractiveMovement()
    }
  }

}
```

---
### 문서 끝 ( by 재성 )