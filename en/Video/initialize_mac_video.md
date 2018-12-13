
---
title: Create and Initialize an AgoraRtcEngine Instance
description: 
platform: macOS
updatedAt: Thu Dec 13 2018 22:38:24 GMT+0000 (UTC)
---
# Create and Initialize an AgoraRtcEngine Instance
Before creating an AgoraRtcEngine instance, ensure that you prepared the development environment. See [Integrate the SDK](../../en/Video/mac_video.md).

## Implementation
Create an AgoraRtcEngine instance by invoking `sharedEngineWithAppId` before joining a live broadcast channel.

In this method:

- Pass the Agora App ID. Only apps with the same App ID can join the same channel.
- Specify a delegate object. The Agora SDK uses the delegate object to inform the app of Agora engine runtime events, such as joining or leaving a channel and adding new users into the channel.

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

## Next Steps
You have created the AgoraRtcEngine instance and can start a video call with the following steps:

* [Join a Channel](../../en/Video/join_video_mac.md)
* [Publish and Subscribe to Streams](../../en/Video/publish_mac.md)

To check the network connection or audio quality before joining a channel, you can refer to the following sections:

* [Conduct a Last mile Test](../../en/Video/lastmile_ios.md)
* [Set the Stereo/High-fidelity Audio Profile](../../en/Video/audio_profile_mac.md)
