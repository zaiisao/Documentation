
---
title: 实现语音通话
description: 
platform: Android
updatedAt: Fri Nov 02 2018 04:00:01 GMT+0800 (CST)
---
# 实现语音通话
在本页你可以了解如何使用 Agora SDK 实现语音通话。

## 准备环境

1.  关于具体的开发环境要求、如何获取 App ID 以及 SDK 集成方法，详见 [设置开发环境](../../cn/Quickstart%20Guide/android_audio.md) 。

2.  参考 [Android Voice Tutorial](https://github.com/AgoraIO/Basic-Audio-Call/tree/master/One-to-One-Voice/Agora-Android-Voice-Tutorial-1to1) 了解如何从头创建一个实例项目。


## 快速开始

### 创建实例

导入以下 Agora API 包:

-   `io.agora.rtc.Constants`
-   `io.agora.rtc.IRtcEngineEventHandler`
-   `io.agora.rtc.RtcEngine`


进入语音通话频道之前，调用 `RtcEngine.create` 创建一个实例。在该方法中:

-   填入 [设置开发环境](../../cn/Quickstart%20Guide/android_audio.md) 里获取的 App ID。只有 App ID 相同的应用程序才能进入同一个频道进行互通。
-   指定一个事件回调。SDK 通过指定的事件通知应用程序 SDK 的运行事件，如: 加入或离开频道，新用户加入频道等。


```
import io.agora.rtc.Constants;
import io.agora.rtc.IRtcEngineEventHandler;
import io.agora.rtc.RtcEngine;

...

private void initializeAgoraEngine() {
    try {
        mRtcEngine = RtcEngine.create(getBaseContext(), getString(R.string.agora_app_id), mRtcEventHandler);
    } catch (Exception e) {
        Log.e(LOG_TAG, Log.getStackTraceString(e));

        throw new RuntimeException("NEED TO check rtc sdk init fatal error\n" + Log.getStackTraceString(e));
    }
}
```

### 设置频道模式

创建实例后，调用 `setChannelProfile` 方法设置频道模式。SDK 会根据所设置的频道模式使用不同的优化手段。 

在该方法中，将频道模式设置为通信模式。通信模式适用于语音或视频通话场景，如一对一聊天或群聊。频道中的任何用户都可以自由发言。该模式为默认模式。

> - 该方法必须在加入频道前调用才能生效。
> - 同一频道只能设置一种频道模式。如果需要切换频道模式，请先调用 `destroy` 方法销毁后重新创建一个 Engine 实例，再调用该方法将频道设置为其他模式。

```
mRtcEngine.setChannelProfile(Constants.CHANNEL_PROFILE_COMMUNICATION);
```

### 加入频道

现在你可以加入语音通话频道了。调用 `joinChannel` 方法加入频道。同一个频道内的用户可以互相通话；多个用户加入同一个频道，可以群聊。

-   传入能标识用户角色和权限的 Token。如果安全要求不高，也可以将值设为 null。Token 需要在应用程序的服务器端生成，具体生成办法，详见 [密钥说明](../../cn/Agora%20Platform/token.md)。
-   传入能标识频道的频道 ID。输入相同频道 ID 的用户会进入同一个频道。
-   传入能标识用户身份的用户 UID。请确保频道内每个用户的 UID 必须是独一无二的。如果想要从不同的设备同时接入同一个频道，请确保每个设备上使用的 UID 是不同的。

> 如果已在通话中，用户必须调用 `leaveChannel` 方法退出当前通话，才能进入下一个频道。


```
 private void joinChannel() {
    mRtcEngine.joinChannel(null, "demoChannel1", "Extra Optional Data", 0); // if you do not specify the uid, Agora will assign one.
}
```

### 结束通话

调用 `leaveChannel` 方法离开频道，挂掉或结束通话。

不论当前是否还在通话中，调用该方法会把会话相关的所有资源释放掉。`mRtcEngine.leaveChannel` 并不会直接让用户离开频道。调用该方法后 SDK 会触发 `onLeaveChannel` 回调。

```
 private void leaveChannel() {
    mRtcEngine.leaveChannel();
}
```

> 如果在调用 `leaveChannel` 方法后立即使用 `destroy`，则退出频道会被打断，SDK 也不会触发 `onLeaveChannel` 回调。

### 结论

现在你可以开始一个一对一通话或多人群聊了，两者唯一的区别在于频道内人数的差别。

## 参考

在上述通话过程中，我们使用了 Agora SDK 提供的部分 API 功能：

-   创建 RtcEngine 对象：`create`
-  设置频道属性： `setChannelProfile`
-   加入频道：`joinChannel`
-   离开频道：`leaveChannel`


在实现视频通话的过程中，你还可能需要使用以下功能:

-   [录制音视频](../../cn/Quickstart%20Guide/recording_voice_video.md)

-   [选择加密方案](../../cn/Quickstart%20Guide/encryption_android_agora.md)

-   [修改裸数据](../../cn/Quickstart%20Guide/rawdata_android.md)


更多功能实现，请参考 [语音通话 API](https://docs.agora.io/cn/Voice/API%20Reference/java/index.html) 中各 API 的功能及描述。


