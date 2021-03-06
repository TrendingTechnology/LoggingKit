# LoggingKit

<p align="center">
   <img width="750" src="https://raw.githubusercontent.com/alexanderwe/LoggingKit/master/assets/loggingkit_logo.png" alt="SwiftKit Header Logo">
</p>

<p align="center">
   <a href="https://developer.apple.com/swift/">
      <img src="https://img.shields.io/badge/Swift-5.0-orange.svg?style=flat" alt="Swift 5.0">
   </a>
   <a href="https://github.com/apple/swift-package-manager">
      <img src="https://img.shields.io/badge/Swift%20Package%20Manager-compatible-brightgreen.svg" alt="SPM">
   </a>

   <a href="https://github.com/alexanderwe/LoggingKit">
      <img src="https://github.com/alexanderwe/LoggingKit/workflows/CI/badge.svg" alt="CI">
   </a>   
</p>

<p align="center">
LoggingKit is a micro framework for logging based on log providers 
</p>

## Features

- [x] Define your own log providers
- [x] `Combine` ready
- [x] Comes with pre-defined `OSLogProvider` which uses `os_log` under the hood

## Example

The example application is the best way to see `LoggingKit` in action. Simply open the `LoggingKit.xcodeproj` and run the `Example` scheme.

After the application has started you should see several log messages in your Xcode terminal and the `Console.app` for the device you ran the app on.

## Installation

### Swift Package Manager

To integrate using Apple's [Swift Package Manager](https://swift.org/package-manager/), add the following as a dependency to your `Package.swift`:

```swift
dependencies: [
    .package(url: "https://github.com/alexanderwe/LoggingKit.git", from: "2.0.0")
]
```

Alternatively navigate to your Xcode project, select `Swift Packages` and click the `+` icon to search for `LoggingKit`.

### Manually

If you prefer not to use any of the aforementioned dependency managers, you can integrate LoggingKit into your project manually. Simply drag the `Sources` Folder into your Xcode project.

## Usage

At first it makes sense to create an extensions on `LogCategories` to define your own categories.

```swift
import LoggingKit

extension LogCategories {
    public var viewControllers: LogCategory { return .init("viewControllers") }
    public var networking: LogCategory { return .init("networking") }
    ...
}
```

Then register your log providers in the `application(application:didFinishLaunchingWithOptions:)`.

```swift
import LoggingKit

class AppDelegate: UIResponder, UIApplicationDelegate {

    ...

    func application(_ application: UIApplication, didFinishLaunchingWithOptions launchOptions:[UIApplication.LaunchOptionsKey: Any]?) -> Bool {

        LogService.register(logProviders: LogProvider, LogProvider ...)

    }

    ...
}
```

After that Simply import `LoggingKit` in the files you want to use the logging methods and use them accordingly

```swift
import LoggingKit

LogService.shared.debug("Hello Debug", logCategory: \.viewControllers)
LogService.shared.verbose("Hello Verbose", logCategory: \.viewControllers)
LogService.shared.info("Hello Info", logCategory: \.viewControllers)
LogService.shared.warning("Hello Warning", logCategory: \.viewControllers)
LogService.shared.error("Hello Error", logCategory: \.viewControllers)

```

### Combine

If you are using combine, `LoggingKit` offers some extensions on the `Publisher` type to log `Self.Output` and `Self.Failure`.

You can choose whichever category you want. The `\.combine` category is a custom defined one.

```swift
import LoggingKit

// logs `Self.Output`
myPublisher.logValue(logType: .info, logCategory: \.combine) {
    "My Value is \($0)"
}

// logs `Self.Failure`
myPublisher.logError(logCategory: \.combine) {
    "My Error is \($0)"
}

// logs `Self.Output` as well as `Self.Failure`
myPublisher.log()
```

### Providers

The idea behind this small framework is, that you can extend it by writing your own log providers by conforming to the `LogProvider` protocol. These implementations then can be registered in the `LogService.register(providers:)` method.

You can find an example `LogProvider` implementation in [./Example/MyTestLogProvider.swift](./Example/MyTestLogProvider.swift)

#### OSLogProvider

LoggingKit comes with one pre-defined `OSLogProvider` . It uses `os_log` under the hood to log your messages. These messages can then be viewed in the `Console.app` application of your mac and on the console in Xcode.

##### Console App

Open `Console.App` on your mac, select the device from which you want to view the log messages, to view the messages printed by the `OSLogProvider`

![Console App Screenshot](./assets/console_screenshot.png)

## Contributing

Contributions are very welcome 🙌

## License

```
LoggingKit
Copyright (c) 2020 Alexander Weiß

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in
all copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN
THE SOFTWARE.
```
