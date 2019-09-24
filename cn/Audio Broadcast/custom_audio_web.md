
---
title: 自定义音频采集
description: How to use external audio sources for Web SDK
platform: Web
updatedAt: Tue Sep 24 2019 08:17:03 GMT+0800 (CST)
---
# 自定义音频采集
## 功能介绍

实时通信过程中，Agora Web SDK 通常会启动浏览器默认的音频模块进行采集和渲染。如果想要实现自定义音频采集和渲染，则可以使用自定义的音频源或渲染器，来进行实现。

**自定义采集**主要适用于以下场景：

- 当 SDK 内置的音频源不能满足开发者需求时，比如需要使用自定义的前处理库。
- 开发者的 App 中已有自己的音频模块，为了复用代码，也可以自定义音视频源。

## 实现方法

在开始自定义采集前，请确保你已完成环境准备、安装包获取等步骤，详见[集成客户端](../../cn/Audio%20Broadcast/web_prepare.md)。

### 自定义音源

在创建音频流时，通过  [`audioSource`](https://docs.agora.io/cn/Audio%20Broadcast/API%20Reference/web/interfaces/agorartc.streamspec.html#audiosource) 指定自定义的音频源。例如，你可以通过 `mediaStream` 方法从 `MediaStreamTrack` 获得音频 track，然后指定 `audioSource`，如下所示：

```javascript
navigator.mediaDevices.getUserMedia(
    {video: false, audio: true}
).then(function(mediaStream){
    var audioSource = mediaStream.getAudioTracks()[0];
    // After processing audioSource
    var localStream = AgoraRTC.createStream({
        video: false,
        audio: true,
        audioSource: audioSource
    });
    localStream.init(function(){
        client.publish(localStream, function(e){
            //...
        });
    });
});
```

> - `MediaStreamTrack` 对象是指浏览器原生支持的 `MediaStreamTrack` 对象，具体用法和浏览器支持状况请参考 [MediaStreamTrack API 说明](https://developer.mozilla.org/en-US/docs/Web/API/MediaStreamTrack)。
> - 目前该功能仅支持 Chrome 浏览器。

我们提供自定义音视频采集的示例 demo，详见  [Agora Custom Media Device](https://github.com/AgoraIO/Advanced-Video/tree/master/Custom-Media-Device/Agora-Custom-VideoSource-Web).
