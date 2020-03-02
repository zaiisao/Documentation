
---
title: 视频流回退
description: 
platform: Web
updatedAt: Mon Mar 02 2020 08:02:11 GMT+0800 (CST)
---
# 视频流回退
## 功能描述

网络不理想的环境下，直播音视频的质量都会下降。为提升直播效率，Agora 新增了 `setStreamFallbackOption` 接口。用户设置这个接口后，在网络条件差、无法同时保证音视频质量的情况下，SDK 会自动将视频流从大流切换为小流，或将媒体流回退为音频流，从而保证或提高音频质量。

## 实现方法

在实现视频流回退机制前，请确保已在你的项目中实现基本的音视频功能。详见[开始互动直播](../../cn/Interactive%20Broadcast/start_live_web.md)。

参考如下步骤，在你的项目中实现视频流回退功能：

1. 调用 `Stream.init` 成功后，主播调用 `Client.enableDualStream` 方法开启[双流模式](https://docs.agora.io/cn/Agora%20Platform/terms?platform=All%20Platforms#a-name-duala双流模式)。

   > 建议不要对双流进行 track 操作（包括 [addTrack](https://docs.agora.io/cn/Interactive%20Broadcast/API%20Reference/web/interfaces/agorartc.stream.html#addtrack)/[removeTrack](https://docs.agora.io/cn/Interactive%20Broadcast/API%20Reference/web/interfaces/agorartc.stream.html#removetrack)/[replaceTrack](https://docs.agora.io/cn/Interactive%20Broadcast/API%20Reference/web/interfaces/agorartc.stream.html#replacetrack)），否则会导致大流和小流的表现不一致。

2. 频道内用户调用 `Client.setStreamFallbackOption` 方法设置弱网环境下接收媒体流的回退选项。

   - 设置 `fallbackType (1)`，则下行网络较弱时，只接收主播的视频小流。
   - 设置 `fallbackType (2)`，则下行网络较弱时，先尝试只接收主播的视频小流。如果网络环境无法显示视频，则只接收主播的音频流。

3. （可选）频道内用户调用 `Client.setRemoteVideoStreamType` 方法，并设置 `streamType (1)` 让接收端只接收视频小流。

用户接收到的流从媒体流切换到音频流，或从音频流切换到媒体流时，SDK 会触发 `Client.on("stream-fallback")` 回调，该用户可以了解当前接收流的状态。当接收到的流从大流切换到小流，或从小流切换到大流时，SDK 会触发 `Client.on("stream-type-changed")` 回调，该用户可以了解当前接收到的视频流类型。

### 示例代码

```javascript
//Javascript
// 开启视频双流模式。
client.enableDualStream(function() {
console.log("Enable dual stream success!")
}, function(err) {
console,log(err)
})

// 接收端的配置。弱网环境下先尝试接收小流；若当前网络环境无法显示视频，则只接收音频流。
client.setStreamFallbackOption(remoteStream, 2)

// 接收端的配置。弱网情况下为保证通话质量，将订阅的视频大流手动切换成视频小流。
client.setRemoteVideoStreamType(remoteStream, 1);
```

### API 参考

- [`enableDualStream`](https://docs.agora.io/cn/Interactive%20Broadcast/API%20Reference/web/interfaces/agorartc.client.html#enabledualstream)
- [`setStreamFallbackOption`](https://docs.agora.io/cn/Interactive%20Broadcast/API%20Reference/web/interfaces/agorartc.client.html#setstreamfallbackoption)
- [`setRemoteVideoStreamType`](https://docs.agora.io/cn/Interactive%20Broadcast/API%20Reference/web/interfaces/agorartc.client.html#setremotevideostreamtype)

### 开发注意事项

-  `enableDualStream` 方法对以下场景无效：
   - 使用自采集属性（[`audioSource`](https://docs.agora.io/cn/Interactive%20Broadcast/API%20Reference/web/interfaces/agorartc.streamspec.html#audiosource) 和 [`videoSource`](https://docs.agora.io/cn/Interactive%20Broadcast/API%20Reference/web/interfaces/agorartc.streamspec.html#videosource)）创建的流
   - 音频通话 (audio: true, video: false)
   - iOS 上的 Safari 浏览器
   - 共享屏幕的场景

- 调用 `setRemoteVideoStreamType` 方法时，某些浏览器对双流模式不完全适配。目前发现的适配问题有：
   - macOS Safari：大小流帧率、分辨率一致
   - Firefox：小流帧率固定为 30 fps
   - iOS Safari：Safari 11 当前不支持大小流切换
