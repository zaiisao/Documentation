
---
title: Join a Channel
description: 
platform: macOS
updatedAt: Fri Nov 02 2018 04:15:15 GMT+0000 (UTC)
---
# Join a Channel
Set the channel profile before the App joins a channel.

## Set the channel profile as live broadcast
After initializing AgoraRtcEngine, call the `setChannelProfile` method to set the channel profile. AgoraRtcEngine applies optimization according to the channel profile.

In this method, set the channel profile as Live Broadcast. This profile applies to an interactive broadcast scenario. Each channel includes two roles: the host and the audience. The host (broadcaster) sends and receives audio and video streams while the audience can only receive the audio streams.

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

## Join a live broadcast channel
Call the `joinChannelByToken` method to join a channel. 

In this method:

- Pass a Token that can identify the role and privilege of the user. Set it as null if the safety requirements are relatively low. A Token is generated at the Server of the application. For how to generate a Token, see [Security Keys](../../en/Interactive%20Broadcast/token.md).
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
