
---
title: Join a Channel
description: 
platform: iOS
updatedAt: Thu Jul 18 2019 11:45:58 GMT+0800 (CST)
---
# Join a Channel
## Prerequisites

Before joining a channel, ensure that you prepared the development environment. See [Integrate the SDK](../../en/Voice/ios_audio.md).

When working on a test version of your application, you can generate a temporary token at the [Agora Dashboard](https://dashboard.agora.io/) to join a channel. Follow the steps to generate a temporary token:

1. Enable the [App Certificate](../../en/Voice/token.md). 

2. On the **Project Details** page, click **Generate a Temp Token**, enter a channel name, and you will get a temporary token on the **Token** page. 

	![](https://web-cdn.agora.io/docs-files/1563113619615)

	![](https://web-cdn.agora.io/docs-files/1563113643411)

## Implementation
You need to set the channel profile before the app joins a channel.

### Set the channel profile as Communication
After initializing AgoraRtcEngine, call the `setChannelProfile` method to set the channel profile. AgoraRtcEngine applies optimization according to the channel profile.

In the `setChannelProfile` method, set the channel profile as Communication. This profile applies to voice or video calls such as one-to-one or group calls, where all users in the channel can talk freely. The Communication profile is the default setting.

> - Call the `setChannelProfile` method before joining a channel.
> - One engine uses one profile only. If you want to switch to another profile, destroy the current engine using the `destroy` method and create a new engine before calling the `setChannelProfile` method to set the new channel profile.

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

### Join a communication channel
Call the `joinChannelByToken` method to join a channel. 

In the `joinChannelByToken` method:

- Pass a token that identifies the role and privilege of the user. A token is generated at the server of the app. For how to generate a token, see [Security Keys](../../en/Voice/token.md). 

	> When creating a project at [Dashboard](https://dashboard.agora.io/), You can generate a Temp Token. A Temp Token can be used at the testing stage. For the production environment, we recommend using a Token generated at your server.
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
You are in the channel and can start a voice call with the following step:

* [Publish and Subscribe to Streams](../../en/Voice/publish_ios_audio.md)

For more functions, you can refer to the following sections:

* [Adjust the Volume](../../en/Voice/volume_ios_audio.md)
* [Play Audio Effects/Audio Mixing](../../en/Voice/effect_mixing_ios_audio.md)
* [Use In-Ear Monitoring](../../en/Voice/in-ear_ios_audio.md)
* [Adjust the Pitch and Tone](../../en/Voice/voice_effect_ios_audio.md)
