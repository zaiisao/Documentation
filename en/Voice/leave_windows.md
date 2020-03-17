
---
title: Leave the Channel
description: 
platform: Windows
updatedAt: Wed Oct 30 2019 03:24:11 GMT+0800 (CST)
---
# Leave the Channel
When a call or live broadcast ends, use the Agora SDK to leave the channel.

## Implementation

When a call or live broadcast ends, call the <code>leaveChannel</code> method to leave the channel.

The <code>leaveChannel</code> method releases all resources related to the call or live broadcast. When the user leaves the channel, the SDK triggers the <code>onleaveChannel</code> callback.

```
int nRet = m_lpAgoraEngine->leaveChannel();
```

> If the <code>leaveChannel</code> method is called immediately after the <code>release</code> method, the process will be interrupted, and the SDK will not trigger the <code>onleaveChannel</code> callback.


## Next Steps
You have integrated basic communication or live broadcast into your application. For advanced functions, see the sections under **Advanced Guide**.

If you encounter any problem integrating or using the Agora SDK, refer to the following sections or submit a ticket at [Agora Console](https://dashboard.agora.io).

- [General Questions](../../en/Agora%20Platform/general_questions.md)
- [Integration and Deployment](../../en/Agora%20Platform/general_questions.md)
- [Troubleshooting](../../en/Agora%20Platform/general_questions.md)
