
---
title: 加入频道
description: android平台加入通信频道
platform: Android
updatedAt: Fri Jul 19 2019 09:00:30 GMT+0800 (CST)
---
# 加入频道
在加入频道前，请确保你已完成环境准备、安装包获取等步骤，详见[客户端集成](../../cn/Video/android_video.md)。

## 实现方法

App 在加入频道前，需要先设置频道模式，再加入频道。

### 设置频道模式为通信
创建实例后，调用 `setChannelProfile` 方法设置频道模式。SDK 会根据所设置的频道模式使用不同的优化手段。 

在该方法中，将频道模式设置为通信模式。通信模式适用于语音或视频通话场景，如一对一聊天或群聊。频道中的任何用户都可以自由发言。该模式为默认模式。

> - 该方法必须在加入频道前调用才能生效。
> - 同一频道只能设置一种频道模式。如果需要切换频道模式，请先调用 `destroy` 方法销毁后重新创建一个 Engine 实例，再调用该方法将频道设置为其他模式。

```
mRtcEngine.setChannelProfile(Constants.CHANNEL_PROFILE_COMMUNICATION);
```

### 加入通信频道
调用 `joinChannel` 方法加入频道。

在该方法中：

-   传入能标识用户角色和权限的 Token。测试环境下，你可以使用获取到的临时 Token。生产环境下，我们推荐你使用在自己的服务端生成的正式 Token。关于如何生成正式 Token，详见[校验用户权限](../../cn/Video/token.md)。
-   传入能标识频道的频道 ID。输入相同频道 ID 的用户会进入同一个频道。
-   频道内每个用户的 UID 必须是唯一的。如果将 UID 设为 0，系统将自动分配一个 UID。

> 如果已在频道中，用户必须调用 `leaveChannel` 方法退出当前频道，才能进入下一个频道。

```
 private void joinChannel() {
    mRtcEngine.joinChannel("token", "channel1", "Extra Optional Data", 0); // if you do not specify the uid, Agora will assign one.
}
```

## 相关文档
成功加入频道后，你可以使用 Agora SDK，实现如下功能进行视频通话：

- [发布和订阅音视频流](../../cn/Video/publish_android.md)

如果在通话过程中，对音量、音效、视频分辨率等有特殊需求，你还可以：

- [调整通话音量](../../cn/Video/volume_android.md)
- [播放音效/音乐混音](../../cn/Video/effect_mixing_android.md)
- [使用耳返](../../cn/Video/in-ear_android.md)
- [调整音调、音色](../../cn/Video/voice_effect_android.md)
- [设置视频属性](../../cn/Video/videoProfile_android.md)
