---
title: Swift Setup Guide
tags:
  - Setup
  - iOS
  - macOS
  - watchOS
  - tvOS
featured: true
testedOn: Xcode 14.1 & Swift 5.5
description: Configure the TelemetryDeck SDK in Your Swift Application for iOS, macOS, watchOS and tvOS
lead: Let's include the TelemetryDeck Swift Package in your application and send signals!
order: 100
---

## Prerequisites

This guide assumes you have already created a TelemetryDeck account. If you haven't yet, please [sign up now](https://dashboard.telemetrydeck.com/registration/organization)!

## Installing the Swift Package

The TelemetryDeck Swift package uses Swift Package Manager.

1. Open Xcode and navigate to the project you want to add TelemetryDeck to.
1. In the menu, select <kbd>File</kbd> -> <kbd>Add Packages...</kbd>. This will open the Swift Package Manager view.
1. Paste `https://github.com/TelemetryDeck/SwiftSDK` into the search field.
1. Select the `SwiftSDK` package that appears in the list
1. Set the <kbd>Dependency Rule</kbd> to <kbd>Up to Next Major Version</kbd>.
1. Click <kbd>Add Package</kbd>.

![A screenshot of Xcode adding the TelemetryDeck Package](/docs/images/xcode-swift-package.png)

This will include the TelemetryDeck Swift Client into your app by downloading the source code. Feel free to browse the client's source code, it's tiny and you'll see for yourself how TelemetryDeck is hashing user identifiers before they ever reach the server. Privacy, yay!

## Including the package in your target

Xcode will ask you to link the package with your target in the next screen, titles <kbd>Choose Package Products for SwiftSDK</kbd>. Select the `TelemetryDeck` library and click <kbd>Add Package</kbd>.

{% noteinfo "Link Library with more than one Target" %}

In case Xcode forgets to ask you to link the library with your target, you can do so manually by selecting your target in the project navigator and selecting the <kbd>Build Phases</kbd> tab. Click the <kbd>+</kbd> button in the <kbd>Link Binary With Libraries</kbd> section and select the `TelemetryDeck` library.
{% endnoteinfo %}

## Initializing the TelemetryDeck Swift Package

The `TelemetryDeck` package will provide you with a class `TelemetryDeck` that you'll use for all interactions with TelemetryDeck. Before you can use that class, you'll need to initialize it at the start of your app. We strongly recommend doing so as soon as possible, as you won't be able to send Signals before the `TelemetryDeck` is initialized.

This is slightly different depending on whether you use SwiftUI or UIKit's `AppDelegate` to manage your app's lifecycle, so let's look at these individually.

{% noteinfo "You need your app's Unique Identifier" %}
TelemetryDeck assigns a unique identifier to your app, and you need to hand that identifier to the TelemetryDeck SDK.

