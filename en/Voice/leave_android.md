
---
title: Leave the Channel
description: 
platform: Android
updatedAt: Thu Dec 13 2018 08:34:50 GMT+0000 (UTC)
---
# Leave the Channel
When a call or live broadcast ends, use the Agora SDK to leave the channel.

## Implementation
The `leaveChannel` method allows a user to leave a channel and releases all resources related to the call or live broadcast. When the user leaves the channel, the SDK triggers the `onLeaveChannel` callback.

```
 private void leaveChannel() {
    mRtcEngine.leaveChannel();
}
```

> If the `destroy` method is called immediately after the `leaveChannel` method, the `leaveChannel` process will be interrupted, and the SDK will not trigger the `onLeaveChannel` callback.

## Next Steps
You hava now integrated basic communication or live broadcast into your app, and are ready to experience more advanced and complex functions following articles listed under **Advanced Guide**.

If you encounter any problem integrating or using the Agora SDK, refer to the following documents or file a Ticket at [Agora Dashboard](https://dashboard.agora.io).

- [General Asked Questions](../../en/Agora%20Platform/general_questions.md)
- [Integration and Deployment](../../en/Agora%20Platform/general_questions.md)
- [Trouble Shooting](../../en/Agora%20Platform/general_questions.md)
