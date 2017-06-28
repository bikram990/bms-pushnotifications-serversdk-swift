# BluemixPushNotifications

[![Swift][swift-badge]][swift-url]
[![Platform][platform-badge]][platform-url]
[![Build Status](https://travis-ci.org/ibm-bluemix-mobile-services/bms-pushnotifications-serversdk-swift.svg)](https://travis-ci.org/ibm-bluemix-mobile-services/bms-pushnotifications-serversdk-swift)
[![Codacy Badge](https://api.codacy.com/project/badge/Grade/51d26cb37b4d474887087d455a311a43)](https://www.codacy.com/app/ibm-bluemix-mobile-services/bms-pushnotifications-serversdk-swift?utm_source=github.com&amp;utm_medium=referral&amp;utm_content=ibm-bluemix-mobile-services/bms-pushnotifications-serversdk-swift&amp;utm_campaign=Badge_Grade)
[![Coverage Status](https://coveralls.io/repos/github/ibm-bluemix-mobile-services/bms-pushnotifications-serversdk-swift/badge.svg?branch=development)](https://coveralls.io/github/ibm-bluemix-mobile-services/bms-pushnotifications-serversdk-swift?branch=development)

## Summary

BluemixPushNotifications is a Swift server-side SDK for sending push notifications via Bluemix Push Notifications services.

## Installation

#### Swift Package Manager

```swift
import PackageDescription

let package = Package(
    dependencies: [
        .Package(url: "https://github.com/ibm-bluemix-mobile-services/bluemix-pushnotifications-swift-sdk.git", majorVersion: 0, minor: 6)
	]
)
```

#### Build on Linux

```bash
sudo apt-get update // not required on Mac
swift build -Xcc -fblocks -Xlinker -rpath -Xlinker $(pwd)/.build/debug/
```

## Releases
* 0.6.x releases were tested on OSX and Linux with Swift 3.1
* 0.5.x releases were tested on OSX and Linux with Swift 3.1
* 0.4.x releases were tested on OSX and Linux with Swift 3.0.1
* 0.4.x releases were tested on OSX and Linux with Swift DEVELOPMENT-SNAPSHOT-2016-09-07-a
* 0.3.x releases were tested on OSX and Linux with Swift DEVELOPMENT-SNAPSHOT-2016-06-20-a
* 0.2.x releases were tested on OSX and Linux with Swift DEVELOPMENT-SNAPSHOT-2016-06-06-a
* 0.1.x releases were tested on OSX and Linux with Swift DEVELOPMENT-SNAPSHOT-2016-05-03-a

## Usage

Import the BluemixPushNotifications framework.

```swift
import BluemixPushNotifications
```

Initialize with details about your Bluemix Push Notifications service.

```swift
let myPushNotifications = PushNotifications(bluemixRegion: PushNotifications.Region.US_SOUTH, bluemixAppGuid: "your-bluemix-app-guid", bluemixAppSecret: "your-push-service-appSecret")
```

Create a simple push notification that will broadcast to all devices.
```swift
let messageExample = Notification.Message(alert: "Testing BluemixPushNotifications")
let notificationExample = Notification(message: messageExample)
```
Send the Push notification in following way,

```swift
myPushNotifications.send(notification: notificationExample) { (error) in
  if error != nil {
    print("Failed to send push notification. Error: \(error!)")
  }
}
```


To create a more selective push notification with specified settings that only gets sent to certain devices either by deviceIds or by userIds of users that own the devices or by device platforms or based on tag-subscriptions, or to set GCM and APNs features - there are optional parameters that you can use in the corresponding intializers.

#### Targets

 In `Targets` you pass the following values,

  * deviceIds - Array of Devices IDs
  * userIds - Array of user IDs
  * platforms - Array of platforms
  * tagNames - Array of tag names.

  ```swift
  let targetExample = Notification.Target(deviceIds: ["device1","device2"],
         userIds: ["userId1", "userId2"],
         platforms: [TargetPlatform.Apple,TargetPlatform.Google,TargetPlatform.ChromeExtApp,
         TargetPlatform.WebChrome,TargetPlatform.webFirefox,TargetPlatform.WebSafari], tagNames: ["tag1", "tag2"])
  ```
>**Note**: Do not use userIds, tagNames and deviceIds together.

#### Settings

Settings can contain any of the following types,

 * Apns
 * Gcm
 * FirefoxWeb
 * ChromeWeb
 * SafariWeb
 * ChromeAppExt

 ```swift
 let notificationExample = Notification(message: messageExample,
                apnsSettings: nil, gcmSettings: nil, firefoxWebSettings: nil,
                chromeWebSettings: nil, safariWebSettings: nil, chromeAppExtSettings: nil)
 ```

##### Apns

APNs settings can have the following parameters,

 * badge - The number to display as the badge of the application icon
 * interactiveCategory - The category identifier to be used for the interactive push notifications
 * iosActionKey - The title for the Action key
 * sound - The name of the sound file in the application bundle. The sound of this file is played as an alert.
 * type - Notification type: DEFAULT, MIXED or SILENT
 * payload -  Custom JSON payload that will be sent as part of the notification message
 * titleLocKey -  The key to a title string in the Localizable.strings file for the current localization. The key string can be formatted with %@ and %n$@ specifiers to take the variables specified in the titleLocArgs arra
 * locKey - A key to an alert-message string in a Localizable.strings file for the current localization (which is set by the user’s language preference). The key string can be formatted with %@ and %n$@ specifiers to take the variables specified in the locArgs array
 * launchImage - The filename of an image file in the app bundle, with or without the filename extension. The image is used as the launch image when users tap the action button or move the action slider
 * titleLocArgs - Variable string values to appear in place of the format specifiers in title-loc-key
 * locArgs - Variable string values to appear in place of the format specifiers in locKey.
 * title - The title of Rich Push notifications (Supported only on iOS 10 and above)
 * subtitle - The subtitle of the Rich Notifications. (Supported only on iOS 10 and above)
 * attachmentUrl - The link to the iOS notifications media (video, audio, GIF, images - Supported only on iOS 10 and above) ,


```swift
let apnsSetting = Notification.Settings.Apns(badge: 1, interactiveCategory: "Category",
              iosActionKey: "VIEW", sound: "Newtune.wav", type: ApnsType.DEFAULT,
              payload: ["key1":"value1"], titleLocKey: "TITLE1", locKey: "LOCKEY1",
              launchImage: "launchImage1.png", titleLocArgs: ["arg1","arg2"],
              locArgs: ["arg3","arg4"], title: "twelcom to Bluemix Push service",
              subtitle: "Push Notifications", attachmentUrl: "https://bluemix.net/image.png")
```

##### Gcm

GCM settings can have the following parameters,

* collapseKey -  This parameter identifies a group of messages
* delayWhileIdle - When this parameter is set to true, it indicates that the message should not be sent until the device becomes active
* payload -  Custom JSON payload that will be sent as part of the notification message
* priority - A string value that indicates the priority of this notification. Allowed values are 'max', 'high', 'default', 'low' and 'min'. High/Max priority notifications along with 'sound' field may be used for Heads up notification in Android 5.0 or higher.sampleval='low'.
* sound - The sound file (on device) that will be attempted to play when the notification arrives on the device
* timeToLive - This parameter specifies how long (in seconds) the message should be kept in GCM storage if the device is offline
* interactiveCategory - The category identifier to be used for the interactive push notifications.
* icon - Specify the name of the icon to be displayed for the notification. Make sure the icon is already packaged with the client application
* sync - Device group messaging makes it possible for every app instance in a group to reflect the latest messaging state ,
* visibility - private/public - Visibility of this notification, which affects how and when the notifications are revealed on a secure locked screen
* lights - Allows setting the notification LED color on receiving push notification
* style - Options to specify for Android expandable notifications. The types of expandable notifications are picture_notification, bigtext_notification, inbox_notification

```swift
let lights = Notification.Settings.GcmLights(ledArgb: GcmLED.Green, ledOnMs: 3, ledOffMs: 3)

let style = Notification.Settings.GcmStyle(type: GcmStyleTypes.inbox_notification,
              title: "inbox notification", url: "https://bluemix.net/image.png",
              text: "some big text", lines: ["line 1","line 2"])

let gcmSettings = Notification.Settings.Gcm(collapseKey: "collapseKey1", delayWhileIdle: false,
                        payload: ["key1":"value1"], priority: GcmPriority.DEFAULT,
                        sound: "sound.wav", timeToLive: 2.0,
                        interactiveCategory: "category1", icon: "icon.png",
                        sync: false, visibility: GcmVisibility.Public,
                        lights: lights, style: style)
```

##### FirefoxWeb

FirefoxWeb settings can have the following parameters,

* title - Specifies the title to be set for the WebPush Notification
* iconUrl - The URL of the icon to be set for the WebPush Notification
* payload - Custom JSON payload that will be sent as part of the notification message.
* timeToLive - This parameter specifies how long (in seconds) the message should be kept in GCM storage if the device is offline.

```swift
let firefoxSetttings = Notification.Settings.FirefoxWeb(title: "Bluemix Push Notifications",
                              iconUrl: "https://bluemix.net/icon.png",
                              payload: ["key1":"value1"], timeToLive: 3.0)
```

##### ChromeWeb

ChromeWeb settings can have the following parameters,

* title - Specifies the title to be set for the WebPush Notification
* iconUrl - The URL of the icon to be set for the WebPush Notification
* payload - Custom JSON payload that will be sent as part of the notification message.
* timeToLive - This parameter specifies how long (in seconds) the message should be kept in GCM storage if the device is offline.

```swift
let chromeSetttings = Notification.Settings.ChromeWeb(title: "Bluemix Push Notifications",
                              iconUrl: "https://bluemix.net/icon.png",
                              payload: ["key1":"value1"], timeToLive: 3.0)
```

##### SafariWeb

SafariWeb settings can have the following parameters,

* title - Specifies the title to be set for the Safari Push Notifications
* urlArgs - The URL arguments that need to be used with this notification. This has to provided in the form of a JSON Array.
* action - The label of the action button.

```swift
let safariSettings = Notification.Settings.SafariWeb(title: "Bluemix Push Notifications",
                          urlArgs: ["https://bluemix.net"], action: "View")
```


##### ChromeAppExt

ChromeAppExt settings can have the following parameters,

* collapseKey - This parameter identifies a group of messages.
* delayWhileIdle - When this parameter is set to true, it indicates that the message should not be sent until the device becomes active.
* title - Specifies the title to be set for the WebPush Notification.
* iconUrl - The URL of the icon to be set for the WebPush Notification.
* timeToLive - This parameter specifies how long (in seconds) the message should be kept in GCM storage if the device is offline.
* payload - Custom JSON payload that will be sent as part of the notification message.

```swift
let chromeAppExtSettings = Notification.Settings.ChromeAppExt(title: "Bluemix Push Notifications", iconUrl: "https://bluemix.net/icon.png", collapseKey: "collapseKey1", delayWhileIdle: false, payload: ["key1":"value1"], timeToLive: 4.0)
```


## License

Copyright 2017 IBM Corp.

Licensed under the Apache License, Version 2.0 (the "License");
you may not use this file except in compliance with the License.
You may obtain a copy of the License at

http://www.apache.org/licenses/LICENSE-2.0

Unless required by applicable law or agreed to in writing, software
distributed under the License is distributed on an "AS IS" BASIS,
WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
See the License for the specific language governing permissions and
limitations under the License.

[swift-badge]: https://img.shields.io/badge/Swift-3.0-orange.svg
[swift-url]: https://swift.org
[platform-badge]: https://img.shields.io/badge/Platforms-OS%20X%20--%20Linux-lightgray.svg
[platform-url]: https://swift.org
