
---
title: 推流到 CDN
description: 
platform: Android
updatedAt: Mon Jun 10 2019 05:47:18 GMT+0800 (CST)
---
# 推流到 CDN
## 功能描述

旁路推流功能用于将主播的上行音频流转化为 RTMP 流分发，供 Web 端或流媒体播放器端收听。

> 请联系 [sales@agora.io](mailto:sales@agora.io) 开通推流功能。


推流架构图如下：

<img alt="../_images/live_ios_publishing_stream_cn.png" src="https://web-cdn.agora.io/docs-files/cn/live_ios_publishing_stream_cn.png"/>


## 前提条件

请确保在使用该功能前已联系 [sales@agora.io](mailto:sales@agora.io) 开通旁路推流功能。

## 实现方法

声网推出的 CDN 旁路推流方案主要基于以下 API 进行推流、转码设置：

-   [`setLiveTranscoding`](https://docs.agora.io/cn/Audio%20Broadcast/API%20Reference/java/classio_1_1agora_1_1rtc_1_1_rtc_engine.html#a3cb9804ae71819038022d7575834b88c)
-   [`addPublishStreamUrl`](https://docs.agora.io/cn/Audio%20Broadcast/API%20Reference/java/classio_1_1agora_1_1rtc_1_1_rtc_engine.html#a4445b4ca9509cc4e2966b6d308a8f08f)
-   [`removePublishStreamUrl`](https://docs.agora.io/cn/Audio%20Broadcast/API%20Reference/java/classio_1_1agora_1_1rtc_1_1_rtc_engine.html#a87b3f2f17bce8f4cc42b3ee6312d30d4)

其中：

- 主播在 `joinChannel` 成功后通过 `setLiveTranscoding` 接口定义转码参数和设置（RTMP 画布大小等信息）。CDN 音频推流时仍需设置一个 16 &times; 16 的最小视窗。
- 主播通过 `addPublishStreamUrl`接口指定推流地址。推流地址可以在开始推流后动态增删。


### 示例代码：

```java
//CDN 转码设置
LiveTranscoding config;
config.audioSampleRate = TYPE_44100;
config.audioChannels = 2;
config.audioBitrate = 48;
config.width = 16;
config.height = 16;
config.videoFramerate = 15;
config.videoCodecProfile = HIGH;

//分配用户视窗的合图布局
LiveTranscoding transcoding = new LiveTranscoding();
LiveTranscoding.TranscodingUser user = new LiveTranscoding.TranscodingUser();
user.uid = 123456;
transcoding.addUser(user);
user.x = 0;
user.audioChannel = 0;
user.y = 0;
user.width = 16;
user.height = 16;

rtcEngine.setLiveTranscoding(transcoding);
```

```
// 添加推流地址
// transcodingEnabled 设置为 true，表示开启转码。如开启，则必须进行配置 LiveTranscoding 类；
rtcEngine.addPublishStreamUrl(url, true);
```

```
// 删除推流地址
rtcEngine.removePublishStreamUrl(url);
```


