
---
title: Start Live Interactive Audio Streaming
description: 
platform: iOS
updatedAt: Tue Sep 22 2020 02:03:49 GMT+0800 (CST)
---
# Start Live Interactive Audio Streaming
Use this guide to quickly start the live interactive audio streaming demo with the Agora Voice SDK for iOS.

## Sample project

We provide an open-source [OpenLive-Voice-Only-iOS-Objective-C](https://github.com/AgoraIO/Basic-Audio-Broadcasting/tree/master/OpenLive-Voice-Only-iOS-Objective-C) or [OpenLive-Voice-Only-iOS](https://github.com/AgoraIO/Basic-Audio-Broadcasting/tree/master/OpenLive-Voice-Only-iOS) demo project that implements the basic live interactive audio streaming on GitHub. You can try the demo and view the source code.

## Prerequisites

- Xcode 9.0 or later
- An iOS device running iOS 8.0 or later
- A valid [Agora account](https://docs.agora.io/en/Agora%20Platform/sign_in_and_sign_up) and an [App ID](https://docs.agora.io/en/Agora%20Platform/token?platform=All%20Platforms#get-an-app-id)

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

<div class="alert note">As of v3.0.1, the SDK only include the dynamic library <tt>AgoraRtcKit.framework</tt>. To upgrade your SDK to v3.0.1, refer to <a href="../../en/Audio%20Broadcast/migration_apple.md">Migration Guide</a >.</div>

**Method 1: Automatically integrate the SDK with CocoaPods**

1. Ensure that you have installed **CocoaPods** before the following steps. See the installation guide in [Getting Started with CocoaPods](https://guides.cocoapods.org/using/getting-started.html#getting-started).
2. In **Terminal**, go to the project path and run the `pod init` command to create a `Podfile` in the project folder.
3. Open the `Podfile`, delete all contents and input the following contents. Remember to change `Your App` to the target name of your project, and change `version` to the version of the SDK which you want to integrate.

 
 ```
# platform :ios, '9.0' use_frameworks!
target 'Your App' do
    pod 'AgoraAudio_iOS', '~> version'
end
```


4. Go back to **Terminal**, and run the `pod update` command to update the local libraries.
5. Run the `pod install` command to install the Agora SDK. Once you successfully install the SDK, it shows `Pod installation complete!` in Terminal, and you can see an `xcworkspace` file in the project folder.
6. Open the generated `xcworkspace` file in **Xcode**.

**Method 2: Manually add the SDK files**

1. Go to [SDK Downloads](https://docs.agora.io/en/Agora%20Platform/downloads), download the latest version of the Agora SDK for iOS, and unzip the downloaded SDK package. The SDK package contains two types of `AgoraRtcKit.framework` and `AgoraRtcCryptoLoader.framework`, and you can find the following differences:
 - The `AgoraRtcKit.framework` and `AgoraRtcCryptoLoader.framework` in the `libs` folder contains armv7 and arm64 architecture, and does not support a simulator. After integrating these libraries, the app can be uploaded to the App Store directly.
 - The `AgoraRtcKit.framework` and `AgoraRtcCryptoLoader.framework` file in the `ALL_ARCHITECTURE` folder contains armv7, arm64, x86_64 and i386 architecture, and supports simulators. After integrating these libraries, the app cannot be uploaded to the App Store directly, and you need to remove x86_64 and i386 architecture in the libraries manually before uploading the app.
![](https://web-cdn.agora.io/docs-files/1591776711076)
2. Copy `AgoraRtcKit.framework` to the project folder.
3. Open **Xcode** (take the Xcode 11.0 as an example), go to the **TARGETS > Project Name > General > Frameworks, Libraries, and Embedded Content** menu, click **Add Other...** after clicking **+** to add `AgoraRtcKit.framework`. Once added, the project automatically links to other system libraries. To ensure that the signature of the dynamic library is the same as the signature of the app, you need to set the **Embed** attribute of the dynamic library to **Embed & Sign**.

<details>
	<summary><font color="#3ab7f8">To integrate the SDK earlier than v3.0.0, click here to see the integration steps.</font></summary>
	
1. Unzip the downloaded SDK package.
2. Copy `AgoraRtcEngineKit.framework` to the project folder.
3. Open **Xcode** (take the Xcode 11.0 as an example), go to the **TARGETS > Project Name > Build Phases > Link Binary with Libraries** menu, and click **+** to add the following frameworks and libraries. To add the `AgoraRtcEngineKit.framework` file, remember to click **Add Other...** after clicking **+**.

 - AgoraRtcEngineKit.framework
 - Accelerate.framework
 - AudioToolbox.framework
 - AVFoundation.framework
 - CoreMedia.framework
 - CoreTelephony.framework
 - libc++.tbd
 - libresolv.tbd
 - SystemConfiguration.framework

</details>

 <div class="alert warning">According to the requirement of Apple, the Extension of app cannot contain the dynamic library. If you need to integrate the SDK with the dynamic library in the Extension, change the file status as <b>Do Not Embed</b>.</div>

 <div class="alert note">Ensure that you integrate <tt>AgoraRtcCryptoLoader.framework</tt> when using the channel encryption function. After integrating the library, the app size increases.</div>

**Before integrating the dynamic library**：
 
![](https://web-cdn.agora.io/docs-files/1583329514061)
 
**After integrating the dynamic library**：
 
![](https://web-cdn.agora.io/docs-files/1584688934447)


### Add project permissions
Add the following permissions in the **info.plist** file for device access according to your needs:

| Key | Type | Value |
| ---------------- | ---------------- | ---------------- |
| Privacy - Microphone Usage Description      | String      | To access the microphone, such as for a call or live interactive streaming.      |


<div class="alert note">iOS 14.0 adds <b>Privacy - Local Network Usage Description</b> permission. If you use the SDK with the version earlier than v3.1.2, you need to add this permission. See <a href="https://docs.agora.io/en/faq/local_network_privacy">FAQ</a > for details.</div>
	
**Before**:
 
![](https://web-cdn.agora.io/docs-files/1584604864457)
 
**After**:

 ![](https://web-cdn.agora.io/docs-files/1584604875770) 

## Implement the basic live interactive streaming

This section introduces how to use the Agora Voice SDK to make the live interactive streaming. The following figure shows the API call sequence of the live interactive audio streaming.
![](https://web-cdn.agora.io/docs-files/1585490874581)

### 1. Create the UI

Create the user interface (UI) for the live interactive audio streaming in your project. Skip to [Import the class](#ImportClass) if you already have a UI in your project.

When you are implementing the live interactive audio streaming, we recommend adding the following elements into the UI:

- The window for live interactive audio streaming
- The exit button
	
When you use the UI setting of the demo project, you can see the following interface:

![](https://web-cdn.agora.io/docs-files/1585490926278)

### <a name="ImportClass"></a>2. Import the class

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

<div class="alert note"> The RTC Native SDK uses libc++ (LLVM) by default. Contact <a href="mailto:support@agora.io">support@agora.io</a > if you want to use libstdc++ (GNU). The SDK provides FAT image libraries with multi-architecture support for both 32/64-bit audio emulators and 32/64-bit audio real devices.</div>

### 3. Initialize AgoraRtcEngineKit

Create and initialize the `AgoraRtcEngineKit` object before calling any other Agora APIs.

Call the `sharedEngineWithAppId` method and pass in the App ID to initialize the `AgoraRtcEngineKit` object.

You can also listen for callback events, such as when the host joins the channel and leaves the channel. 

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

### 4. Set the channel profile

After initializing the `AgoraRtcEngineKit` object, call the `setChannelProfile` method to set the channel profile as `LiveBroadcasting`. 

One `AgoraRtcEngineKit` object uses one profile only. If you want to switch to another profile, destroy the current `AgoraRtcEngineKit` object with the `destroy` method and create a new one before calling the `setChannelProfile` method.

```objective-c
// Objective-C
// Set the channel profile.
[self.agoraKit setChannelProfile:AgoraChannelProfileLiveBroadcasting];
```

```swift
// Swift
// Set the channel profile.
agoraKit.setChannelProfile(.liveBroadcasting)
```

### 5. Set the client role

An interactive streaming channel has two client roles: `Broadcaster` and `Audience`, and the default role is `Audience`. After setting the channel profile to `LiveBroadcasting`, your app may use the following steps to set the client role:

1. Allow the user to set the role as `Broadcaster` or `Audience`. 
2. Call the `setClientRole` method and pass in the client role set by the user.

Note that in the live interactive streaming, only the host can be heard. If you want to switch the client role after joining the channel, call the `setClientRole` method.

```objective-c
// Objective-C
 if (self.isBroadcaster) {
        self.clientRole = AgoraClientRoleAudience;
        if (self.fullSession.uid == 0) {
            self.fullSession = nil;
        }
    } else {
        self.clientRole = AgoraClientRoleBroadcaster;
    }
    // Set the client role.
    [self.rtcEngine setClientRole:self.clientRole];
```

```swift
// Swift
// Set the client role.
agoraKit.setClientRole(.audience)
agoraKit.setClientRole(.broadcaster)
```

### <a name="JoinChannel"></a>6. Join a channel

After setting the client role, you can call the `joinChannelByToken` method to join a channel. In this method, set the following parameters:

- `channelId`: Specify the channel name that you want to join. Input your `channelId` before running the sample code.
- `token`: Pass a token that identifies the role and privilege of the user.  You can set it as one of the following values:
	- `nil`.
	- A temporary token generated in Console. A temporary token is valid for 24 hours. For details, see [Get a Temporary Token](https://docs.agora.io/en/Agora%20Platform/token#get-a-temporary-token).
	- A token generated at the server. This applies to scenarios with high-security requirements. For details, see [Generate a token from Your Server](../../en/Audio%20Broadcast/token_server.md).
	<div class="alert note">If your project has enabled the app certificate, ensure that you provide a token.</div>
- `uid`: ID of the local user that is an integer and should be unique. If you set `uid` as `0`,  the SDK assigns a user ID for the local user and returns it in the `joinSuccessBlock` callback.
- `joinSuccessBlock`: Returns that the user joins the specified channel. It is same as `didJoinChannel`. We recommend setting `joinSuccessBlock` as `nil`, so that the SDK can trigger the `didJoinChannel` callback.

For more details on the parameter settings, see [joinChannelByToken](https://docs.agora.io/en/Audio%20Broadcast/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/joinChannelByToken:channelId:info:uid:joinSuccess:).

<div class="alert note">For RTC Native SDKs prior to v3.0.0, you need to call <code>enableWebSdkInteroperability</code> to enable the interoperability with the RTC Web SDK if there is a Web users in the channel. As of v3.0.0, the RTC Native SDK enables its interoperability with the RTC Web SDK by default.</div>

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
// Join a channel with a token.
agoraKit.joinChannel(byToken: KeyCenter.Token, channelId: channelId, info: nil, uid: 0, joinSuccess: nil)
```

### 7. Leave the channel

Call the `leaveChannel` method to leave the current channel according to your scenario, for example, when the live interactive streaming ends, when you need to close the app, or when your app runs in the background.

```objective-c
// Objective-C
    // Leave the channel.
    [self.rtcEngine leaveChannel:nil];
    if (self.isBroadcaster) {
        [self.rtcEngine stopPreview];
    }
```

```swift
// Swift
func leaveChannel() {       
    setIdleTimerActive(true)
    // Leave the channel.
    agoraKit.leaveChannel(nil)
    if settings.role == .broadcaster {
       agoraKit.stopPreview()
    }
    navigationController?.popViewController(animated: true)
    }
```

## Run the project

Run the project on your iOS device. When you set the role as the host and successfully start the live interactive audio streaming, you can hear the voice of the host in the app.

## Reference

- [How can I set the log file?](https://docs.agora.io/en/faq/logfile)
- [How can I listen for an audience joining or leaving a live interactive streaming channel?](https://docs.agora.io/en/faq/audience_event)
- [Why do I see a prompt to find local network devices when launching an iOS app?](https://docs.agora.io/en/faq/local_network_privacy)
