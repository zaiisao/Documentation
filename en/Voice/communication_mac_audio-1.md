
---
title: Making a Voice Call
description: 
platform: macOS
updatedAt: Fri Nov 02 2018 04:06:51 GMT+0800 (CST)
---
# Making a Voice Call
In this quickstart, you will learn how to use the Agora SDK to make a voice call.

## Prerequisites

1. See [Setting up Your Development Environment](../../en/Voice/mac_video.md) for the prerequisites, environment setup, and SDK integration guide.
2. See [Agora macOS Tutorial Sample App](https://github.com/AgoraIO/Basic-Video-Call/tree/master/One-to-One-Video/Agora-macOS-Tutorial-Swift-1to1) to better understand how a Sample App is created from scratch.

## Quickstart

### Initializing AgoraRtcEngine

Create an AgoraRtcEngine instance by invoking `sharedEngineWithAppId` before joining a voice call channel.

In this method:

- Pass the [Agora App ID](../../en/Voice/mac_video.md). Only applications with the same App ID can join the same channel.
- Specify a delegate object. The Agora SDK uses the delegate object to inform the application of Agora engine runtime events, such as joining or leaving a channel and adding new users into the channel.

```objective-c
//Objective-C
#import <AgoraAudioKit/AgoraRtcEngineKit.h>

...

- (void)initializeAgoraEngine {
  self.agoraKit = [AgoraRtcEngineKit sharedEngineWithAppId:@"Your App ID" delegate:self];
}
```

```swift
//Swift
import AgoraAudioKit

...

func initializeAgoraEngine() {
   agoraKit = AgoraRtcEngineKit.sharedEngine(withAppId: "Your App ID", delegate: self)
}
```

### Setting the Channel Profile

After initializing AgoraRtcEngine, call the `setChannelProfile` method to set the channel profile. AgoraRtcEngine applies optimization according to the channel profile.

In this method, set the channel profile as Communication. This profile applies to voice or video calls such as one-to-one or group calls, where all users in the channel can talk freely. This is the default setting.

> - Call this method before joining the channel.
> - One Engine can be specified one profile only. If you want to switch to another profile, destroy the current Engine using `destroy` and create a new one before calling this method to set the new channel profile.

```objective-c
//Objective-C
- (void)setChannelProfile() {
  [self.agoraKit setChannelProfile:AgoraChannelProfileCommunication]
}
```

```swift
//Swift
func setChannelProfile() {
  agoraKit.setChannelProfile(.communication)
}
```

### Joining a Channel

Now, you can join a voice call channel. Call the `joinChannelByToken` method to join a channel. Users in the same channel can talk to each other, and multiple users in the same channel can initiate a group chat.

In this method:

- Pass a Token that can identify the role and privilege of the user. Set it as null if the safety requirements are relatively low. A Token is generated at the Server of the application. For how to generate a Token, see [Security Keys](../../en/Agora%20Platform/token.md).
- Pass a channel ID that can identify the channel. Users that input the same channel ID enter into the same channel.
- Pass a UID that can identify the user. Every user in the channel requires a unique UID. If you want to join the same channel on different devices, ensure that different UIDs are used for each device.

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

### Ending a Call

When a voice call ends, call the `leaveChannel` method to leave the channel.

This method releases all resources related to the call. When the user leaves the channel, the SDK triggers the  `didLeaveChannelWithStats` callback.

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

You can now start a one-to-one or group voice call. The only difference between a one-to-one and group call is the number of users in the channel.

## Reference

The following list are references to the Agora APIs used on this page in making a voice call:

- Initialize: `sharedEngineWithAppId`
- Set the Channel Profile: `setChannelProfile`
- Join a Channel: `joinChannelByToken`
- Leave a Channel: `leaveChannel`

Refer to the following links to add more functions to a voice call:

- [Recording Voice and Video](../../en/Recording/recording_voice_video.md)
- [Using Agora Built-in Encryption](../../en/Voice/encryption_ios_agora.md)
- [Modifying Raw Data](../../en/Voice/rawdata_ios.md)

You can find all the Agora APIs that can be implemented in a video call in [Voice Call API](https://docs.agora.io/en/Voice/API%20Reference/oc/index.html).
