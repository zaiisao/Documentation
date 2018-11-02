
---
title: Leave the Channel
description: 
platform: iOS
updatedAt: Thu Nov 01 2018 09:08:01 GMT+0000 (UTC)
---
# Leave the Channel
# Leave the Channel
When a call or live broadcast ends, call the `leaveChannel` method to leave the channel.

This method releases all resources related to the call or live broadcast. When the user leaves the channel, the SDK triggers the  `didLeaveChannelWithStats` callback.

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

