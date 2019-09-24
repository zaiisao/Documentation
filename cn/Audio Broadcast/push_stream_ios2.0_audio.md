
---
title: 推流到 CDN
description: 
platform: macOS,iOS
updatedAt: Tue Sep 24 2019 05:39:39 GMT+0800 (CST)
---
# 推流到 CDN
## 功能描述

将直播流发布到 CDN（Content Delivery Network）的过程称为 CDN 直播推流，用户无需安装 App，可以通过 Web 浏览器观看直播。

在推流到 CDN 过程中，当频道中有多个主播时，通常会涉及到[转码](https://docs.agora.io/cn/Agora%20Platform/terms?platform=All%20Platforms#转码)，将多个直播流组合成单个流，并设置这个流的音视频属性和合图布局。



推流实现原理如下：

<img alt="../_images/live_ios_publishing_stream_cn.png" src="https://web-cdn.agora.io/docs-files/cn/live_ios_publishing_stream_cn.png"/>

## 前提条件

请确保在使用该功能前已联系 [sales@agora.io](mailto:sales@agora.io) 开通旁路推流功能。

## 实现方法

声网提供的 CDN 旁路推流方案主要基于以下 API 进行推流、转码设置：

- [`setLiveTranscoding`](https://docs.agora.io/cn/Audio%20Broadcast/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/setLiveTranscoding:)：配置直播转码参数
- [`addPublishStreamUrl`](https://docs.agora.io/cn/Audio%20Broadcast/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/addPublishStreamUrl:transcodingEnabled:)：添加推流地址
- [`removePublishStreamUrl`](https://docs.agora.io/cn/Audio%20Broadcast/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/removePublishStreamUrl:)：删除推流地址

其中：

- 主播在 `joinChannelByToken` 成功后通过 `setLiveTranscoding` 接口定义转码参数和设置（RTMP 画布大小等信息）。CDN 音频推流时仍需设置一个 16 &times; 16 的最小视窗。
- 主播通过 `addPublishStreamUrl`接口指定推流地址。推流地址可以在开始推流后动态增删。


### 示例代码：

```objective-c
// CDN 转码设置
AgoraLiveTranscodingUser *user = [[AgoraLiveTranscodingUser alloc] init];
user.uid = 12345;
user.rect = CGRectMake(0, 0, 16, 16);
user.audioChannel = 0;

AgoraRtcEngineKit *rtcEngine = [AgoraRtcEngineKit sharedEngineWithAppId:@"" delegate:nil];
AgoraLiveTranscoding *transcoding = [[AgoraLiveTranscoding alloc] init];
transcoding.audioSampleRate = AgoraAudioSampleRateType44100;
transcoding.audioChannels = 2;
transocding.audioBitrate = 48;
transcoding.size = CGSizeMake(16, 16);
transcoding.videoFramerate = 15;
transcoding.videoCodecProfile = AgoraVideoCodecProfileTypeHigh;
transcoding.transcodingUsers = @[user];

[rtcEngine setLiveTranscoding:transcoding];
```

```objective-c
// 添加推流地址
// transcodingEnabled 设置为 YES，表示开启转码。如开启，则必须通过 setLiveTranscoding 接口配置 AgoraLiveTranscoding 类。单主播模式下，我们不建议使用转码
[rtcEngine addPublishStreamUrl:streamUrl transcodingEnabled:YES];
```

```objective-c
// 删除推流地址
[rtcEngine removePublishStreamUrl:streamUrl];
```

## 开发注意事项

使用该功能需要联系 sales@agora.io 开通旁路推流。
