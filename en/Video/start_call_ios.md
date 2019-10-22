
---
title: Start a Call
description: 
platform: iOS
updatedAt: Tue Oct 22 2019 06:19:52 GMT+0800 (CST)
---
# Start a Call
Use this guide to quickly start a basic voice/video call demo with the Agora SDK for iOS.

## Try the demo

We provide an open-source [Agora-iOS-Tutorial-Objective-C-1to1](https://github.com/AgoraIO/Basic-Video-Call/tree/master/One-to-One-Video/Agora-iOS-Tutorial-Objective-C-1to1)/[Agora-iOS-Tutorial-Swift-1to1](https://github.com/AgoraIO/Basic-Video-Call/tree/master/One-to-One-Video/Agora-iOS-Tutorial-Swift-1to1) demo project that implements the basic one-to-one video call on GitHub. You can try the demo and view the source code.

## Prerequisites

- Xcode 9.0 or later
- An iOS device running iOS 8.0 or later
- A valid Agora account. ([Sign up](https://sso.agora.io/en/signup) for free)

<div class="alert note">Open the specified ports in <a href="https://docs.agora.io/en/Agora%20Platform/firewall?platform=All%20Platforms">Firewall Requirements</a > if your network has a firewall.</div>

## Set up the development environment

In this section, we will create an iOS project, and integrate the SDK into the project.

### Create an iOS project
Now, let's build an iOS project from scratch. Skip to [Integrate the SDK](#IntegrateSDK) if an iOS project already exists.

~dbc73cd0-f186-11e9-8bb5-37fc243d5a73~

### <a name="IntegrateSDK"></a>Integrate the SDK
Choose either of the following methods to integrate the Agora SDK into your project.

**Method 1: Automatically integrate the SDK with CocoaPods**

1. Ensure that you have installed **CocoaPods** before the following steps. See the installation guide in [Getting Started with CocoaPods](https://guides.cocoapods.org/using/getting-started.html#getting-started).
2. In **Terminal**, go to the project path and run the `pod init` command to create a **Podfile** in the project folder.
3. Open the **Podfile**, delete all contents and input the following contents. Remember to change **Your App** to the target name of your project.
```
platform :ios, '9.0' use_frameworks!
target 'Your App' do
    pod 'AgoraRtcEngine_iOS'
end
```
4. Go back to **Terminal**, and run the `pod update` command to update the local libraries.
5. Run the `pod install` command to install the Agora SDK. Once you successfully install the SDK, it shows `Pod installation complete!` in Terminal, and you can see an **xcworkspace** file in the project folder.
6. Open the generated xcworkspace file in **Xcode**.

**Method 2: Manually add the SDK files**

1. Go to [SDK Downloads](https://docs.agora.io/en/Agora%20Platform/downloads), download the latest version of the Agora SDK for iOS, and unzip the downloaded SDK package.
2. Copy the **AgoraRtcEngineKit.framework** file in the **libs** folder to the project folder.
3. In **Xcode**, go to the **TARGETS > Project Name > Build Phases > Link Binary with Libraries** menu, and click **+** to add the following frameworks and libraries. To add the **AgoraRtcEngineKit.framework** file, remember to click **Add Other...** after clicking **+**.
	- AgoraRtcEngineKit.framework
	- Accelerate.framework
	- AudioToolbox.framework
	- AVFoundation.framework
	- CoreMedia.framework
	- CoreML.framework
	- CoreTelephony.framework
	- libc++.tbd
	- libresolv.tbd
	- SystemConfiguration.framework
	- VideoToolbox.framework

 **Before**:
 
 ![](https://web-cdn.agora.io/docs-files/1568801101072)
 
 **After**:
 
 ![](https://web-cdn.agora.io/docs-files/1568801108276)
 
<div class="alert note">If your device runs <b>iOS 9.0</b> or earlier, set the dependency of <b>CoreML.framework</b> as <b>Optional</b> in <b>Xcode</b>.</div>


### Add project permissions
Add the following permissions in the **info.plist** file for device access according to your needs:

| Key | Type | Value |
| ---------------- | ---------------- | ---------------- |
| Privacy - Microphone Usage Description      | String      | To access the microphone, such as for a video call.      |
| Privacy - Camera Usage Description      | String      | To access the camera, such as for a video call.      |

**Before**:
 
![](https://web-cdn.agora.io/docs-files/1568801138074)
 
**After**:

![](https://web-cdn.agora.io/docs-files/1568801129112)

## Implement the basic call

This section introduces how to use the Agora SDK to make a video call. The following figure shows the API call sequence of a basic one-to-one video call.

![](https://web-cdn.agora.io/docs-files/1568260835977)

### 1. Create the UI

Create the user interface (UI) for the one-to-one call in your project. Skip to [Import the class](#ImportClass) if you already have a UI in your project.

If you are implementing a video call, we recommend adding the following elements into the UI:
	
- The local video view
- The remote video view
- The end-call button

When you use the UI setting of the demo project, you can see the following interface:

![](https://web-cdn.agora.io/docs-files/1568801473939)

### <a name="ImportClass"></a> 2. Import the class

Import the `AgoraRtcEngineKit` class in your project.

```objective-c
// Objective-C
#import <AgoraRtcEngineKit/AgoraRtcEngineKit.h>
```

```swift
// Swift
import AgoraRtcEngineKit
```

<div class="alert note"> The Agora Native SDK uses libc++ (LLVM) by default. Contact Agora support If you want to use libstdc++ (GNU). The SDK provides FAT image libraries with multi-architecture support for both 32/64-bit audio emulators and 32/64-bit audio/video real devices.</div>

### 3. Initialize AgoraRtcEngineKit

Create and initialize the `AgoraRtcEngineKit` object before calling any other Agora APIs.

In this step, you need to use the App ID of your project. Follow these steps to [create an Agora project](https://docs.agora.io/en/Agora%20Platform/manage_projects?platform=All%20Platforms) in Console and get an [App ID](https://docs.agora.io/en/Agora%20Platform/terms?platform=All%20Platforms#a-nameappidaapp-id ).

1. Go to [Console](https://dashboard.agora.io/) and click the **[Project Management](https://dashboard.agora.io/projects)** icon ![](https://web-cdn.agora.io/docs-files/1551254998344) on the left navigation panel. 
2. Click **Create** and follow the on-screen instructions to set the project name, choose an authentication mechanism, and Click **Submit**. 
3. On the **Project Management** page, find the **App ID** of your project. 

Call the `sharedEngineWithAppId` method and pass in the App ID to initialize the `AgoraRtcEngineKit` object.

You can also listen for callback events, such as when the local user joins the channel, and when the first video frame of a remote user is decoded. 

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

### 4. Set the local video view

If you are implementing a voice call, skip to [Join a channel](#JoinChannel).

After initializing the `AgoraRtcEngineKit` object, set the local video view before joining the channel so that you can see yourself in the call. Follow these steps to configure the local video view: 

- Call the `enableVideo` method to enable the video module. 
- Call the `setupLocalVideo` method to configure the local video display settings. 

```objective-c
// Objective-C
- (void)setupVideo {
  // Enable the video
  [self.agoraKit enableVideo];
}
- (void)setupLocalVideo {
    AgoraRtcVideoCanvas *videoCanvas = [[AgoraRtcVideoCanvas alloc] init];
    videoCanvas.uid = 0;
    videoCanvas.view = self.localVideo;
    videoCanvas.renderMode = AgoraVideoRenderModeHidden;
    // Set the local video view.
    [self.agoraKit setupLocalVideo:videoCanvas];
}
```

```swift
// Swift
func setupVideo() {
    // Enable the video
    agoraKit.enableVideo()
}
func setupLocalVideo() {
  let videoCanvas = AgoraRtcVideoCanvas()
  videoCanvas.uid = 0
  videoCanvas.view = localVideo
  videoCanvas.renderMode = .hidden
  // Set the local video view.
  agoraKit.setupLocalVideo(videoCanvas)
}
```

### <a name="JoinChannel"></a>5. Join a channel

After initializing the `AgoraRtcEngineKit` object and setting the local video view (for a video call), you can call the `joinChannelByToken` method to join a channel. In this method, set the following parameters:

- channelId: Specify the channel name that you want to join. Input your `channelId` before running the sample code.
- token: Pass a token that identifies the role and privilege of the user.  You can set it as one of the following values:
	- `nil`.
	- A temporary token generated in Console. A temporary token is valid for 24 hours. For details, see [Get a Temporary Token](https://docs.agora.io/en/Agora%20Platform/token?platform=All%20Platforms#get-a-temporary-token).
	- A token generated at the server. This applies to scenarios with high-security requirements. For details, see [Generate a token from Your Server](../../en/Video/token_server.md).
	<div class="alert note">If your project has enabled the app certificate, ensure that you provide a token.</div>
- uid: ID of the local user that is an integer and should be unique. If you set `uid` as 0,  the SDK assigns a user ID for the local user and returns it in the `joinSuccessBlock` callback.
- joinSuccessBlock: Returns that the user joins the specified channel. It is same as `didJoinChannel`. We recommend setting `joinSuccessBlock` as `nil`, so that the SDK can trigger the `didJoinChannel` callback.

For more details on the parameter settings, see [joinChannelByToken](https://docs.agora.io/en/Video/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/joinChannelByToken:channelId:info:uid:joinSuccess:).

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
    self.isLocalVideoRender = true
            self.logVC?.log(type: .info, content: "did join channel")
        }
        isStartCalling = true
}
```

### 6. Set the remote video view

In a video call, you should be able to see other users too. This is achieved by calling the `setupRemoteVideo` method after joining the channel.

Shortly after a remote user joins the channel, the SDK gets the remote user's ID in the `firstRemoteVideoDecodedOfUid` callback. Call the `setupRemoteVideo` method in the callback, and pass in the uid to set the video view of the remote user.

```objective-c
// Objective-C
// Listen for the firstRemoteVideoDecodedOfUid callback.
// This callback occurs when the first video frame of a remote user is received and decoded after the remote user successfully joins the channel.
// You can call the setupRemoteVideo method in this callback to set up the remote video view.
- (void)rtcEngine:(AgoraRtcEngineKit *)engine firstRemoteVideoDecodedOfUid:(NSUInteger)uid size: (CGSize)size elapsed:(NSInteger)elapsed {
    if (self.remoteVideo.hidden) {
        self.remoteVideo.hidden = NO;
    }
    AgoraRtcVideoCanvas *videoCanvas = [[AgoraRtcVideoCanvas alloc] init];
    videoCanvas.uid = uid;
    videoCanvas.view = self.remoteVideo;
    videoCanvas.renderMode = AgoraVideoRenderModeHidden;
    // Set the remote video view.
    [self.agoraKit setupRemoteVideo:videoCanvas];
}
```

```swift
// Swift
// Listen for the firstRemoteVideoDecodedOfUid callback.
// This callback occurs when the first video frame of a remote user is received and decoded after the remote user successfully joins the channel.
// You can call the setupRemoteVideo method in this callback to set up the remote video view.
func rtcEngine(_ engine: AgoraRtcEngineKit, firstRemoteVideoDecodedOfUid uid:UInt, size:CGSize, elapsed:Int) {
        isRemoteVideoRender = true
        let videoCanvas = AgoraRtcVideoCanvas()
        videoCanvas.uid = uid
        videoCanvas.view = remoteVideo
        videoCanvas.renderMode = .hidden
        // Set the remote video view.
        agoraKit.setupRemoteVideo(videoCanvas)
    }
```

### 7. Leave the channel

Call the `leaveChannel` method to leave the current call according to your scenario, for example, when the call ends, when you need to close the app, or when your app runs in the background.

```objective-c
// Objective-C
- (void)leaveChannel {
    // Leave the channel.
    [self.agoraKit leaveChannel:^(AgoraChannelStats *stat) {
        [self.agoraKit leaveChannel:^(AgoraChannelStats *stat) {
        [self hideControlButtons];
        [self.remoteVideo removeFromSuperview];
        [self.localVideo removeFromSuperview];
    }];
}
```

```swift
// Swift
func leaveChannel() {
        // Leave the channel.
        agoraKit.leaveChannel(nil)
        isRemoteVideoRender = false
        isLocalVideoRender = false
        isStartCalling = false
        self.logVC?.log(type: .info, content: "did leave channel")
    }
```

### Sample code
You can find the sample code logic in the [VideoChatViewController.m](https://github.com/AgoraIO/Basic-Video-Call/blob/master/One-to-One-Video/Agora-iOS-Tutorial-Objective-C-1to1/Agora%20iOS%20Tutorial%20Objective-C/VideoChat/VideoChatViewController.m)/[VideoChatViewController.swift](https://github.com/AgoraIO/Basic-Video-Call/blob/master/One-to-One-Video/Agora-iOS-Tutorial-Swift-1to1/Agora%20iOS%20Tutorial/VideoChatViewController.swift) file in the Agora-iOS-Tutorial-Objective-C-1to1/Agora-iOS-Tutorial-Swift-1to1  demo project.

## Run the project
Run the project on your iOS device. You can see both the local and remote video views when you successfully start a one-to-one video call in the app.
