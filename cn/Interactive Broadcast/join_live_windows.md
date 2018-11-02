
---
title: 加入频道
description: windows平台加入频道
platform: Windows
updatedAt: Thu Nov 01 2018 08:08:20 GMT+0000 (UTC)
---
# 加入频道
# 加入频道
App 在加入频道前，需要先设置频道模式，再加入频道。

## 设置频道模式为直播
初始化实例后，调用 <code>setChannelProfile</code> 方法设置频道模式。SDK 会根据所设置的频道模式使用不同的优化手段。

在该方法中，将频道模式设置为直播模式。直播模式适用于互动直播场景。频道内有主播和观众两种角色，主播收发音视频消息，观众只收不发。
 
> - 该方法必须在加入频道前调用才能生效。
> - 同一频道只能设置一种频道模式。如果需要切换频道模式，请先调用 `destroy` 方法销毁后重新创建一个 Engine 实例，再调用该方法将频道设置为其他模式。


```
nRet = m_lpAgoraEngine->setChannelProfile(CHANNEL_PROFILE_LIVE_BROADCASTING);
```

## 加入直播频道
调用 <code>joinChannel</code> 方法加入频道。

在该方法中：

-   传入能标识用户角色和权限的 Token。如果安全要求不高，也可以将值设为 null。Token 需要在应用程序的服务器端生成，具体生成办法，详见 [密钥说明](../../cn/Interactive%20Broadcast/token.md)。

-   传入能标识频道的频道 ID。输入相同频道 ID 的用户会进入同一个频道。

-   传入能标识用户身份的用户 UID。请确保频道内每个用户的 UID 必须是独一无二的。

> 如果已在通话中，则必须调用 <code>leaveChannel</code> 方法退出当前通话，才能进入下一个频道。

```
LPCSTR lpStreamInfo = "{\"owner\":true,\"width\":640,\"height\":480,\"bitrate\":500}";
nRet = m_lpAgoraEngine->joinChannel(lpDynamicKey, lpChannelName, lpStreamInfo, nUID);
```

