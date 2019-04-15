
---
title: Starting a Live Video Broadcast
description: 
platform: iOS
updatedAt: Fri Nov 02 2018 04:11:43 GMT+0800 (CST)
---
# Starting a Live Video Broadcast
In this quickstart, you will learn how to use the Agora SDK to start a live video broadcast.

## Prerequisites

1. See [Setting up Your Development Environment](../../en/Quickstart%20Guide/ios_video.md) for the prerequisites, environment setup, and SDK integration guide.
2. See [Agora iOS Video Live Tutorial Sample App](https://github.com/AgoraIO/Basic-Video-Broadcasting/tree/master/OpenLive-iOS) to better understand how a Sample App is created from scratch.

## Quickstart

### Initializing AgoraRtcEngine

Create an AgoraRtcEngine instance by invoking `sharedEngineWithAppId` before joining a live broadcast channel.

In this method:

- Pass the [Agora App ID](../../en/Quickstart%20Guide/ios_video.md). Only applications with the same App ID can join the same channel.
- Specify a delegate object. The Agora SDK uses the delegate object to inform the application of Agora engine runtime events, such as joining or leaving a channel and adding new users into the channel.

```objective-c
//Objective-C
#import <AgoraRtcEngineKit/AgoraRtcEngineKit.h>

...

- (void)initializeAgoraEngine {
  self.agoraKit = [AgoraRtcEngineKit sharedEngineWithAppId:@"Your App ID" delegate:self];
}
```

```swift
//Swift
import AgoraRtcEngineKit

...

func initializeAgoraEngine() {
   agoraKit = AgoraRtcEngineKit.sharedEngine(withAppId: "Your App ID", delegate: self)
}
```

### Setting the Channel Profile

After initializing AgoraRtcEngine, call the `setChannelProfile` method to set the channel profile. AgoraRtcEngine applies optimization according to the channel profile.

In this method, set the channel profile as Live Broadcast. This profile applies to an interactive broadcast scenario. Each channel includes two roles: the host and the audience. The host\(broadcaster\) sends and receives audio and video streams while the audience can only receive the audio streams.

> - Call this method before joining the channel.
> - One Engine can be specified one profile only. If you want to switch to another profile, destroy the current Engine using `destroy` and create a new one before calling this method to set the new channel profile.

```objective-c
//Objective-C
- (void)setChannelProfile() {
  [self.agoraKit setChannelProfile:AgoraChannelProfileLiveBroadcasting]
}
```

```swift
//Swift
func setChannelProfile() {
  agoraKit.setChannelProfile(.channelProfile_LiveBroadcasting)
}
```

### Setting the User Role

An interactive broadcast channel consists of two user roles: the host and the audience. Call the `setClientRole` method to set the user role as broadcaster or audience according to your needs. The differences of the two roles are:

- Host\(Broadcaster\): A host receives and publishes the voice/video streams. The host can also interact with other hosts using voice and video.
- Audience: An audience receives the voice and video streams of the host\(s\).

```objective-c
//Objective-C
- (void)setClientRole() {
[self.agoraKit setClientRole:AgoraClientRoleBroadcaster]
}
```

```swift
 //Swift
 func setClientRole() {
 agoraKit.setClientRole(.clientRole_Broadcaster)
}
```

### Enabling the Video Mode

Call `enableVideo` to enable the video mode. The voice function is enabled by default in the Agora SDK, so you can call this method before or after joining in the channel.

- If you enable the video mode before joining the channel, you enter directly into a video broadcast.
- If you enable the video mode after joining the channel, the voice broadcast switches to a video broadcast.

```objective-c
//Objective-C
- (void)enableVideo {
  [self.agoraKit enableVideo];
  // Default mode is disableVideo
}
```

```swift
//Swift
func enableVideo() {
  agoraKit.enableVideo()  //Default mode is disableVideo
}
```

### Setting the Video Encoding Profile

After the video mode is enabled, call `setVideoEncoderConfiguration` to set the video encoding profile.

In this method, secify the video encoding resolution, frame rate bitrate and orientation mode. 

> - The parameters specified in this method are the maximum values under ideal network conditions. If the video engine cannot render the video using the specified parameters due to poor network conditions, the parameters further down the list are considered until success.
> - If the device camera does not support the resolution specified by the video profile, the SDK automatically chooses a suitable camera resolution. This does not change the encoder resolution, and encoding will still use the resolution specified by `setVideoProfile` .

```objective-c
//Objective-C
AgoraVideoEncoderConfiguration *configuration =
[[AgoraVideoEncoderConfiguration alloc]
initWithSize:AgoraVideoDimension640x360
frameRate:AgoraVideoFrameRateFps15 bitrate:400
orientationMode:AgoraVideoOutputOrientationModeFixedPortrait];

[self.agoraKit setVideoEncoderConfiguration:configuration];
```

```swift
//Swift
let configuration = AgoraVideoEncoderConfiguration(size:
AgoraVideoDimension640x360, frameRate: .fps15, bitrate: 400,
orientationMode: .fixedPortrait)
agoraKit.setVideoEncoderConfiguration(configuration)
```

### Setting the Local Video View

The local video view is the display of the local video streams on the user’s local device.

Call the `setupLocalVideo` method before entering into a channel to bind the application with the video window of the local stream and configure the local video display.

In this method:

- Choose the display window of the local video streams.
- Specify the video display mode.
  - Hidden mode: The SDK scales the video until it fills the visible view boundaries. One dimension of the video may be clipped.
  - Fit mode: The SDK scales the video until one of its dimensions fits the boundaries. Areas that are not filled due to the disparity in the aspect ratio will be filled with black.
- Pass the UID of the local user. The default value is 0.

```objective-c
//Objective-C
- (void)setupLocalVideo {
  AgoraRtcVideoCanvas *videoCanvas = [[AgoraRtcVideoCanvas alloc] init];
  videoCanvas.uid = 0;

  videoCanvas.view = self.localVideo;
  videoCanvas.renderMode = AgoraVideoRenderModeHidden;
  [self.agoraKit setupLocalVideo:videoCanvas];
  // Bind local video stream to view
}
```

```swift
//Swift
func setupLocalVideo() {
  let videoCanvas = AgoraRtcVideoCanvas()
  videoCanvas.uid = 0
  videoCanvas.view = localVideo
  videoCanvas.renderMode = .hidden
  agoraKit.setupLocalVideo(videoCanvas)
}
```

### Joining a Channel

Now, you can join a live broadcast channel.Call the `joinChannelByToken` method to join a channel. Hosts in the same channel can talk to and see each other, and audiences in the same channel can hear and see the hosts.

In this method:

- Pass a Token that can identify the role and privilege of the user. Set it as null if the safety requirements are relatively low. A Token is generated at the Server of the application. For how to generate a Token, see [Security Keys](../../en/Agora%20Platform/token.md).
- Pass a channel ID that can identify the channel. Users that input the same channel ID enter into the same channel.
- Pass a UID that can identify the user. Every user in a channel requires a unique UID. If you want to join the same channel on different devices, ensure that different UIDs are used for each device.

> Once in a call, a user must call the `leaveChannel` method to exit the current call before entering another channel.

```objective-c
//Objective-C
- (void)joinChannel {
  [self.agoraKit joinChannelByToken:nil channelId:@"demoChannel1" info:nil uid:0 joinSuccess:^(NSString *channel, NSUInteger uid, NSInteger elapsed) {
    // Join channel "demoChannel1"
  }];
}
```

```swift
//Swift
func joinChannel() {
  agoraKit.joinChannel(by Token: nil, channelId: "demoChannel1", info:nil, uid:0){[weak self] (sid, uid, elapsed) -> Void in
      // Join channel "demoChannel1"
  }
}
```

### Setting the Remote Video View

The remote video view is the display of the remote video streams on the user’s local device.

Call the `setupRemoteVideo` method to configure the remote video display.

In this method:

- Choose the display window of the remote video streams.
- Specify the video display mode.
  - Hidden mode: The SDK scales the video until it fills the visible view boundaries. One dimension of the video may be clipped.
  - Fit mode: The SDK scales the video until one of its dimensions fits the boundaries. Areas that are not filled due to the disparity in the aspect ratio will be filled with black.
- Pass the UID of the remote user. If the remote UID is unknown to the application, set it later when the application receives the `didJoinedOfUid` event.

```objective-c
//Objective-C
- (void)setupRemoteVideo {
  AgoraRtcVideoCanvas *videoCanvas = [[AgoraRtcVideoCanvas alloc] init];
  videoCanvas.uid = uid;

  videoCanvas.view = self.remoteVideo;
  videoCanvas.renderMode = AgoraVideoRenderModeFit;
  [self.agoraKit setupRemoteVideo:videoCanvas];
  // Bind remote video stream to view
}
```

```swift
//Swift
func setupRemoteVideo() {
    let videoCanvas = AgoraRtcVideoCanvas()
    videoCanvas.uid = 1
    videoCanvas.view = remoteVideo
    videoCanvas.renderMode = .fit
    agoraKit.setupRemoteVideo(videoCanvas)
}
```

### Ending a Live Broadcast

When a live broadcast ends, call the `leaveChannel` method to leave the channel.

This method releases all resources related to the live broadcast. When the user leaves the channel, the SDK triggers the  `didLeaveChannelWithStats` callback.

```objective-c
//Objective-C
-(void)leaveChannel {
  [self.agoraKit leaveChannel:nil];
```

```swift
//Swift
func leaveChannel() {
    agoraKit.leaveChannel(nil)
}
```

> If the `destroy` method is called immediately after `leaveChannel`, the process will be interrupted, and the SDK will not trigger the  `didLeaveChannelWithStats` callback.

## Conclusion

You can now start a video live broadcast.

## Reference

Followings are part of the Agora APIs that we use to start a live broadcast:

- Initialize: `sharedEngineWithAppId`
- Set the Channel Profile: `setChannelProfile`
- Set the User Role: `setClientRole`
- Enable the Video Mode: `enableVideo`
- Set the Video Profile: `setVideoProfile`
- Set the Local Video View: `setupLocalVideo`
- Join a Channel: `joinChannelByToken`
- Set the Remote Video View: `setupRemoteVideo`
- Leave a Channel: `leaveChannel`

Refer to the following links to add more functions to a live broadcast:

- [Advanced: Hosting In](../../en/Quickstart%20Guide/hostin_ios.md)
- [Advanced: Hosting In Across Channels](../../en/Quickstart%20Guide/hostin_crosschannel_ios.md)
- [Advanced: Pushing Streams to the CDN](../../en/Quickstart%20Guide/push_stream_ios.md)
- [Advanced: Recording Voice and Video](../../en/Quickstart%20Guide/recording_voice_video.md)
- [Advanced: Using Agora Built-in Encryption](../../en/Quickstart%20Guide/encryption_ios_agora.md)
- [Advanced: Modifying Raw Data](../../en/Quickstart%20Guide/rawdata_ios.md)
- [Basic: Video Rotation Guide](../../en/Quickstart%20Guide/rotation_guide_ios.md)

You can find all the Agora APIs that can be implemented in a live broadcast in [Interactive Broadcast API](https://docs.agora.io/en/Audio%20Broadcast/API%20Reference/oc/index.html).