Use the [TelemetryDeck Dashboard](https://dashboard.telemetrydeck.com) to create a new app and copy its unique identifier into your computer's clipboard.
{% endnoteinfo %}

You only need **one** way of initializing the TelemetryDeck SDK, either SwiftUI/SceneKit or AppDelegate. If your app is new, you'll likely want to use SwiftUI/SceneKit.

### Initialization with SwiftUI

For Scene-based SwiftUI applications, we recommend adding the initialization to your `@main` App struct! Open `YourAppNameApp.swift` and look for code that looks like this:

```swift
import SwiftUI

@main
struct Example_AppApp: App {
    var body: some Scene {
        WindowGroup {
            ContentView()
        }
    }
}
```

This is the entry point to your app. Now let's add the initialization here.

Import the TelemetryDeck Package by adding `import TelemetryDeck`. Then add an `init()` method to your App struct that creates a `TelemetryDeck.Config` instance and hands it to the `TelemetryDeck`, using the **Unique Identifier of your app** that you copied into your clipboard earlier. If you don't have that anymore, you can get it at any time from the TelemetryDeck Dashboard.

Your code should now look like this:

```swift
import SwiftUI
import TelemetryDeck

@main
struct Example_AppApp: App {
    var body: some Scene {
        WindowGroup {
            ContentView()
        }
    }

    init() {
        let config = TelemetryDeck.Config(appID: "YOUR-APP-UNIQUE-IDENTIFIER")
        TelemetryDeck.initialize(config: config)
    }
}
```

If you already have an `init()` method, add the two lines to its end.

Your app is now ready. You can skip the AppDelegate part if you're using SwiftUI and SceneKit.

### Initialization in an AppDelegate based app

If you use `AppDelegate` to manage your app's life cycle, open the file `AppDelegate.swift` and look for the method `application(:didFinishLaunchingWithOptions:)`. It will probably look similar to this:

```swift
import UIKit

@main
class AppDelegate: UIResponder, UIApplicationDelegate {
    func application(_ application: UIApplication, didFinishLaunchingWithOptions launchOptions: [UIApplication.LaunchOptionsKey: Any]?) -> Bool {
        // Override point for customization after application launch.
        return true
    }

    // ...
}
```

By default, Xcode even adds a comment here to tell you where to add stuff that should happen right after launch.

Now, import the `TelemetryDeck` package and configure the `TelemetryDeck` using the **Unique Identifier of your app** that you copied into your clipboard earlier. If you don't have that anymore you can get it at any time from the TelemetryDeck Dashboard.

```swift
import UIKit
import TelemetryDeck

@main
class AppDelegate: UIResponder, UIApplicationDelegate {
    func application(_ application: UIApplication, didFinishLaunchingWithOptions launchOptions: [UIApplication.LaunchOptionsKey: Any]?) -> Bool {

        let config = TelemetryDeck.Config(appID: "YOUR-APP-UNIQUE-IDENTIFIER")
        TelemetryDeck.initialize(config: config)

        return true
    }
    // ...
}
```

If you already have code in this function, add the two new lines to the end.

You are now ready to send signals!

## Sending signals

Let's send a signal to show the app has launched correctly.

{% noteinfo "What is a signal?" %}

Signals are an indication that **an event** happened in your app, which is used by a **user**. Signals consist of these parts:

- **Signal Type** – A string that indicates which kind of event happened
- **User Identifer** – A string that identifies your user
- **Metadata Payload** – A dictionary of additional data about your app or the event triggering the signal

See the [Signals Reference](/docs/api/signals-reference/) for more information about how you can effectively use Signals.
{% endnoteinfo %}

{% notewarning "When running from Xcode, you're sending testing signals" %}

If your app is built in `DEBUG` configuration (i.e. running from Xcode), your signals will be tagged as **Testing Signals**, meaning that you can easily filter them out later. You'll see them show up in the TelemetryDeck Dashboard when it is set to **Test Mode**.
{% endnotewarning %}

See the [TelemetryDeck SDK's `README.md` file](https://github.com/TelemetryDeck/SwiftSDK/blob/main/README.md) for more information on how to send signals. For now, let's just send one signal that tells us the app has launched. Go to the place where you just added the initialization, and directly below add this line:

```swift
let config = TelemetryDeck.Config(appID: "YOUR-APP-UNIQUE-IDENTIFIER")
TelemetryDeck.initialize(config: config)

TelemetryDeck.signal("App.didFinishLaunching")
```

And done. This is all you need to send a signal. You do not need to keep an instance of TelemetryDeck and hand it around, just call the `send` function on the class directly. If you want to add a custom user identifer or metadata payload, add them to the function call like this:

```swift
TelemetryDeck.signal(
    "App.didFinishLaunching",
    parameters: [
        "numberOfTimesPizzaModeHasActivated": "\(dataStore.pizzaMode.count)",
        "pizzaCheeseMode": "\(dataStore.pizzaCheeseMode)"
    ],
    customUserID: "my very cool user"
)
```

And you're done! You are now sending signals to the TelemetryDeck server.

## Fill out Apple's app privacy details

Last thing you need to do before you can send signals is going through Apple's privacy details. This informs your users about what data is collected, and how it is collected.

Also TelemetryDeck is privacy friendly, and we only handle not personally identifiable information, you still need to click through the privacy details.

We have a [handy guide](/docs/articles/apple-app-privacy/) where we go over each step that is required.

## Privacy Policy and Opt-Out

You don't need to update your privacy policy, [but we recommend you do it anyway](/docs/guides/privacy-faq/#do-i-need-to-add-telemetrydeck-to-my-privacy-policy%3F).

## You're all set!

You can now send signals! Don't overdo it in the beginning. It's okay if you only send **one** signal, named `applicationDidFinishLaunching` in the beginning. This will already give you number of users, number of launches, retention… a lot!

After a while, you can add a send call for each screen in your app, so you can see which screens your users use most. It's also recommended to add all your custom settings to your metadata each time (except the ones that might identify an individual user please). This way you can see which settings most of your users use.
