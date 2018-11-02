
---
title: Leave the Channel
description: 
platform: Android
updatedAt: Thu Nov 01 2018 09:07:44 GMT+0000 (UTC)
---
# Leave the Channel
# Leave the Channel
The `leaveChannel` method allows a user to leave a channel.

This method releases all resources related to the call or live broadcast. When the user leaves the channel, the SDK triggers the `onLeaveChannel` callback.

```
 private void leaveChannel() {
    mRtcEngine.leaveChannel();
}
```

> If the `destroy` method is called immediately after `leaveChannel`, the process will be interrupted, and the SDK will not trigger the onLeaveChannel callback.
