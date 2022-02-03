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

## ListAdapter and Data Source

To use the IGListKit, we also need to create the ListAdapter, to control our predefined UICollectionView, and its data source, just as we create a normal UICollectionView.

### ListAdapter

Build a lazy-variable property of the ListAdapter. I assume the usage of `lazy var` on the ListAdapter because it has some expensive work that may slow down the user's phone based on the [HackingWithSwift's definition of it](https://www.hackingwithswift.com/example-code/language/what-are-lazy-variables).

`
lazy var adapter: ListAdapter = {
    ListAdapter(
    updater: ListAdapterUpdater(),
    viewController: self,
    workingRangeSize: 0
    )
}()
`

There are three required parameters: 
1. `updater`.
2. `viewController`.
3. `workingRangeSize`. 

The `updater` is the one who handles any updates on the ListAdapter. By default, use the `ListAdapterUpdater()`, I haven't deep dive is there any other `updater`, and I didn't found it at the [IGListKit official documentation](https://instagram.github.io/IGListKit/index.html).

The `viewController` is to tell which UIViewController will have the created adapter. The definition can be found at the [official documentation](https://instagram.github.io/IGListKit/Classes/IGListAdapter.html#/c:objc(cs)IGListAdapter(py)viewController).

The `workingRangeSize` is numbers of contents that doesn't appear in the UIView, and the ListAdapter wants to prepares it before comes to the screen. The number applies to the entrance and the exit of the range.

For instance, we set the `workingRangeSize: 1`, then the ListAdapter prepares for one at both entrance and exit. The full description can be found at the [official documentation](https://instagram.github.io/IGListKit/getting-started.html#working-range).
