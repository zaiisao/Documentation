
---
title: 输入在线媒体流
description: 
platform: Android
updatedAt: Mon Jun 08 2020 02:55:13 GMT+0800 (CST)
---
# 输入在线媒体流
## 功能描述
输入在线媒体流功能可以将音视频流作为一个发送端输入正在进行的直播房间。通过将正在播放的音视频输入到直播频道中，主播和观众可以一起收听/观看该媒体流并实时互动。

### 常用场景

- 赛事直播中，主播直接拉比赛的音视频流，实现主播和观众边看比赛边点评的功能。
- 同一直播间内，主播与观众一同欣赏电影、音乐、演出，并实时交流讨论。
- 无人机或网络摄像头直接采集视频，该视频作为在线媒体流输入直播频道中。

### 工作原理

![](https://web-cdn.agora.io/docs-files/1576059223619)

直播频道中的主播通过 Video Inject 服务器将在线媒体流拉到 Agora SD-RTN 中，输入到直播频道内。

- 频道内的连麦主播、普通观众都可以听/看到该媒体流。
- 如果主播开启了 CDN 旁路推流，该媒体流也会被推送到 CDN 上， CDN 观众就可以听/看到这路媒体流。

> - 频道内同一时间只允许输入一个在线媒体流。
> - 支持的编码格式：音频 AAC，视频 H.264。
> - 纯音频流也可作为在线媒体流输入直播频道。
> - 只有主播可以输入/删除在线媒体流，连麦主播、普通观众和 CDN 观众都不可以。

## 实现方法

在实现输入在线媒体流功能前，请确保你已在项目中完成基本的实时音视频功能。详见[开始互动直播](../../cn/Interactive%20Broadcast/start_live_android.md)。

> 请确保已开通旁路推流的功能，详见[前提条件](../../cn/Interactive%20Broadcast/cdn_streaming_android.md)。

参考如下步骤，在你的项目中实现输入在线媒体流功能：

1. 频道内主播调用 `addInjectStreamUrl` 方法向直播频道内输入指定在线媒体流。你也可以修改 config 的参数设置媒体流输入的分辨率、码率和帧率等参数，详见[`InjectStreamConfig`](https://docs.agora.io/cn/Interactive%20Broadcast/API%20Reference/java/classio_1_1agora_1_1rtc_1_1live_1_1_live_inject_stream_config.html)。

   > 频道内同一时间只允许输入一个在线媒体流。

   输入媒体流成功后，该媒体流会在直播频道内自动播放，频道内所有用户都会收到 `onUserJoined(uid: 666)` 回调，输入媒体流的主播同时还会收到 `onStreamInjectedStatus` 回调。

   > 如果输入媒体流失败，查阅 [API 参考](#api)排查问题。

2. 频道内主播调用 `removeInjectStreamUrl` 方法从直播频道内删除指定的已输入在线媒体流。

   删除媒体流成功后，频道内所有用户都会收到  `onUserOffline(uid: 666)` 回调。

   > 主播退出频道后，无需再调用 `removeInjectStreamUrl` 接口。

### 示例代码

```java
// Java
// 输入在线媒体流
String urlPath = "Some online RTMP/HLS url path"

  LiveInjectStreamConfig config = new LiveInjectStreamConfig();
  config.width = 0;
  config.height = 0;
  config.videoGop = 30;
  config.videoFramerate = 15;
  config.videoBitrate = 400;
  config.audioSampleRate = LiveInjectStreamConfig.AudioSampleRateType.TYPE_48000; 
  config.audioBitrate = 48;
  config.audioChannels = 1;

  rtcEngine.addInjectStreamUrl(urlPath, config);

// 删除在线媒体流
String urlPath = "The same online RTMP/HLS url path added before"
  rtcEngine.removeInjectStreamUrl(urlPath)
```

同时，我们在 GitHub 提供一个开源的 [Live-Streaming-Injection](https://github.com/AgoraIO/Advanced-Interactive-Broadcasting/tree/master/Live-Streaming-Injection) 示例项目。

<a name="api"></a>
### API 参考

- [`addInjectStreamUrl`](https://docs.agora.io/cn/Interactive%20Broadcast/API%20Reference/java/classio_1_1agora_1_1rtc_1_1_rtc_engine.html#a67547508dd8b98318b55d764eb0da311)
- [`removeInjectStreamUrl`](https://docs.agora.io/cn/Interactive%20Broadcast/API%20Reference/java/classio_1_1agora_1_1rtc_1_1_rtc_engine.html#a1bd152dba2c28459ff5202bb8039fb42)
- [`onStreamInjectedStatus`](https://docs.agora.io/cn/Interactive%20Broadcast/API%20Reference/java/classio_1_1agora_1_1rtc_1_1_i_rtc_engine_event_handler.html#a470bb5a47f90705fa3da3e3b6aebb28d)
- [`onUserJoined`](https://docs.agora.io/cn/Interactive%20Broadcast/API%20Reference/java/classio_1_1agora_1_1rtc_1_1_i_rtc_engine_event_handler.html#aa466d599b13768248ac5febd2978c2d3)
- [`onUserOffline`](https://docs.agora.io/cn/Interactive%20Broadcast/API%20Reference/java/classio_1_1agora_1_1rtc_1_1_i_rtc_engine_event_handler.html#a9fbb08177fbc8f74d64044a78aea0dda)

## 开发注意事项

主播在直播过程中启用输入在线媒体流时，观众需要订阅主播才能观看外部视频。
