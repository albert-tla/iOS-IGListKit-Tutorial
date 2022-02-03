# IGListKit Tutorial

This IGListKit tutorial provided by [Raywenderlich](https://www.raywenderlich.com/9106-iglistkit-tutorial-better-uicollectionviews). The reason of using IGListKit because we want different cell in a CollectionView without manually insert a custom cell based on the index.

By using IGListKit, we only need these steps: 
1. Determine what data to show on a ListAdapter sequentally.
2. ListAdapter will pass and ask an appropriate SectionController based on the data class.
3. The appropriate SectionController will generate a cell based on the received data.

## Build the Collection View

Create a constant property of UICollectionView on the desired UIViewController. To customize it, it's recommended to create a custom functionality by using open curly brace as a start and end it with closed curly brace with a `()` to call our function, where it returns a UICollectionView itself.

`
let view: UICollectionView = {
    var view = UICollectionView(
    frame: .zero, 
    collectionViewLayout: UICollectionViewFlowLayout()
    )
    view.backgroundColor = .black
    return view
}()
`

The code above creates a UICollectionView with a black background color. It also has a `.zero` frame, which also equals to:
`CGRect(x: 0, y: 0, width: 0, height: 0)`.

The collectionViewLayout's type is `UICollectionViewFlowLayout()`, which it can create a UICollectionView that has a various kinds of UICollectionViewCell on a row or column depending on the scrolling direction. I found the reason of using this type from the [official documentation](https://developer.apple.com/documentation/uikit/uicollectionviewflowlayout).
