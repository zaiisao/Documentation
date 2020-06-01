
---
title: Start a Voice Call
description: 
platform: iOS
updatedAt: Tue Apr 28 2020 13:24:27 GMT+0800 (CST)
---
# Start a Voice Call
Use this guide to quickly start a basic voice call with the Agora Voice SDK for iOS.

## Sample project

We provide an open-source [Agora-iOS-Voice-Tutorial-Swift-1to1](https://github.com/AgoraIO/Basic-Audio-Call/tree/master/One-to-One-Voice/Agora-iOS-Voice-Tutorial-Swift-1to1) sample project that implements the basic one-to-one voice call on GitHub. You can try the demo and view the source code.

## Prerequisites

- Xcode 9.0 or later
- An iOS device running iOS 8.0 or later
- A valid Agora account. ([Sign up](https://sso.agora.io/en/signup) for free)

<div class="alert note">Open the specified ports in <a href="https://docs.agora.io/en/Agora%20Platform/firewall?platform=All%20Platforms">Firewall Requirements</a > if your network has a firewall.</div>

## Set up the development environment

In this section, we will create an iOS project, and integrate the SDK into the project.

### Create an iOS project
Now, let's build an iOS project from scratch. Skip to [Integrate the SDK](#IntegrateSDK) if an iOS project already exists.

<details>
	<summary><font color="#3ab7f8">Create an iOS project</font></summary>

1. Open **Xcode** and click **Create a new Xcode project**.
2. Choose **Single View App** as the template and click **Next**.
3. Input the project information, such as the project name, team, organization name, and language, and click **Next**.
	
  **Note**: If you haven't added any team information, you will see an **Add account...** button. Click it, input your Apple ID, and click **Next** to add your team.
4. Choose the storage path of the project and click **Create**.
5. Connect your iOS device to your computer.
6. Go to the **TARGETS > Project Name > General > Signing** menu, choose **Automatically manage signing**, and then click **Enable Automatic** on the pop-up window.
	
	![](https://web-cdn.agora.io/docs-files/1568803558097)
</details>

### <a name="IntegrateSDK"></a>Integrate the SDK
Choose either of the following methods to integrate the Agora SDK into your project.

<div class="alert note">As of v3.0.1, the SDK only include the dynamic library <tt>AgoraRtcKit.framework</tt>. To upgrade your SDK to v3.0.1, refer to <a href="../../en/Voice/migration_apple.md">Migration Guide</a >.</div>

**Method 1: Automatically integrate the SDK with CocoaPods**

1. Ensure that you have installed **CocoaPods** before the following steps. See the installation guide in [Getting Started with CocoaPods](https://guides.cocoapods.org/using/getting-started.html#getting-started).
2. In **Terminal**, go to the project path and run the `pod init` command to create a **Podfile** in the project folder.
3. Open the **Podfile**, delete all contents and input the following contents. Remember to change **Your App** to the target name of your project.
```
# platform :ios, '9.0' use_frameworks!
target 'Your App' do
    pod 'AgoraRtcEngine_iOS'
end
```

<div class="alert note">Ensure that you use <tt>pod 'AgoraRtcEngine_iOS_Crypto'</tt> to replace <tt>pod 'AgoraRtcEngine_iOS'</tt> when using the channel encryption function. After integrating the encryption library, the app size increases.</div>

4. Go back to **Terminal**, and run the `pod update` command to update the local libraries.
5. Run the `pod install` command to install the Agora SDK. Once you successfully install the SDK, it shows `Pod installation complete!` in Terminal, and you can see an **xcworkspace** file in the project folder.
6. Open the generated xcworkspace file in **Xcode**.

**Method 2: Manually add the SDK files**

1. Go to [SDK Downloads](https://docs.agora.io/en/Agora%20Platform/downloads), download the latest version of the Agora SDK for iOS, and unzip the downloaded SDK package.
2. Copy the **AgoraRtcKit.framework** file in the **libs** folder to the project folder.
3. Open **Xcode** (take the Xcode 11.0 as an example), go to the **TARGETS > Project Name > General > Frameworks, Libraries, and Embedded Content** menu, click **Add Other...** after clicking **+** to add **AgoraRtcKit.framework**. Once added, the project automatically links to other system libraries. To ensure that the signature of the dynamic library is the same as the signature of the app, you need to set the **Embed** attribute of the dynamic library to **Embed & Sign**.
  <div class="alert warning">According to the requirement of Apple, the Extension of app cannot contain the dynamic library. If you need to integrate the SDK with the dynamic library in the Extension, change the file status as <b>Do Not Embed</b>.</div>

 <div class="alert note">Ensure that you integrate <b>AgoraRtcCryptoLoader.framework</b> when using the channel encryption function. After integrating the library, the app size increases.</div>

 **Before integrating the dynamic library**：
 
 ![](https://web-cdn.agora.io/docs-files/1583329514061)
 
 **After integrating the dynamic library**：
 
 ![](https://web-cdn.agora.io/docs-files/1584688934447)


### Add project permissions
Add the following permissions in the **info.plist** file for device access according to your needs:

| Key | Type | Value |
| ---------------- | ---------------- | ---------------- |
| Privacy - Microphone Usage Description      | String      | To access the microphone, such as for a call.      |


**Before**:
 
![](https://web-cdn.agora.io/docs-files/1584604864457)
 
**After**:

 ![](https://web-cdn.agora.io/docs-files/1584604875770) 

## Implement the basic voice call

This section introduces how to use the Agora Voice SDK to make a voice call. The following figure shows the API call sequence of a basic one-to-one voice call.

![](https://web-cdn.agora.io/docs-files/1584691892432)

### 1. Create the UI

Create the user interface (UI) for the one-to-one call in your project. Skip to [Import the class](#ImportClass) if you already have a UI in your project.

When you are implementing a voice call, we recommend adding the following elements into the UI:
	
- The voice call window
- The end-call button

When you use the UI setting of the demo project, you can see the following interface:

![](https://web-cdn.agora.io/docs-files/1584592390009)

### <a name="ImportClass"></a> 2. Import the class

Import the `AgoraRtcKit` class in your project.

```objective-c
// Objective-C
// As of v3.0.0, the SDK uses the AgoraRtcKit object.
#import <AgoraRtcKit/AgoraRtcEngineKit.h>
// The SDK of a version earlier than v3.0.0 uses the AgoraRtcEngineKit object.
#import <AgoraRtcEngineKit/AgoraRtcEngineKit.h>
```

```swift
// Swift
// As of v3.0.0, the SDK uses the AgoraRtcKit object.
import AgoraRtcKit
// The SDK of a version earlier than v3.0.0 uses the AgoraRtcEngineKit object.
import AgoraRtcEngineKit
```

<div class="alert note"> The Agora Voice SDK uses libc++ (LLVM) by default. Contact Agora support If you want to use libstdc++ (GNU). The SDK provides FAT image libraries with multi-architecture support for both 32/64-bit audio emulators and 32/64-bit voice real devices.</div>

### 3. Initialize AgoraRtcEngineKit

Create and initialize the `AgoraRtcEngineKit` object before calling any other Agora APIs.

In this step, you need to use the App ID of your project. Follow these steps to [create an Agora project](https://docs.agora.io/en/Agora%20Platform/manage_projects?platform=All%20Platforms) in Console and get an [App ID](https://docs.agora.io/en/Agora%20Platform/terms?platform=All%20Platforms#a-nameappidaapp-id ).

1. Go to [Console](https://dashboard.agora.io/) and click the **[Project Management](https://dashboard.agora.io/projects)** icon ![](https://web-cdn.agora.io/docs-files/1551254998344) on the left navigation panel. 
2. Click **Create** and follow the on-screen instructions to set the project name, choose an authentication mechanism, and Click **Submit**. 
3. On the **Project Management** page, find the **App ID** of your project. 

Call the `sharedEngineWithAppId` method and pass in the App ID to initialize the `AgoraRtcEngineKit` object.

You can also listen for callback events, such as when the remote user joins or leaves the channel. 

```objective-c
// Objective-C
- (void)initializeAgoraEngine {
    // Input your App ID to initialize the AgoraRtcEngineKit object.
    self.agoraKit = [AgoraRtcEngineKit sharedEngineWithAppId:appID delegate:self];
}
```

```swift
// Swift
func initializeAgoraEngine() {
   // Initialize the AgoraRtcEngineKit object.
   agoraKit = AgoraRtcEngineKit.sharedEngine(withAppId: AppID, delegate: self)
}
```

### 4. Join a channel

After initializing the `AgoraRtcEngineKit` object, you can call the `joinChannelByToken` method to join a channel. In this method, set the following parameters:

- channelId: Specify the channel name that you want to join. Input your `channelId` before running the sample code.
- token: Pass a token that identifies the role and privilege of the user.  You can set it as one of the following values:
	- `nil`.
	- A temporary token generated in Console. A temporary token is valid for 24 hours. For details, see [Get a Temporary Token](https://docs.agora.io/en/Agora%20Platform/token?platform=All%20Platforms#get-a-temporary-token).
	- A token generated at the server. This applies to scenarios with high-security requirements. For details, see [Generate a token from Your Server](../../en/Voice/token_server.md).
	<div class="alert note">If your project has enabled the app certificate, ensure that you provide a token.</div>
- uid: ID of the local user that is an integer and should be unique. If you set `uid` as 0,  the SDK assigns a user ID for the local user and returns it in the `joinSuccessBlock` callback.
- joinSuccessBlock: Returns that the user joins the specified channel. It is same as `didJoinChannel`. We recommend setting `joinSuccessBlock` as `nil`, so that the SDK can trigger the `didJoinChannel` callback.

For more details on the parameter settings, see [joinChannelByToken](https://docs.agora.io/en/Voice/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/joinChannelByToken:channelId:info:uid:joinSuccess:).

```objective-c
// Objective-C
- (void)joinChannel {
    // Join a channel with a token.
    [self.agoraKit joinChannelByToken:token channelId:@"demoChannel1" info:nil uid:0 joinSuccess:^(NSString *channel, NSUInteger uid, NSInteger elapsed) {
    }];
}
```

```swift
// Swift
func joinChannel() {
    // Join a channel with a token.
    agoraKit.joinChannel(byToken: Token, channelId: "demoChannel1", info:nil, uid:0) { [unowned self] (channel, uid, elapsed) -> Void in}
            self.logVC?.log(type: .info, content: "did join channel")
        }
        isStartCalling = true
}
```

### 5. Leave the channel

Call the `leaveChannel` method to leave the current call according to your scenario, for example, when the call ends, when you need to close the app, or when your app runs in the background.

```objective-c
// Objective-C
- (void)leaveChannel {
    // Leave the channel.
    [self.agoraKit leaveChannel:^(AgoraChannelStats *stat)
}
```

```swift
// Swift
func leaveChannel() {
        // Leave the channel.
        agoraKit.leaveChannel(nil)
        isStartCalling = false
        self.logVC?.log(type: .info, content: "did leave channel")
    }
```

## Run the project
Run the project on your iOS device. You can hear the remote user when you successfully start a one-to-one voice call in the app.

## Reference
We provide open-source Group-Voice-Call sample projects that implements the group voice call on GitHub. For scenarios involving group voice calls, you can download the demo project as a code source reference.

- Swift: See [RoomViewController.swift](https://github.com/AgoraIO/Basic-Audio-Call/blob/master/Group-Voice-Call/OpenVoiceCall-iOS/OpenVoiceCall/RoomViewController.swift) file in the [OpenVoiceCall-iOS](https://github.com/AgoraIO/Basic-Audio-Call/tree/master/Group-Voice-Call/OpenVoiceCall-iOS) sample project.
- Objective-C project: See [RoomViewController.m](https://github.com/AgoraIO/Basic-Audio-Call/blob/master/Group-Voice-Call/OpenVoiceCall-iOS-Objective-C/OpenVoiceCall-iOS-Objective-C/RoomViewController.m) file in the [OpenVoiceCall-iOS-Objective-C](https://github.com/AgoraIO/Basic-Audio-Call/tree/master/Group-Voice-Call/OpenVoiceCall-iOS-Objective-C) sample project.

When integrating the Agora Voice SDK, you can also refer to the following articles: 

- [How can I set the log file?](https://docs.agora.io/en/faq/logfile)

