
---
title: 加入频道
description: windows平台加入频道
platform: Windows
updatedAt: Thu Jul 18 2019 11:31:24 GMT+0800 (CST)
---
# 加入频道
## 前提条件

在加入频道前，请确保你已完成准备、安装包获取等步骤，详见[客户端集成](../../cn/Voice/windows_video.md)。

为方便体验，Agora 支持在 Dashboard 的项目详情页，生成一个试用的临时 Token。在项目详情处，点击**生成临时 Token**，输入频道名，你就会在 **Token** 页面获取一个临时 Token。

![](https://web-cdn.agora.io/docs-files/1562926292439)

![](https://web-cdn.agora.io/docs-files/1562926303571)

## 实现方法
App 在加入频道前，需要先设置频道模式，再加入频道。

### 设置频道模式为通信
初始化实例后，调用 <code>setChannelProfile</code> 方法设置频道模式。SDK 会根据所设置的频道模式使用不同的优化手段。

在该方法中，将频道模式设置为通信模式。通信模式适用于语音或视频通话场景，如一对一聊天或群聊。频道中的任何用户都可以自由发言。该模式为默认模式。
 
> - 该方法必须在加入频道前调用才能生效。
> - 同一频道只能设置一种频道模式。如果需要切换频道模式，请先调用 `release` 方法销毁后重新创建一个 AgoraRtcEngine 实例，再调用该方法将频道设置为其他模式。


```
nRet = m_lpAgoraEngine->setChannelProfile(CHANNEL_PROFILE_COMMUNICATION);
```

### 加入通信频道
调用 <code>joinChannel</code> 方法加入频道。

在该方法中：

-   传入能标识用户角色和权限的 Token。测试环境下，你可以使用获取到的临时 Token。生产环境下，我们推荐你使用在自己的服务端生成的正式 Token。关于如何生成正式 Token，详见[校验用户权限](../../cn/Voice/token.md)。
-   传入能标识频道的频道 ID。输入相同频道 ID 的用户会进入同一个频道。
-   传入能标识用户身份的用户 UID。请确保频道内每个用户的 UID 必须是独一无二的。

> 如果已在通话中，则必须调用 <code>leaveChannel</code> 方法退出当前通话，才能进入下一个频道。

```
LPCSTR lpStreamInfo = "{\"owner\":true,\"width\":640,\"height\":480,\"bitrate\":500}";
nRet = m_lpAgoraEngine->joinChannel(lpToken, lpChannelName, lpStreamInfo, nUID);
```


## 相关文档
成功加入频道后，你可以使用 Agora SDK，实现如下功能进行语音通话：

- [发布和订阅音频流](../../cn/Voice/publish_windows_audio.md)

如果在通话过程中，对音量、音效、音调等有特殊需求，你还可以：

- [调整通话音量](../../cn/Voice/volume_windows.md)
- [播放音效/音乐混音](../../cn/Voice/effect_mixing_windows.md)
- [调整音调、音色](../../cn/Voice/voice_effect_windows.md)

