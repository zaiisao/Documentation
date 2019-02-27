
---
title: 推流到 CDN
description: 
platform: iOS,macOS
updatedAt: Wed Feb 27 2019 07:01:35 GMT+0000 (UTC)
---
# 推流到 CDN
## 功能描述

旁路推流功能用于将主播的上行音视频流转化为 RTMP 流分发，供 Web 端或流媒体播放器端观看。

> 请联系 [sales@agora.io](mailto:sales@agora.io) 开通推流功能。


声网提供的 CDN 旁路推流方案主要基于以下 API 进行推流、外部输入视频源、转码和布局设置：

- `addPublishStreamUrl`
- `removePublishStreamUrl`
- `setLiveTranscoding`

声网的 CDN 推流方案具有以下优点：

- 能够随时启动或停止推流
- 增加了控制信息
- 能够在不间断推流的同时增减推流地址
- 通过回调接口掌握推流成功与否
- 较少的接口便于客户快速升级。


## 推流到 CDN

你需要联系 [sales@agora.io](mailto:sales@agora.io) 开通推流功能。

> 声网今后将在 Dashboard 提供自助服务。

- 主播需要通过 `addPublishStreamUrl` 接口指定推流地址。推流地址可以在开始推流后动态增删。
- 主播需要在 `joinChannel` 成功后通过 `setLiveTranscoding` 接口定义转码参数和设置（RTMP 画布大小、多人混流布局等信息）。

### Objective-C 示例代码：

```objective-c
//CDN 转码设置
AgoraLiveTranscodingUser *user = [[AgoraLiveTranscodingUser alloc] init];
user.uid = 12345;
user.rect = CGRectMake(0, 0, 640, 720);
user.audioChannel = 2;

AgoraRtcEngineKit *rtcEngine = [AgoraRtcEngineKit sharedEngineWithAppId:@"" delegate:nil];
AgoraLiveTranscoding *transcoding = [[AgoraLiveTranscoding alloc] init];
transcoding.audioSampleRate = AgoraAudioSampleRateType44100;
transcoding.audioChannels = 2;
//config.audioBitrate
transcoding.size = CGSizeMake(640, 720);
transcoding.videoFramerate = 30;
transcoding.videoCodecProfile = AgoraVideoCodecProfileTypeHigh;
transcoding.transcodingUsers = @[user];

[rtcEngine setLiveTranscoding:transcoding];
```

```objective-c
// 添加推流地址
// transcodingEnabled: 是否启用转码功能。如启用，需要先通过 setLiveTranscoding 接口设置转码参数
[rtcEngine addPublishStreamUrl:streamUrl transcodingEnabled:NO];
```

```objective-c
// 删除推流地址
[rtcEngine removePublishStreamUrl:streamUrl];
```

## 调整合图布局

频道内有多个主播时，需要通过设置 `transcodingUser` 调整合图布局。

主播需要在 `joinChannel` 后通过 `setLiveTranscoding` 接口定义转码参数和设置（RTMP 画布大小、多人混流布局等信息）。

### 示例 1: 两人横向平铺

如果你想显示以下布局:

<img alt="../_images/sei_2host.png" src="https://web-cdn.agora.io/docs-files/cn/sei_2host.png" style="width: 500px;"/>

设置参数如下:

```objective-c
Canvas:
     width: 360
     height: 640
     backgroundColor: #FFFFFF

User0:
      userId: 123
      x: 0
      y: 0
      width: 320
      height: 360
      zOrder: 1
      alpha: 1.0

User1:
      userId: 456
      x: 320
      y: 0
      width: 320
      height: 360
      zOrder: 1
      alpha: 1.0
```

### 示例 2: 三人纵向平铺

如果你想显示以下布局:

<img alt="../_images/sei_3host.png" src="https://web-cdn.agora.io/docs-files/cn/sei_3host.png" style="width: 370px;"/>

设置参数如下:

```objective-c
Canvas:
      width: 360
      height: 640
      backgroundColor: #FFFFFF

   User0:
      userId: 123
      x: 0
      y: 0
      width: 360
      height: 640
      zOrder: 1
      alpha: 1.0

   User1:
       userId: 456
       x: 0
       y: 320
       width: 180
       height: 320
       zOrder: 2
       alpha: 1.0

   User2:
       userId: 789
       x: 180
       y: 320
       width: 180
       height: 320
       zOrder: 2
       alpha: 1.0
```

### 示例 3: 1 人全屏 + 多人任意悬浮小窗

如果你想显示以下布局:

<img alt="../_images/sei_random.png" src="https://web-cdn.agora.io/docs-files/cn/sei_random.png" style="width: 370px;"/>

设置参数如下:

```objective-c
Canvas:
   width: 360
   height: 640
   backgroundColor: #FFFFFF

User0:
   userId: 123
   x: 0
   y: 0
   width: 360
   height: 640
   zOrder: 1
   alpha: 1.0

User1:
    userId: 456
    x: 45
    y: 390
    width: 110
    height: 213
    zOrder: 2
    alpha: 1.0

User2:
    userId: 789
    x: 185
    y: 390
    width: 110
    height: 213
    zOrder: 2
    alpha: 1.0
```
