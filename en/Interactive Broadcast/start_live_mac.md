
---
title: Start Live Interactive Video Streaming
description: 
platform: macOS
updatedAt: Fri Sep 18 2020 04:52:30 GMT+0800 (CST)
---
# Start Live Interactive Video Streaming
Use this guide to quickly start the live interactive video streaming demo with the Agora Video SDK for macOS.

## Sample project

We provide an open-source [OpenLive-macOS-Objective-C](https://github.com/AgoraIO/Basic-Video-Broadcasting/tree/master/OpenLive-macOS-Objective-C) or [OpenLive-macOS-Swift](https://github.com/AgoraIO/Basic-Video-Broadcasting/tree/master/OpenLive-macOS) demo project that implements the basic live interactive video streaming on GitHub. You can try the demo and view the source code.

## Prerequisites

- Xcode 9.0 or later
- A macOS device running macOS 10.10 or later
- A valid [Agora account](https://docs.agora.io/en/Agora%20Platform/sign_in_and_sign_up) and an [App ID](https://docs.agora.io/en/Agora%20Platform/token?platform=All%20Platforms#get-an-app-id)

<div class="alert note">Open the specified ports in <a href="https://docs.agora.io/en/Agora%20Platform/firewall?platform=All%20Platforms">Firewall Requirements</a> if your network has a firewall.</div>

## Set up the development environment

In this section, we will create a macOS project, and integrate the SDK into the project.

### Create a macOS project
Now, let's build a macOS project from scratch. Skip to [Integrate the SDK](#IntegrateSDK) if a macOS project already exists.
<details>
	<summary><font color="#3ab7f8">Create a macOS project</font></summary>

1. Open **Xcode** and click **Create a new Xcode project**.
2. Choose **Cocoa App** as the template and click **Next**.
3. Input the project information, such as the project name, team, organization name, and language, and click **Next**.

	 <div class="alert note">If you haven't added any team information, you will see an <b>Add account...</b> button. Click it, input your Apple ID, and click <b>Next</b> to add your team.</div>
4. Choose the storage path of the project and click **Create**.
5. Go to the **TARGETS > Project Name > General > Signing** menu, choose **Automatically manage signing**, and then click **Enable Automatic** on the pop-up window.
	
 ![](https://web-cdn.agora.io/docs-files/1568803534506)
</details>

### <a name="IntegrateSDK"></a>Integrate the SDK
Choose either of the following methods to integrate the Agora SDK into your project.

<div class="alert note">As of v3.0.1, the SDK only include the dynamic library <tt>AgoraRtcKit.framework</tt>. To upgrade your SDK to v3.0.1, refer to <a href="../../en/Interactive%20Broadcast/migration_apple.md">Migration Guide</a >.</div>

**Method 1: Automatically integrate the SDK with CocoaPods**
	
1. Ensure that you have installed **CocoaPods** before the following steps. See the installation guide in [Getting Started with CocoaPods](https://guides.cocoapods.org/using/getting-started.html#getting-started).
2. In **Terminal**, go to the project path and run the `pod init` command to create a **Podfile** in the project folder.
3. Open the **Podfile**, delete all contents and input the following contents. Remember to change `Your App` to the target name of your project, and change `version` to the version of the SDK which you want to integrate.

 ```
# platform :macOS, '10.11' use_frameworks!
 target 'Your App' do
     pod 'AgoraRtcEngine_macOS', '~> version'
 end
```
4. Go back to **Terminal**, and run the `pod update` command to update the local libraries.
5. Run the `pod install` command to install the Agora SDK. Once you successfully install the SDK, it shows `Pod installation complete!` in Terminal, and you can see an `xcworkspace` file in the project folder.
6. Open the generated xcworkspace file in **Xcode**.

**Method 2: Manually add the SDK files**

1. Go to [SDK Downloads](https://docs.agora.io/en/Agora%20Platform/downloads), download the latest version of the Agora SDK for macOS, and extract the files from the downloaded SDK package.	
2. Copy the `AgoraRtcEngineKit.framework` file in the `libs` folder to the project folder.
3. Open **Xcode** (take the Xcode 11.0 as an example), go to the **TARGETS > Project Name > General > Frameworks, Libraries, and Embedded Content** menu, click **Add Other...** after clicking **+** to add `AgoraRtcKit.framework`. Once added, the project automatically links to other system libraries. To ensure that the signature of the dynamic library is the same as the signature of the app, you need to set the **Embed** attribute of the dynamic library to **Embed & Sign**.

<details>
	<summary><font color="#3ab7f8">To integrate the SDK earlier than v3.0.0, click here to see the integration steps.</font></summary>
	
1. Unzip the downloaded SDK package.
2. Copy `AgoraRtcEngineKit.framework` to the project folder.
3. Open **Xcode** (take the Xcode 11.0 as an example), go to the **TARGETS > Project Name > Build Phases > Link Binary with Libraries** menu, and click **+** to add the following frameworks and libraries. To add the `AgoraRtcEngineKit.framework` file, remember to click **Add Other...** after clicking **+**.

 - AgoraRtcKit.framework
 - Accelerate.framework
 - CoreWLAN.framework
 - libc++.tbd
 - libresolv.9.tbd
 - SystemConfiguration.framework
 - VideoToolbox.framework
	
	
</details>

  <div class="alert warning">According to the requirement of Apple, the Extension of app cannot contain the dynamic library. If you need to integrate the SDK with the dynamic library in the Extension, change the file status as <b>Do Not Embed</b>.</div>

 **Before integrating the dynamic library**：
 
 ![](https://web-cdn.agora.io/docs-files/1583330350955)
 
 **After integrating the dynamic library**：
 
 ![](https://web-cdn.agora.io/docs-files/1584688978762)

### Add project permissions

1. Add the following permissions in the **info.plist** file for device access according to your needs:

| Key | Type | Value |
| ---------------- | ---------------- | ---------------- |
| Privacy - Microphone Usage Description      | String      | To access the microphone, such as for a call or live interactive streaming.      |
| Privacy - Camera Usage Description      | String      | To access the camera, such as for a call or live interactive streaming.      |


**Before**:

![](https://web-cdn.agora.io/docs-files/1584691351348)

**After**:

 ![](https://web-cdn.agora.io/docs-files/1584604886884) 

2. If you enable the **App Sandbox** or **Hardened Runtime** settings in your project, check the following items to add the corresponding permissions:

<table>
    <tr>
        <td><b>Menu</b></td>
        <td><b>Category</b></td>
        <td><b>Item</b></td>
    </tr>
    <tr>
        <td rowspan="4">App Sandbox</td>
        <td rowspan="2">Network</td>
        <td>Incoming Connections (Server)</td>
    </tr>
    <tr>
        <td>Incoming Connections (Client)</td>
    </tr>
	    <tr>
        <td rowspan="2">Hardware</td>
        <td>Camera</td>
    </tr>
    <tr>
        <td>Audio Input</td>
    </tr>
	<tr>
        <td rowspan="2">Hardened Runtime</td>
        <td rowspan="2">Resource Access</td>
        <td>Camera</td>
    </tr>
    <tr>
        <td>Audio Input</td>
</table>
 

<div class="alert note"><li>According to the requirements of Apple: <ul><li>Mac software distributed in the Mac App Store must enable the <b>App Sandbox</b> setting. See details in <a href="https://developer.apple.com/app-sandboxing/">Apple News and Updates</a>.<li>Mac software distributed outside the Mac App Store must enable the <b>Hardened Runtime</b> setting. See details in <a href="https://developer.apple.com/news/?id=09032019a">Apple News and Updates</a>.</li></ul><li>The <b>Hardened Runtime</b>’s library validation prevents an app from loading frameworks, plug-ins, or libraries unless they’re either signed by Apple or signed with the same team ID as the app. When you face with problems such as failure to enumerate the virtual camera, check <b>Hardened Runtime -> Runtime Exceptions -> Disable Library Validation</b> to disable the library validation. </li></div>

## Implement the basic live interactive streaming

This section introduces how to use the Agora Video SDK to start the live interactive video streaming. The following figure shows the API call sequence of the live interactive video streaming.

![](https://web-cdn.agora.io/docs-files/1570604601537)

### 1. Create the UI

Create the user interface (UI) for the interactive video streaming in your project. Skip to [Import the class](#ImportClass) if you already have a UI in your project.
	
If you are implementing the interactive video streaming, we recommend adding the following elements into the UI:

- The view of the host
- The exit button

When you use the UI setting of the demo project, you can see the following interface:

![](https://web-cdn.agora.io/docs-files/1568802207185)

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

<div class="alert note"> The Agora Native SDK uses libc++ (LLVM) by default. Contact Agora support If you want to use libstdc++ (GNU). The SDK provides FAT image libraries with multi-architecture support for both 32/64-bit audio emulators and 32/64-bit audio/video real devices.</div>

### 3. Initialize AgoraRtcEngineKit

Create and initialize the `AgoraRtcEngineKit` object before calling any other Agora APIs.

Call the `sharedEngineWithAppId` method and pass in the App ID to initialize the `AgoraRtcEngineKit` object.

You can also listen for callback events, such as when the local user joins the channel, and when the first video frame of a host is decoded. 

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

```objective
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

An interactive streaming channel has two client roles: `Broadcaster` and `Audience`, and the default role is `Audience`. After setting the channel profile to `Live Broadcast`, your app may use the following steps to set the client role:

1. Allow the user to set the role as `Broadcaster` or `Audience`. 
2. Call the `setClientRole` method and pass in the client role set by the user.

Note that in an live interactive streaming, only the host can be heard and seen. If you want to switch the client role after joining the channel, call the `setClientRole` method.

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

### 6. Set the local video view

After setting the channel profile and client role, set the local video view before joining the channel so that the host can see the local video in the interactive streaming. Follow these steps to configure the local video view:

1. Call the `enableVideo` method to enable the video module.
2. Call the `setupLocalVideo` method to configure the local video display settings. 

```objective-c
// Objective-C
// Enable the video module.
[self.agoraKit enableVideo];
- (void)setupLocalVideo {
    AgoraRtcVideoCanvas *videoCanvas = [[AgoraRtcVideoCanvas alloc] init];
    videoCanvas.uid = 0;
    videoCanvas.view = self.localVideo;
    videoCanvas.renderMode = AgoraVideoRenderModeHidden;
    // Setup the local video view.
    [self.agoraKit setupLocalVideo:videoCanvas];
}
```

```swift
// Swift
// Enable the video module.
agoraKit.enableVideo()
func addLocalSession() {
    let localSession = VideoSession.localSession()
    videoSessions.append(localSession)
    // Setup the local video view.
    agoraKit.setupLocalVideo(localSession.canvas) 
    let mediaInfo = MediaInfo(dimension: settings.dimension,
                                  fps: settings.frameRate.rawValue)
    localSession.mediaInfo = mediaInfo
    }
```

### <a name="JoinChannel"></a>7. Join a channel

After initializing the `AgoraRtcEngineKit` object and setting the local video view (for the interactive video streaming), you can call the `joinChannelByToken` method to join a channel. In this method, set the following parameters:

- `channelId`: Specify the channel name that you want to join. Input your `channelId` before running the sample code.
- `token`: Pass a token that identifies the role and privilege of the user. You can set it as one of the following values:

 - `nil`.
 - A temporary token generated in Console. A temporary token is valid for 24 hours. For details, see [Get a Temporary Token](https://docs.agora.io/en/Agora%20Platform/token#get-a-temporary-token).
 - A token generated at the server. This applies to scenarios with high-security requirements. For details, see [Generate a token from Your Server](../../en/Interactive%20Broadcast/token_server.md).

 <div class="alert note">If your project has enabled the app certificate, ensure that you provide a token.</div>

- `uid`: ID of the local user that is an integer and should be unique. If you set `uid` as `0`,  the SDK assigns a user ID for the local user and returns it in the `joinSuccessBlock` callback.
- `joinSuccessBlock`: Returns that the user joins the specified channel. It is same as `didJoinChannel`. We recommend setting `joinSuccessBlock` as `nil`, so that the SDK can trigger the `didJoinChannel` callback.

For more details on the parameter settings, see [joinChannelByToken](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/joinChannelByToken:channelId:info:uid:joinSuccess:).

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

### 8. Set the remote video view

In the live interactive video streaming, you should be able to see other users too. This is achieved by calling the `setupRemoteVideo` method after joining the channel.

Shortly after a remote host joins the channel, the SDK gets the remote host's ID in the `firstRemoteVideoDecodedOfUid` callback. Call the `setupRemoteVideo` method in the callback, and pass in the `uid` to set the video view of the remote host.

```objective-c
// Objective-C
// Listen for the firstRemoteVideoDecodedOfUid callback.
// This callback occurs when the first video frame of a remote host is received and decoded after the remote host successfully joins the channel.
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
// This callback occurs when the first video frame of a remote host is received and decoded after the remote host successfully joins the channel.
// You can call the setupRemoteVideo method in this callback to set up the remote video view.
func rtcEngine(_ engine: AgoraRtcEngineKit, firstRemoteVideoDecodedOfUid uid: UInt, size: CGSize, elapsed: Int) {
    let userSession = videoSession(of: uid)
    userSession.updateMediaInfo(resolution: size)
    // Set the remote video view.
    agoraKit.setupRemoteVideo(userSession.canvas)
```

### 9. Leave the channel

Call the `leaveChannel` method to leave the current channel according to your scenario, for example, when the live interactive streaming ends, when you need to close the app, or when your app runs in the background.

```objective-c
// Objective-C
[self.rtcEngine setupLocalVideo:nil];
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
    agoraKit.setupLocalVideo(nil)
    // Leave the channel.
    agoraKit.leaveChannel(nil)
    if settings.role == .broadcaster {
       agoraKit.stopPreview()
    }
    navigationController?.popViewController(animated: true)
    }
```

## Run the project

Run the project on your macOS device. When you set the role as the host and successfully start the live interactive video streaming, you can see the video view of yourself in the app. When you set the role as the audience and successfully join the live interactive video streaming, you can see the video view of the host in the app.

## Reference

- [How can I listen for an audience joining or leaving a live interactive streaming channel?](https://docs.agora.io/en/faq/audience_event)
- [How can I set the log file?](https://docs.agora.io/en/faq/logfile)
- [How can I solve black screen issues?](https://docs.agora.io/en/faq/video_blank)
