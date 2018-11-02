
---
title: Leave the Channel
description: 
platform: Windows
updatedAt: Fri Nov 02 2018 04:09:13 GMT+0000 (UTC)
---
# Leave the Channel
When a call or live broadcast ends, call the <code>leaveChannel</code> method to leave the channel.

This method releases all resources related to the call or live broadcast. When the user leaves the channel, the SDK triggers the <code>onleaveChannel</code> callback.

```
int nRet = m_lpAgoraEngine->leaveChannel();
```

> If the <code>leaveChannel</code> method is called immediately after <code>release</code>, the process will be interrupted, and the SDK will not trigger the <code>onleaveChannel</code> callback.

