# Wistory 

## Getting started

### Cocoapods

Add sources to Podfile

```
source 'https://github.com/Wistory/Specs.git'
source 'https://github.com/CocoaPods/Specs.git'

```

And set last avaliable version for target

```
  pod 'Wistory', '~> 0.3.0'
  
```

Run

```
pod install --repo-update
```

### Carthage

Now instalation supports only Carthage as dependency manager. 
To add libaray add `git "https://gitlab.com/Wistory/Wistory" ~> 0.3.0` to Cartfile with latest actual version and install as usual.

## Initialization
Create instance of class:

```swift
let instance = Wistory()
``` 

Don't forget to provide given company token in initialization. Otherwise Wistory will be inited with demo token
Also you can provide own user id for store favourites stories. By default it provided by us, but not works at 100% cases:

```swift
UIDevice.current.identifierForVendor?.uuidString
```

### Examples of initialization  

```swift
let instance = Wistory(token: String)
```
or
```swift
let instance = Wistory()
.with(token: String)
```
or
```swift
let instance = Wistory()
instance.token = token
```

Wistory support chaining init:
```swift
Wistory()
  .with(userToken: "token")
  .with(companyToken: "company")
  .presentingSettings(style: .fullscreen)
  .storiesViewController
```

After this framework ready to use

### Settings

- `presentStyle`

Choose how stories will be presented. Can be `fullscreen` or `popover`

- `fullscreen`

Option for user stories root collection as embdeed in any view. Bool
Example of usage:

```swift
  addChild(wistory)
  wistory.view.translatesAutoresizingMaskIntoConstraints = false
  self.embdedView.addSubview(wistory.view)

  NSLayoutConstraint.activate([
    wistory.view.leadingAnchor.constraint(equalTo: embdedView.leadingAnchor),
    wistory.view.trailingAnchor.constraint(equalTo: embdedView.trailingAnchor),
    wistory.view.topAnchor.constraint(equalTo: embdedView.topAnchor),
    wistory.view.bottomAnchor.constraint(equalTo: embdedView.bottomAnchor)
  ])

  wistory.didMove(toParent: self)
```

## Delegate
Wistory provides handlers for most of common cases. To implement adopt `delegate` by your class

And confirm to `WistoryEventsDelegate` protocol

### Delegate methods

- `func onItemsLoaded()`

Called if the stories are successfully downloaded from the server

- `func onRead(story: SnapModel)`

It's called when story was readen. Passes the story object

- `func onPrevSnap(story: SnapModel)`

Called when switching to the previous "snap". Passes the story object

- `func onNextSnap(story: SnapModel)`

Called when switching to the next "snap". Passes the story object

- `func onNavigate(action: String, value: String)`

Called up by pressing the action button. Passes the action type and value

- `func onFavorite(id: String, isFavorite: Bool)`

Called up by clicking the add to favorites button. Passes id of story and value

- `func onRelation(id: String, relation: String)`

Called up by pressing the like or dislike button. Passes id of story and value

- `func onPoll(id: String, snap: Int, option: String)`

Called up after the answer choice is made in the voting. Passes the story, "snap" position and the answer choice id

- `func onError(error: Error)`

Called on any error. Mostly by network layer

## Usage

Currently supported only one way to initialize framework. You need to get instance of our root view controller by calling `storiesViewController` property and then set to your navigation stack. **Wistory uses own navigation flow, take it in mind**

Example:

```swift
let rootViewController = Wistory().storiesViewController
self.present(rootViewController, animated: true, completion: nil)
```
