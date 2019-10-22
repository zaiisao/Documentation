
---
title: Join a Channel
description: 
platform: iOS
updatedAt: Fri Jul 19 2019 09:22:35 GMT+0800 (CST)
---
# Join a Channel
Before joining the channel, ensure that you prepared the development environment. See [Integrate the SDK](../../en/Interactive%20Broadcast/ios_video.md).

## Implementation

### Set the channel profile as Live Broadcast
After initializing AgoraRtcEngine, call the `setChannelProfile` method to set the channel profile. AgoraRtcEngine applies optimization according to the channel profile.

In the `setChannelProfile` method, set the channel profile as Live Broadcast. This profile applies to an interactive broadcast scenario. Each channel includes two roles:

- The host (broadcaster) sends and receives audio and video streams.
- The audience receives audio and video streams.

> - Call the `setChannelProfile` method before joining a channel.
> - One engine uses one profile only. If you want to switch to another profile, destroy the current engine using the `destroy` method and create a new engine before calling the `setChannelProfile` method to set the new channel profile.

```objective-c
//Objective-C
- (void)setChannelProfile() {
  [self.agoraKit setChannelProfile:AgoraChannelProfileLiveBroadcasting]
}
```

```swift
//Swift
func setChannelProfile() {
  agoraKit.setChannelProfile(.liveBroadcasting)
}
```

### Join a live broadcast channel
Call the `joinChannelByToken` method to join a channel. 

In the `joinChannelByToken` method:

- Pass a token that identifies the role and privilege of the user. 
	- For the testing environment, we recommend usign a Temp Token generated on Console. See [Get a Temp Token](../../en/Audio%20Broadcast/token.md).
	- For the production environment, we recommend using a Token generated at your server. For how to generate a token, see [Token Security](../../en/Audio%20Broadcast/token_server.md). 
- Pass a channel ID that identifies the channel. Users with the same channel ID enter into the same channel.
- Pass a uid that identifies the user. Each user in a channel requires a unique uid. If you want to join the same channel on different devices, ensure that different uids are used for each device.

> Once in a call, a user must call the `leaveChannel` method to exit the current call before entering another channel.

```objective-c
//Objective-C
- (void)joinChannel {
  [self.agoraKit joinChannelByToken:"token" channelId:@"demoChannel1" info:nil uid:0 joinSuccess:^(NSString *channel, NSUInteger uid, NSInteger elapsed) {
    // Join channel "demoChannel1"
  }];
}
```

```swift
//Swift
func joinChannel() {
  agoraKit.joinChannel(by Token: "token", channelId: "demoChannel1", info:nil, uid:0){[weak self] (sid, uid, elapsed) -> Void in
      // Join channel "demoChannel1"
  }
}
```

## Next Steps
You are in the channel and can start a live broadcast with the following steps:

- [Switch the Client Role](../../en/Interactive%20Broadcast/role_ios.md)
- [Publish and Subscribe to Streams](../../en/Interactive%20Broadcast/publish_ios_live.md)

For more functions, you can refer to the following sections:

- [Adjust the Volume](../../en/Interactive%20Broadcast/volume_ios.md)
- [Play Audio Effects/Audio Mixing](../../en/Interactive%20Broadcast/effect_mixing_ios.md)
- [Use In-Ear Monitoring](../../en/Interactive%20Broadcast/in-ear_ios.md)
- [Adjust the Pitch and Tone](../../en/Interactive%20Broadcast/voice_effect_ios.md)
- [Set the Video Profile](../../en/Interactive%20Broadcast/videoProfile_ios.md)
