
---
title: Leave the Channel
description: 
platform: Android
updatedAt: Thu Dec 13 2018 15:35:55 GMT+0000 (UTC)
---
# Leave the Channel
Use the Agora SDK to leave the channel when a call or live broadcast ends.

## Implementation
The `leaveChannel` method allows a user to leave a channel and releases all resources related to the call or live broadcast. When the user leaves the channel, the SDK triggers the `onLeaveChannel` callback.

```
 private void leaveChannel() {
    mRtcEngine.leaveChannel();
}
```

> If the `destroy` method is called immediately after the `leaveChannel` method, the `leaveChannel` process will be interrupted, and the SDK will not trigger the `onLeaveChannel` callback.

## Next Steps
You have now integrated basic communication or live broadcast into your app. For advanced functions, see the sections under **Advanced Guide**.

If you encounter any problem integrating or using the Agora SDK, refer to the following sections or submit a ticket at [Agora Dashboard](https://dashboard.agora.io).

- [General Asked Questions](../../en/Agora%20Platform/general_questions.md)
- [Integration and Deployment](../../en/Agora%20Platform/general_questions.md)
- [Trouble Shooting](../../en/Agora%20Platform/general_questions.md)
