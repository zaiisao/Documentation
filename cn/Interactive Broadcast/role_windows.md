
---
title: 切换用户角色
description: windows平台上设置或切换用户角色
platform: Windows
updatedAt: Thu Dec 13 2018 08:14:07 GMT+0000 (UTC)
---
# 切换用户角色
直播频道分主播和观众两种用户角色。在将频道模式为直播后，调用 <code>setClientRole</code> 方法，并根据需要将用户设置为主播或观众。两者的区别在于：

-   主播：可以收听和发布音视频消息。根据应用程序的实现，还可以与观众互动、指定观众连麦。同一直播频道内，主播只能听到和看到自己以及连麦主播的音视频。
-   观众：只能收听主播的音视频消息。根据应用程序的实现，还可以发布实时文字消息，与主播互动。同一直播频道内，所有观众都能听到和看到主播以及连麦主播的音视频。

```
int nRet = m_lpAgoraEngine->setClientRole(role);
```

该方法在加入频道前后都可以调用：

- 加入直播频道前，调用该方法将用户设置为主播或观众。
- 直播过程中，调用该方法将用户角色由观众切换为主播（上麦），或由主播切换为观众。

以用户 A、B 为例。如果用户 A、B 均以主播身份加入频道：

1. 用户 A 调用 `setClientRole`，将角色设置为主播，然后调用 `joinChannel` 加入频道。

   ```cpp
   //设置用户角色为主播
   int nRet = m_lpAgoraEngine->setClientRole(role);
   
   //创建并加入频道
   LPCSTR lpStreamInfo = "{\"owner\":true,\"width\":640,\"height\":480,\"bitrate\":500}";
   nRet = m_lpAgoraEngine->joinChannel(lpDynamicKey, lpChannelName, lpStreamInfo, nUID);
   ```

2. 用户 B 调用 `setClientRole`，将角色设置为主播，然后调用 `joinChannel` 加入频道。A 和 B 现在可以连麦了！

   ```cpp
   //设置用户角色为主播
   int nRet = m_lpAgoraEngine->setClientRole(role);
   
   //创建并加入频道
   LPCSTR lpStreamInfo = "{\"owner\":true,\"width\":640,\"height\":480,\"bitrate\":500}";
   nRet = m_lpAgoraEngine->joinChannel(lpDynamicKey, lpChannelName, lpStreamInfo, nUID);
   ```

如果 A 以主播身份加入频道，B 以默认的观众身份加入频道，一段时间后  B 想要连麦：

1. 用户 A 调用 `setClientRole`，将角色设置为主播，然后调用 `joinChannel` 加入频道。

   ```cpp
   //设置用户角色为主播
   int nRet = m_lpAgoraEngine->setClientRole(role);
   
   //创建并加入频道
   LPCSTR lpStreamInfo = "{\"owner\":true,\"width\":640,\"height\":480,\"bitrate\":500}";
   nRet = m_lpAgoraEngine->joinChannel(lpDynamicKey, lpChannelName, lpStreamInfo, nUID);
   ```

2. 用户 B 调用 `joinChannel`，以默认的观众身份加入频道；然后调用 `setClientRole` 将用户角色切换为主播后进行连麦。

	```cpp
	//创建并加入频道
	LPCSTR lpStreamInfo = "{\"owner\":true,\"width\":640,\"height\":480,\"bitrate\":500}";
	nRet = m_lpAgoraEngine->joinChannel(lpDynamicKey, lpChannelName, lpStreamInfo, nUID);

	//设置用户角色为主播
	int nRet = m_lpAgoraEngine->setClientRole(role);
	```
