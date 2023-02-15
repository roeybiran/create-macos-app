# create-xcode-project

1. Delete the `MainMenu.xib` or `Storyboard.main` files.
2. Respectively, delete the `NSMainNibFile` or `NSMainStoryboardFile` keys in the project’s `Info.plist`.
3. Add a `-NSConstraintBasedLayoutVisualizeMutuallyExclusiveConstraints YES` launch argument.
4. Remove the `@main` attribute from the `AppDelegate` class.
5. Create a `main.swift` file, and add the code snippet below. Aside from bootstrapping the app, it checks whether the app is run within Xcode’s test environment. If so, the standard `AppDelegate` isn’t initialized. This speeds up the unit tests significantly (see [this](https://stackoverflow.com/questions/39116318/swift-how-to-not-load-appdelegate-during-tests) and [this](https://www.oakcity.io/replacing-the-app-delegate-for-unit-tests-on-the-mac/)).  

```swift
import Cocoa

let app = NSApplication.shared

if NSClassFromString("XCTestCase") != nil {
	app.run()
} else {
	let appDelegate = AppDelegate()
	app.delegate = appDelegate
	_ = NSApplicationMain(CommandLine.argc, CommandLine.unsafeArgv)
}
```

6. Create a menu bar (an instance of `NSMenu`) and assign it to the `mainMenu` property of the shared `NSApplication` instance.