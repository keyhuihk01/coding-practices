# Coding Practices in iOS Development

## Summary

* [Cocoapods](#cocoapods)
* [Project Structure](#project-structure)
* [Swift](#swift)
* [Log](#log)
* [Unit Test](#unit-test)

- Other Reference: raywenderlich(https://github.com/raywenderlich/swift-style-guide)

----------

### Cocoapods

**Use Cocoapods, less pain in development**

- We tried to use `Carthage`, but stuck in Archive Enterprise and Ad-Hoc Build in XCode 11


#### Cocoapods (https://guides.cocoapods.org/using/getting-started.html) Installation
```bash
sudo gem install cocoapods
```


### Project Structure

**Keep the project tree Sort by Name and Type**

- 1. `Right Click the folder`

- 2. `Sort by Type`

- 3. `Sort by Name`


**MVVM**

#### Main Target - using RxSwift (https://github.com/ReactiveX/RxSwift)

	├─ Core
	  ├─ Manager Class
    ├─ Data
	├─ Extensions
	├─ Screens
	  ├─ Home
	    ├─ ViewController
	    ├─ ViewModel
	├─ Protocol
	├─ Utils
	├─ AppDelegate.swift

#### UIKit - using SnapKit (https://github.com/SnapKit/SnapKit)

    ├─ Extensions
	├─ Fundamentals (Base & Constants Class)
	├─ Screens (Big Section Screen)
	  ├─ Login
	  ├─ Home
	├─ UI (Custom, Shared View)
	├─ xib


#### APIService

    ├─ Models
	├─ Requests
	├─ Responses


#### ResourceKit - using R.swift (https://github.com/mac-cain13/R.swift)

    ├─ Assets.xcassets
	├─ Colors.xcassets
	├─ Localizable.strings
	├─ R.generated.swift


### Swift

#### Class Initialization

**Good**
```swift
class SomeViewController: UIViewController {

	var someValue: Int?

	static func getInstance(value: Int?) -> SomeViewController { 
		let vc = SomeViewController()
		vc.someValue = value
		return vc
	}
}

// Usage
let vc = SomeViewController.getInstance(value: 777)
```

**Bad, native way but not friendly for coding**
```swift
class SomeViewController: UIViewController {

	let someValue: Int

	init(someValue: Int) {
        self.someValue = someValue
        super.init()
    }

    required init() {
        fatalError("init() has not been implemented")
    }

    required init?(coder aDecoder: NSCoder) {
        fatalError("init(coder:) has not been implemented")
    }
}

// Usage
let vc = SomeViewController(someValue: 777)
```

#### Protocol

**Good**
```swift
class MyViewController: UIViewController {
  // class stuff here
}

// MARK: - UITableViewDataSource
extension MyViewController: UITableViewDataSource {
  // table view data source methods
}

// MARK: - UIScrollViewDelegate
extension MyViewController: UIScrollViewDelegate {
  // scroll view delegate methods
}
```

**Bad**
```swift
class MyViewController: UIViewController, UITableViewDataSource, UIScrollViewDelegate {
  // all methods
}
```

#### Minimal Imports

**Good**
```swift
import UIKit
var view: UIView
var deviceModels: [String]
```

**Bad**
```swift
import UIKit
import Foundation
var view: UIView
var deviceModels: [String]
```


### Log

**Don't use print("hello") directly**

Instead, using `LoggerKit`

```
logger.d("hello")
```


### Unit Test

[WIP]
