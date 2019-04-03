
---
title: 推流到 CDN
description: 
platform: Windows
updatedAt: Wed Apr 03 2019 03:46:34 GMT+0000 (UTC)
---
# 推流到 CDN
## 功能描述

旁路推流功能用于将主播的上行音频流转化为 RTMP 流分发，供 Web 端或流媒体播放器端收听。

> 请联系 [sales@agora.io](mailto:sales@agora.io) 开通推流功能。


声网提供的 CDN 旁路推流方案主要基于以下 API 进行推流、外部输入音频源、转码和布局设置：

-   [`addPublishStreamUrl`](https://docs.agora.io/cn/Audio%20Broadcast/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine.html#a5d62a13bd8391af83fb4ce123450f839)
-   [`removePublishStreamUrl`](https://docs.agora.io/cn/Audio%20Broadcast/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine.html#a30e6c64cb616fbd78bedd8c516c320e7)
-   [`setLiveTranscoding`](https://docs.agora.io/cn/Audio%20Broadcast/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine.html#a0601e4671357dc1ec942cccc5a6a1dde)


声网的 CDN 旁路推流方案具有以下优点：

-   能够随时启动或停止推流

-   增加了控制信息

-   能够在不间断推流的同时增减推流地址

-   通过回调接口掌握推流成功与否

-   较少的接口便于客户快速升级。

## 推流到 CDN

您需要联系 [sales@agora.io](mailto:sales@agora.io) 开通推流功能。

> 声网今后将在 Dashboard 提供自助服务。

> -   主播需要通过 <code>addPublishStreamUrl</code> 接口指定推流地址。推流地址可以在开始推流后动态增删。

> -   主播需要在 <code>joinChannel</code> 成功后通过 <code>setLiveTranscoding</code> 接口定义转码参数和设置（RTMP 画布大小等信息）。CDN 音频推流时仍需设置一个 16 &times; 16 的最小视窗。


推流架构图如下：

<img alt="../_images/live_ios_publishing_stream_cn.png" src="https://web-cdn.agora.io/docs-files/cn/live_ios_publishing_stream_cn.png"/>



### 示例代码：

```
//CDN 转码设置
LiveTranscoding config;
config.audioSampleRate = TYPE_44100;
config.audioChannels = 2;
//config.audioBitrate
config.width = 16;
config.height = 16;
config.videoFramerate = 15;
config.videoCodecProfile = HIGH;

//分配用户视窗的合图布局
config.transcodingUser = new TranscodingUser[config.userCount];
config.transcodingUser[0].uid = 123456; //uid 应和 joinChannel() 输入的 uid 保持一致
config.transcodingUser[0].x = 0;
config.transcodingUser[0].audioChannel = 0;
config.transcodingUser[0].y = 0;
config.transcodingUser[0].width = 16;
config.transcodingUser[0].height = 16;

m_rtcEngine->setLiveTranscoding(config);
// 添加推流地址
m_rtcEngine->addPublishStreamUrl(url, true);

 if (config.transcodingUsers)
   {
      delete[] config.transcodingUsers;
   }
```

```
// 添加推流地址
m_rtcEngine->addPublishStreamUrl(url, false);
```

```
// 删除推流地址
m_rtcEngine->removePublishStreamUrl(url);
```
