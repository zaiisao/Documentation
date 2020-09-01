
---
title: 自定义视频采集和渲染
description: How to use external audio/video sources for Web SDK
platform: Web
updatedAt: Tue Sep 01 2020 07:22:58 GMT+0800 (CST)
---
# 自定义视频采集和渲染
## 功能介绍

实时音视频传输过程中，Agora SDK 通常会启动默认的音视频模块进行采集和渲染。在以下场景中，你可能会发现默认的音视频模块无法满足开发需求：

- app 中已有自己的音频或视频模块
- 希望使用非 Camera 采集的视频源，如录屏数据
- 需要使用自定义的美颜库或有前处理库
- 某些视频采集设备被系统独占。为避免与其它业务产生冲突，需要灵活的设备管理策略

本文介绍如何使用 Agora Web SDK 在项目中实现自定义的视频源和渲染器。


<div class="alert info">点击<a href="https://webdemo.agora.io/agora-web-showcase/examples/Agora-Custom-VideoSource-Web/">在线体验</a>试用自定义视频源功能。</div>

## 实现方法

在开始自定义视频源和渲染器前，请确保你已在项目中实现了基本的音视频通话或直播功能，详见[开始音视频通话](../../cn/Interactive%20Broadcast/start_call_web.md)或[开始互动直播](../../cn/Interactive%20Broadcast/start_live_web.md)。

### 自定义音视频源

在创建音视频流 `createStream`  时，通过  [`audioSource`](https://docs.agora.io/cn/Interactive%20Broadcast/API%20Reference/web/interfaces/agorartc.streamspec.html#audiosource) 和  [`videoSource`](https://docs.agora.io/cn/Interactive%20Broadcast/API%20Reference/web/interfaces/agorartc.streamspec.html#videosource) 指定自定义的音视频源。例如，你可以通过 `mediaStream` 方法从 `MediaStreamTrack` 获得音视频 track，然后指定 `audioSource` 和 `videoSource`，如下所示：

```javascript
navigator.mediaDevices.getUserMedia(
    {video: true, audio: true}
).then(function(mediaStream){
    var videoSource = mediaStream.getVideoTracks()[0];
    var audioSource = mediaStream.getAudioTracks()[0];
    // After processing videoSource and audioSource
    var localStream = AgoraRTC.createStream({
        video: true,
        audio: true,
        videoSource: videoSource,
        audioSource: audioSource
    });
    localStream.init(function(){
        client.publish(localStream, function(e){
            //...
        });
    });
});
```

<div class="alert info"><code>MediaStreamTrack</code> 对象是指浏览器原生支持的 <code>MediaStreamTrack</code> 对象，具体用法和浏览器支持状况请参考 <a href="https://developer.mozilla.org/en-US/docs/Web/API/MediaStreamTrack">MediaStreamTrack API 说明</a>。</div>


### 自定义渲染器

你可以调用 [`Stream.getVideoTrack`](https://docs.agora.io/cn/Interactive%20Broadcast/API%20Reference/web/interfaces/agorartc.stream.html#getvideotrack)  方法，然后将视频 track 导到 canvas 里自己渲染。

### 示例代码

我们在 GitHub 提供一个开源的 [Agora-Custom-VideoSource-Web-Webpack](https://github.com/AgoraIO/Advanced-Video/tree/master/Web/Agora-Custom-VideoSource-Web-Webpack) 示例项目。你可以下载体验，或查看 [rtc-client.js](https://github.com/AgoraIO/Advanced-Video/blob/master/Web/Agora-Custom-VideoSource-Web-Webpack/src/rtc-client.js) 文件中的源代码。
