
---
title: Leave the Channel
description: 
platform: macOS
updatedAt: Tue Oct 22 2019 06:18:47 GMT+0800 (CST)
---
# Leave the Channel
When a call or live broadcast ends, use the Agora SDK to leave the channel.

## Implementation
When a call or live broadcast ends, call the `leaveChannel` method to leave the channel.

The `leaveChannel` method releases all resources related to the call or live broadcast. When the user leaves the channel, the SDK triggers the  `didLeaveChannelWithStats` callback.

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

> If the `destroy` method is called immediately after the `leaveChannel` method, the `leaveChannel` process will be interrupted, and the SDK will not trigger the  `didLeaveChannelWithStats` callback.


## Next Steps
You have integrated basic communication or live broadcast into your app. For advanced functions, see the sections under **Advanced Guide**.

If you encounter any problem integrating or using the Agora SDK, refer to the following sections or submit a ticket at [Agora Console](https://dashboard.agora.io).

- [General Questions](../../en/Agora%20Platform/general_questions.md)
- [Integration and Deployment](../../en/Agora%20Platform/general_questions.md)
- [Troubleshooting](../../en/Agora%20Platform/general_questions.md)
