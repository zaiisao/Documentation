
---
title: Starting a Live Voice Broadcast
description: 
platform: macOS
updatedAt: Fri Nov 02 2018 04:08:30 GMT+0800 (CST)
---
# Starting a Live Voice Broadcast
In this quickstart, you will learn how to use the Agora SDK to start a live voice broadcast.

## Prerequisites

1. See [Setting up Your Development Environment](../../en/Quickstart%20Guide/mac_video.md) for the prerequisites, environment setup, and SDK integration guide.
2. See [Agora macOS Live Sample App](https://github.com/AgoraIO/Basic-Video-Broadcasting/tree/master/OpenLive-macOS) to better understand how a Sample App is created from scratch.

## Quickstart

### Initializing AgoraRtcEngine

Create an AgoraRtcEngine instance by invoking `sharedEngineWithAppId` before joining a live broadcast channel.

In this method:

- Pass the [Agora App ID](../../en/Quickstart%20Guide/ios_audio.md). Only applications with the same App ID can join the same channel.
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

In this method, set the channel profile as Live Broadcast. This profile applies to an interactive broadcast scenario. Each channel includes two roles: the host and the audience. The host\(broadcaster\) sends and receives audio streams while the audience can only receive the audio streams.

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

- Host\(Broadcaster\): A host receives and publishes the voice streams. The host can also interact with other hosts using voice.
- Audience: An audience receives the voice streams of the host\(s\).

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

### Joining a Channel

Now, you can join a live broadcast channel. Call the `joinChannelByToken` method to join a channel.  Hosts in the same channel can talk to and see each other, and audiences in the same channel can hear and see the hosts.

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

You can now start a voice live broadcast.

## Reference

Followings are part of the Agora APIs that we use to start a voice live broadcast:

- Initialize: `sharedEngineWithAppId`
- Set the Channel Profile: `setChannelProfile`
- Set the User Role: `setClientRole`
- Join a Channel: `joinChannelByToken`
- Leave a Channel: `leaveChannel`

Refer to the following links to add more functions to a live broadcast:

- [Hosting In](../../en/Quickstart%20Guide/hostin_mac.md)
- [Recording Voice and Video](../../en/Quickstart%20Guide/recording_voice_video.md)
- [Using Agora Built-in Encryption](../../en/Quickstart%20Guide/encryption_mac_agora.md)
- [Implementing Encryption](../../en/Quickstart%20Guide/encryption_ios_agora.md)
- [Modifying Raw Data](../../en/Quickstart%20Guide/rawdata_mac.md)

You can find all the Agora APIs that can be implemented in a live broadcast in [Interactive Broadcast API](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/oc/index.html).
