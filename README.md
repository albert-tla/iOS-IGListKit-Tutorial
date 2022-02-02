# IGListKit Tutorial

This IGListKit tutorial provided by [Raywenderlich](https://www.raywenderlich.com/9106-iglistkit-tutorial-better-uicollectionviews). The reason of using IGListKit because we want different cell in a CollectionView without manually insert a custom cell based on the index.

By using IGListKit, we only need these steps: 
1. Determine what data to show on a ListAdapter sequentally.
2. ListAdapter will pass and ask an appropriate SectionController based on the data class.
3. The appropriate SectionController will generate a cell based on the received data.

