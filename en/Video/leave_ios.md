
---
title: Leave the Channel
description: 
platform: iOS
updatedAt: Thu Dec 13 2018 15:43:40 GMT+0000 (UTC)
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
You hava now integrated basic communication or live broadcast into your app, and can experience more advanced and complex functions following the articles listed under **Advanced Guide**.

If you encounter any problem integrating or using the Agora SDK, refer to the following documents or file a Ticket at [Agora Dashboard](https://dashboard.agora.io).

- [General Asked Questions](../../en/Agora%20Platform/general_questions.md)
- [Integration and Deployment](../../en/Agora%20Platform/general_questions.md)
- [Trouble Shooting](../../en/Agora%20Platform/general_questions.md)
