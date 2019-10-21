
---
title: 输入在线媒体流
description: 
platform: Windows
updatedAt: Mon Oct 21 2019 06:00:16 GMT+0800 (CST)
---
# 输入在线媒体流
## 功能描述
输入在线媒体流功能可以将音视频流作为一个发送端导入正在进行的直播房间。通过将正在播放的音视频导入到直播频道中，主播和观众可以一起收听/观看该媒体流并实时互动。

### 常用场景

- 赛事直播中，主播直接拉比赛的音视频流，实现主播和观众边看比赛边点评的功能。
- 同一直播间内，主播与观众一同欣赏电影、音乐、演出，并实时交流讨论。
- 无人机或网络摄像头直接采集视频，该视频作为在线媒体流导入直播频道中。

### 工作原理

![](https://web-cdn.agora.io/docs-files/1569397540228)

直播频道中的主播通过 Video Inject 服务器将在线媒体流拉到 Agora SD-RTN 中，导入到直播频道内。

- 频道内的连麦主播、普通观众都可以听/看到该媒体流。
- 如果主播开启了 CDN 旁路推流，该媒体流也会被推送到 CDN 上， CDN 观众就可以听/看到这路媒体流。

> - 支持的媒体流格式：RTMP、HLS 和 FLV。纯音频流也可作为在线媒体流导入直播频道。
> - 只有主播可以导入/删除在线媒体流，连麦主播、普通观众和 CDN 观众都不可以。

## 实现方法

在实现输入在线媒体流功能前，请确保你已在项目中完成基本的实时音视频功能。详见[开始互动直播](../../cn/Interactive%20Broadcast/start_live_windows.md)。

参考如下步骤，在你的项目中实现输入在线媒体流功能：

1. 频道内主播调用 `addInjectStreamUrl` 方法向直播频道内导入指定在线媒体流。你也可以修改 config 的参数设置媒体流导入的分辨率、码率和帧率等参数，详见[`InjectStreamConfig`](https://docs.agora.io/cn/Interactive%20Broadcast/API%20Reference/cpp/structagora_1_1rtc_1_1_inject_stream_config.html)。

   > 频道内同一时间只允许输入一个在线媒体流。

   导入媒体流成功后，该媒体流会在直播频道内自动播放，频道内所有用户都会收到 `onUserJoined` (`uid`: 666) 回调，导入媒体流的主播同时还会收到 `onStreamInjectedStatus` 回调。

   > 如果导入媒体流失败，查阅 [API 参考](#api)排查问题。

2. 频道内主播调用 `removeInjectStreamUrl` 方法从直播频道内删除指定的已导入在线媒体流。

   删除媒体流成功后，频道内所有用户都会收到  `onUserOffline` (`uid: 666`) 回调。

   > 主播退出频道后，无需再调用 `removeInjectStreamUrl` 接口。



### 示例代码

```c++
// C++
// 导入在线媒体流
const char* urlPath = "Some online RTMP/HLS url path";
  InjectStreamConfig config;
  rtcEngine->addInjectStreamUrl(urlPath, config);

// 删除在线媒体流
const char* urlPath = "The same online RTMP/HLS url path added before";
  rtcEngine->removeInjectStreamUrl(urlPath)
```

同时，我们在 Github 提供一个开源的 [Live-Streaming-Injection](https://github.com/AgoraIO/Advanced-Interactive-Broadcasting/tree/master/Live-Streaming-Injection) 示例项目。

<a name="api"></a>
### API 参考

- [`addInjectStreamUrl`](https://docs.agora.io/cn/Interactive%20Broadcast/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine.html#a42247db589b55d3cfa98d8e1be06d8e6)
- [`removeInjectStreamUrl`](https://docs.agora.io/cn/Interactive%20Broadcast/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine.html#aff904ee7a5f0a9741d9cead45249f3cf)
- [`onStreamInjectedStatus`](https://docs.agora.io/cn/Interactive%20Broadcast/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine_event_handler.html#aa34d75a2d01f4ad5297f79f1b1bf3c1d)
- [`onUserJoined`](https://docs.agora.io/cn/Interactive%20Broadcast/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine_event_handler.html#a985a675469baf9d54feb8781732e0ca8)
- [`onUserOffline`](https://docs.agora.io/cn/Interactive%20Broadcast/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine_event_handler.html#a9602593c05a16eafa7c094aa330c0719)

## 开发注意事项

主播在直播过程中启用输入在线媒体流时，观众需要订阅主播才能观看外部视频。
