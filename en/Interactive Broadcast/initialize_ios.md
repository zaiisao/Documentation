
---
title: Initialize AgoraRtcEngine
description: 
platform: iOS
updatedAt: Thu Nov 01 2018 09:00:27 GMT+0000 (UTC)
---
# Initialize AgoraRtcEngine
# Initialize AgoraRtcEngine
Create an AgoraRtcEngine instance by invoking `sharedEngineWithAppId` before joining a live broadcast channel.

In this method:

- Pass the Agora App ID. Only applications with the same App ID can join the same channel.
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
