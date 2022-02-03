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

The `viewController` is to tell which ViewController will have the created adapter. The definition can be found at the [official documentation](https://instagram.github.io/IGListKit/Classes/IGListAdapter.html#/c:objc(cs)IGListAdapter(py)viewController).

The `workingRangeSize` is numbers of contents that doesn't appear in the UIView, and the ListAdapter wants to prepares it before comes to the screen. The number applies to the entrance and the exit of the range.

For instance, we set the `workingRangeSize: 1`, then the ListAdapter prepares for one at both entrance and exit. The full description can be found at the [official documentation](https://instagram.github.io/IGListKit/getting-started.html#working-range).

### Data Source

When we set the predefined `adapter.dataSource = self`, the compiler will say error because the ViewController hasn't conform to the `ListAdapterDataSource`. In this tutorial, there are three main functions to be used:
1. `objects(for listAdapter: ListAdapter) -> [ListDiffable]`
2. `listAdapter(_ listAdapter: ListAdapter, sectionControllerFor objects: Any) -> ListSectionController`
3. `emptyView(for listAdapter: ListAdapter) -> UIView? `

The `objects` function is to set what kind of data you want to show at the data source. I assume we need to sort what data to show at the top until the bottom from this function.

My assumption is based on the [tutorial when we add the Weather to the ListDiffable array](https://www.raywenderlich.com/9106-iglistkit-tutorial-better-uicollectionviews#toc-anchor-009). I didn't see anything about order in the [official documentation](https://instagram.github.io/IGListKit/Protocols/IGListAdapterDataSource.html#/c:objc(pl)IGListAdapterDataSource(im)objectsForListAdapter:).

The `listAdapter` function will ask the data source to redirect to the SectionController. The `objects` parameter may be used to check whether it fits to the proper SectionController.

Just as the tutorial, we implements an if-statements to check the `objects` parameter with each statement returns the fit SectionController. There are no other explanation in the [official documentation](https://instagram.github.io/IGListKit/Protocols/IGListAdapterDataSource.html#/c:objc(pl)IGListAdapterDataSource(im)listAdapter:sectionControllerForObject:).

The `emptyView` function will ask the data source to show a UIView, as a background view, when the list is empty. Do note whenever the ListAdapter is updated, it also call this function, based on the [official documentation](https://instagram.github.io/IGListKit/Protocols/IGListAdapterDataSource.html#/c:objc(pl)IGListAdapterDataSource(im)emptyViewForListAdapter:).

## Section Controller

The SectionController is an abstract, of given data object to control the UICollectionViewCell. It has similarity to a ViewModel (data object) controls a View (UICollectionViewCell).

For some reasons, the Xcode only recognize IGListSectionController subclass instead of ListSectionController. Do note when creating a SectionController, even the created file is conforming the ListSectionController, don't forget to `import IGListKit` otherwise every overridden functions may not appear.

In this tutorial, we're introduced one main overridden function and four overridden data provider functions:
1. `init()`
2. `didUpdate(to object: Any)`
3. `numberOfItems() -> Int`
4. `sizeForItem(at index: Int) -> CGSize`
5. `cellForItem(at index: Int) -> UICollectionViewCell`

The `init` function is self explanatory and similar to the `init()` function in a UIViewController. If there's any inset on a previous (left or top) or next (right or bottom) SectionController, those implementation is best placed at this function.

The `didUpdate` function is to give this SectionController an `object: Any` parameter. Because it has `Any` type, before we add to our SectionController's property, we need to check if the `object` has a same type with it.

The tutorial states [this function will be called first before any cell protocol functions](https://www.raywenderlich.com/9106-iglistkit-tutorial-better-uicollectionviews#toc-anchor-007). The [official document](https://instagram.github.io/IGListKit/Classes/IGListSectionController.html#/c:objc(cs)IGListSectionController(im)didUpdateToObject:) didn't have something about being first called, instead it talks about being this function will be invoked every time the `object` parameter changes.

The `numberOfItems` function is returning the number of cells displayed in this SectionController. If you didn't override this function, it'll return 1 by default based on the [official documentation](https://instagram.github.io/IGListKit/Classes/IGListSectionController.html#/c:objc(cs)IGListSectionController(im)numberOfItems).

The `sizeForItem` function is to return the size of each UICollectionViewCell, or known as `CGRect`. By default it'll return zero CGRect based on the [official documentation](https://instagram.github.io/IGListKit/Classes/IGListSectionController.html#/c:objc(cs)IGListSectionController(im)sizeForItemAtIndex:).

To make our cell's width is equals to the container's width, we can access it through the `collectionContext` by make a `guard let context = collectionContext` and then access it from `context.containerSize.width`. In the [official documentation](https://instagram.github.io/IGListKit/Classes/IGListSectionController.html#/c:objc(cs)IGListSectionController(py)collectionContext), it states the reason why the `collectionContext` is an optional variable but not nullable.

The `cellForItem` function is to return a UICollectionView cell for each index. This function is to set our customized cell that conforms UICollectionViewCell class, with the SectionController's property from the `didUpdate` function, and then we return it.

To use this overridden function, it has these steps:
1. Define the customized cell that conforms UICollectionViewCell.
2. Create the cell by dequeuing cell from the `collectionContext!.dequeueReusableCell(of: UICollectionViewCell, for: ListSectionController, at: Int)`.
3. Set the value to the predefined cell.
4. Return the cell.

From four data provider functions above, the [official documentation](https://instagram.github.io/IGListKit/Classes/IGListSectionController.html) states, that four of them is not required to override it by calling `super`, except for the `cellForItem` function. It states we should not call `super` on it.

## Questions I've Had

1. Do we have to sort what data to show on the ListDiffable array?
2. When creating the [JournalSectionController](https://www.raywenderlich.com/9106-iglistkit-tutorial-better-uicollectionviews#toc-anchor-007), the `numberOfItems` function has an implementation of `return 2`. But how does it returns four cells?

## Acknowledgement

Big thanks to [Randy Efan](https://github.com/randyefan1998), [Alnodi Adnan](https://github.com/alnodiadnan08), and [Aldo Leonardo](https://github.com/aldoleonardo) for referencing this tutorial to us.
