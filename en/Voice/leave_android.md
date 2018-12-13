
---
title: Leave the Channel
description: 
platform: Android
updatedAt: Thu Dec 13 2018 08:34:34 GMT+0000 (UTC)
---
# Leave the Channel
The `leaveChannel` method allows a user to leave a channel and releases all resources related to the call or live broadcast. When the user leaves the channel, the SDK triggers the `onLeaveChannel` callback.

```
 private void leaveChannel() {
    mRtcEngine.leaveChannel();
}
```

> If the `destroy` method is called immediately after the `leaveChannel` method, the `leaveChannel` process will be interrupted, and the SDK will not trigger the `onLeaveChannel` callback.
