# Coding Practices in iOS Development

## Summary

* [Cocoapods](#cocoapods)
* [Project Structure](#project-structure)
* [Swift](#swift)
* [ViewController](#viewcontroller)
* [Log](#log)
* [Unit Test](#unit-test)

- Other Reference: raywenderlich (https://github.com/raywenderlich/swift-style-guide)

----------

### Cocoapods

**Use Cocoapods, less pain in development**

- We tried to use `Carthage`, but stuck in Archive Enterprise and Ad-Hoc Build in XCode 11


#### Cocoapods (https://guides.cocoapods.org/using/getting-started.html) Installation
```bash
sudo gem install cocoapods
pod install
```


### Project Structure

**Keep the project tree Sort by Name and Type**

1. `Right Click the folder`

2. `Sort by Type`

3. `Sort by Name`


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

**Native way but not friendly for coding**
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

#### Function Declarations

**Good**
```swift
func updateConstraints() -> Void {
  // magic happens here
}

typealias CompletionHandler = (result) -> Void
```

**Bad**
```swift
func updateConstraints() -> () {
  // magic happens here
}

typealias CompletionHandler = (result) -> ()
```

#### Types - Always use Swift's native types and expressions when available.

**Good**
```swift
let width = 120.0                                    // Double
let widthString = "\(width)"                         // String
```

**Bad**
```swift
let width: Double = 120.0                            // Double
let widthString = (width as NSNumber).stringValue    // String
```

#### Static Constants

**Good**
```swift
static let kName = "MyString"
static let kWidth = 120.0
```

**Bad**
```swift
static let NAME = "MyString"
static let witdh = 120.0
```

#### Weak Reference, use optional instead of guard if possible

**Good**
```swift
typedView.btnMore.rx.tap
	.observeOn(MainScheduler.instance)
	.subscribe(onNext: { [weak self] _ in
		self?.navigationController?.pushViewController(TargetViewController.getInstance(), animated: true)
		}).disposed(by: disposeBag)
```

**OK, more than one nil checking**
```swift
typedView.btnMore.rx.tap
	.observeOn(MainScheduler.instance)
	.subscribe(onNext: { [weak self] item in
		guard let _self = self else { return }
		_self.someValue = _self.someValue + item.value
		_self.navigationController?.pushViewController(TargetViewController.getInstance(), animated: true)
		}).disposed(by: disposeBag)
```

**Bad**
```swift
typedView.btnMore.rx.tap
	.observeOn(MainScheduler.instance)
	.subscribe(onNext: { [weak self] _ in
		guard let _self = self else { return }
		_self.navigationController?.pushViewController(TargetViewController.getInstance(), animated: true)
		}).disposed(by: disposeBag)
```


#### Use of Closure Parameter `$0` in SnapKit or Single Parameter Closure

**Good**
```swift
view.snp.makeConstraints {
	$0.width.equalToSuperview()
	$0.height.equalToSuperview()
}
```

**Not Good, need more code**
```swift
view.snp.makeConstraints { (make) in
	make.width.equalToSuperview()
	make.height.equalToSuperview()
}
```

#### Constants, Swift does not prefer all capital letters

**Good**
```swift
struct Constants {
    static let transitionTime: TimeInterval = 0.5
}
```

**Bad**
```swift
struct Constants {
    static let kTransitionTime: TimeInterval = 0.5
    static let TRANSITION_TIME: TimeInterval = 0.5
}
```

#### Enum

**Good**
```swift
enum DataKey: String {
    case isFirstLaunch = "is_first_launch"
}```

**Bad**
```swift
enum DataKey: String {
    case Is_First_Launch = "is_first_launch"
}
```

**Bad**
```swift
enum DataKey: String {
    case is_first_launch = "is_first_launch"
}
```

### ViewController

#### Class Structure for MVVM

```swift
class WelcomeViewController: BaseViewController, TypedViewControllerProtocol {
    typealias ViewType = WelcomeView
    let vm = WelcomeVM()

    override func viewDidLoad() {
        super.viewDidLoad()
        view = createTypedView()

        // other stuff...
    }
}
```


### Log

**Don't use print("hello") directly**

Instead, using `LoggerKit`

```
logger.d("hello")
```


### Unit Test

[WIP]
