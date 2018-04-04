# CollectionNode 
[![Carthage compatible](https://img.shields.io/badge/Carthage-compatible-4BC51D.svg?style=flat)](https://github.com/Carthage/Carthage) 
[![Badge w/ Version](https://cocoapod-badges.herokuapp.com/v/NSStringMask/badge.png)](https://cocoapods.org/pods/CollectionNode) 
![MIT](https://cocoapod-badges.herokuapp.com/l/NSStringMask/badge.png) 
![Swift 4.0.x](https://img.shields.io/badge/Swift-4.0.x-orange.svg)
[![Build Status](https://travis-ci.org/bwide/CollectionNode.svg?branch=master)](https://travis-ci.org/bwide/CollectionNode)

 A collectionView made for Sprite Kit
 
 ![Preview](https://github.com/bwide/BWCollectionView/blob/master/iphonePreview.gif)

## installation

### Carthage

[Carthage](https://github.com/Carthage/Carthage) is a dependency manager that provides binary frameworks for your projects.

you can install Carthage through [Homebrew](http://brew.sh/), with the following command:

```bash
$ brew update
$ brew install carthage
```

Then you need to tell carthage to integrate this framework in your Xcode project, by adding the following to your [Cartfile](https://github.com/Carthage/Carthage/blob/master/Documentation/Artifacts.md#cartfile):

```ruby
github "bwide/CollectionNode"
```

Now:

1. On your project folder, run `carthage update` 
1. On your application target, drag `BWCollectionView.framework` into your Xcode project `Embedded Binaries`

### CocoaPods

Add this to your [Podfile](https://guides.cocoapods.org/syntax/podfile.html)

```ruby
pod 'CollectionNode'
```


### important

If you plan to upload your app you must follow additional instructions on Carthage's [README](https://github.com/Carthage/Carthage/blob/master/README.md) on adding frameworks to your application if you're building for iOS, tvOS, or watchOS.

## usage

1. Import  ```CollectionNode``` module on your  ```CollectionNodeScene```  class:

```swift
import CollectionNode
```

2. Add a ```CollectionNode``` to ```CollectionNodeScene``` and set it's dataSource and Delegate:

```swift
private var myCollectionNode: CollectionNode!

override func didMove(to view: SKView) {
    myCollectionNode = CollectionNode(at: view)

    myCollectionNode.dataSource = self
    myCollectionNode.delegate = self

    addChild(myCollectionNode)
}
```

3. Conform this ```CollectionNodeScene``` to ```CollectionNodeDataSource``` and implement all it's methods:
```swift
extension GameScene: CollectionNodeDataSource {
    func numberOfItems() -> Int {
        return EmojiModel.default.emojis.count
    }

    func collectionNode(_ collection: CollectionNode, itemFor index: Index) -> CollectionNodeItem {
        //create and configure items
        let item = EmojiItem()
        item.emoji = EmojiModel.default.emojis[index]
        return item
    }
}
```
4. Conform to ```CollectionNodeDelegate```and override the methods that you need:
```swift
extension GameScene: CollectionNodeDelegate {

     func collectionNode(_ collectionNode: CollectionNode, didShowItemAt index: Index) {
        let growAction = SKAction.scale(to: 1.3, duration: 0.15)
        let shrinkAction = SKAction.scale(to: 1, duration: 0.15)

        collectionNode.item(at: index).run(growAction)
        collectionNode.children.filter{ emojiCollection.children.index(of: $0) != index }.forEach{ $0.run(shrinkAction) }
    }

    func collectionNode(_ collectionNode: CollectionNode, didSelectItem item: CollectionNodeItem, at index: Index) {
        print("selected \(item.name ?? "noNameItem") at index \(index)")
    }
}
```

5. Update your ```CollectionNode``` with the scene:

```swift
override func update(_ currentTime: TimeInterval) {
    collectionNode.update(currentTime)
}
```

6. Now ```CollectionNode```will work with it's default implementation.

### Properties

```swift
private(set) public var index: Int
```
the current index of the CollectionNode

```swift
public weak var dataSource: CollectionNodeDataSource?
```
the object that acts as data source for the collection view

```swift
public weak var delegate: CollectionNodeDelegate?
```
the object that acts as delegate for the collection view

```swift
public var spaceBetweenItems: CGFloat
```
the spacing between elements of the CollectionNode

```swift
public var items: [CollectionNodeItem]
```
returns all the children of this node that are CollectionNodeItems

### Methods

```swift
public func update(_ currentTime: TimeInterval, dampingRatio: Double)
```
To be called on the scene's update. Allows this node to animate when touch is released
dampingRatio: the ratio for the collectionNode deacceleration (0 to 1 meaning the percentage of speed to deaccelerate when touch is released, default is 1%)

```swift
public func snap(to index: Index, withDuration duration: Double)
```
snaps to an item at a given index
duration: The duration of the snap animation in seconds (default is 0.3)

```swift
public func reloadData()
```
reloads all the items in the collection


### CollectionNodeDelegate

```swift
func collectionNode(_ collectionNode: CollectionNode, didShowItemAt index: Index) -> Void
```
returns the number of items to be displayed on this collectionNode


```swift
func collectionNode(_ collectionNode: CollectionNode, didSelectItem item: CollectionNodeItem, at index: Index ) -> Void
```
called each time an item is selected

### CollectionNodeDataSource

```swift
func numberOfItems() -> Int
```
here you should tell the number of items this collection will display


```swift
func collectionNode(_ collection: CollectionNode, itemFor index: Index) -> CollectionNodeItem
```
here you should return an item for each index in the collectionVIew

## Apps using CollectionNode

* [Tiles](https://itunes.apple.com/br/app/tiles-puzzle-game/id1253612564?mt=8)

![Preview](https://github.com/bwide/CollectionNode/blob/master/tiles.gif)

Show me your apps! if you have used this collection i'd love to see it, reach me in ```bfwide07@gmail.com``` you can send me images and i will post them here.
