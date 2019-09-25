
---
title: 视频流回退机制
description: 
platform: Web
updatedAt: Wed Sep 25 2019 09:14:08 GMT+0800 (CST)
---
# 视频流回退机制
## 功能描述

网络不理想的环境下，直播音视频的质量都会下降。为提升直播效率，在发送端通过 `enableDualStream` 开启双流模式的情况下，Agora 提供了两种优化方法：

- 开启音视频流回退。通过 `setStreamFallbackOption` 接口，设置订阅端在弱网情况下音视频流的回退策略：
  - 网络不理想的情况下，将订阅的视频大流自动回退为低分辨率、低码率的视频小流。视频流变化时，会触发 `stream-type-changed` 回调；
  - 网络较弱时，先尝试订阅视频小流，如果网络环境无法显示视频，则再将订阅的视频流自动回退为音频流。流回退或恢复时，触发 `stream-fallback` 回调。
- 设置视频大小流。通过 `setRemoteVideoStreamType` 接口，弱网情况下为保证通话质量，可以将订阅的视频大流手动切换成低分辨率、低码率的视频小流，视频流变化时，会触发 `stream-type-changed` 回调。

## 实现方法

发送端：

```javascript
// 开启视频大小流模式
client.enableDualStream(function() {
console.log("Enable dual stream success!")
}, function(err) {
console,log(err)
})
```

订阅端：

- 订阅成功后，如果开发者希望开启自动音视频流回退，则调用以下方法：

```javascript
// 网络较弱时，先尝试订阅视频小流，如果网络环境无法显示视频，则再回退到只订阅音频流。
client.setStreamFallbackOption(remoteStream, 2)
```

- 如果开发者希望主动控制远端视频的大小流切换，则调用以下方法：

```javascript
// 订阅的视频大流手动切换成低分辨率、低码率的视频小流
client.setRemoteVideoStreamType(remoteStream, 1);
```

### API 参考

- [`enableDualStreamMode`](https://docs.agora.io/cn/Interactive%20Broadcast/API%20Reference/web/interfaces/agorartc.client.html#enabledualstream)
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
