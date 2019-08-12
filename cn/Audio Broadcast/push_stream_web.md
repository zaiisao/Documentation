
---
title: 推流到 CDN
description: 
platform: Web
updatedAt: Mon Aug 12 2019 10:33:33 GMT+0800 (CST)
---
# 推流到 CDN
## 功能描述

将直播流发布到 CDN（Content Delivery Network）的过程称为 CDN 直播推流，用户无需安装 App，可以通过 Web 浏览器观看直播。

在推流到 CDN 过程中，当频道中有多个主播时，通常会涉及到[转码](https://docs.agora.io/cn/Agora%20Platform/terms?platform=All%20Platforms#转码)，将多个直播流组合成单个流，并设置这个流的音视频属性和合图布局。



## 实现方法

### 1. 检查浏览器兼容性

检查浏览器兼容性 \(`checkSystemRequirements`\)

```javascript
checkSystemRequirements()
```

### 2. 创建音视频对象

创建音视频对象 \(`createClient`\)

```javascript
createClient()
```

### 3. 初始化客户端对象

初始化客户端对象 \(`init`\)

```javascript
init(appId, onSuccess, onFailure)
```

### 4. 加入频道

加入 AgoraRTC 频道 \(`join`\)

```javascript
join(token, channel, uid, onSuccess, onFailure)
```

### 5. 创建音视频流对象

创建音视频流对象 \(`createStream`\)

```javascript
createStream(spec)
```

### 6. 设置直播转码

设置直播转码 \(`setLiveTranscoding`\)

```javascript
var LiveTranscoding = {
  width: 640, //用于旁路直播的输出视频的总宽度，默认值为 640。取值范围为 [16,10000]。
  height: 360, //用于旁路直播的输出视频的总高度，默认值为 360。取值范围为 [16,10000]。
  videoBitrate: 400, //设置转码推流的码率，单位为 Kbps，默认值为 400。
  videoFramerate: 15, //用于旁路直播的输出视频的帧率，单位为帧每秒，默认值为 15。取值范围为 [1,30]。服务器会将高于 30 的帧率设置改为 30。
  lowLatency: false,
  audioSampleRate: 48000,
  audioBitrate: 48,
  audioChannels: 1,
  videoGop: 30,
  videoCodecProfile: 100, //用于旁路直播的输出视频的编码规格。可以设置为 66、77 或 100。如果设置其他值，Agora 会统一设为默认值 100。
  userCount: 0,
  userConfigExtraInfo: {},
  backgroundColor: 0x000000,
  transcodingUsers: [],
};
```

> - 在 `AgoraRTC.createClient({mode:'nterop'})` 模式下，如果使用单 Web 主播进行推流，需要将 Web 单主播的码流进行转码后再进行推流，否则会出现没有视频的现象。
> - 若要对 Web 单主播直接进行推流，请使用 `AgoraRTC.createClient({mode:'h264_interop'})` 模式。
> - Agora 转码需要收取转码费用。
> - 你可以参考[视频分辨率表格](https://docs.agora.io/cn/Audio%20Broadcast/cn/Video/API%20Reference/web/v2.9.0/interfaces/agorartc.videoencoderconfiguration.html?transId=2.9.0#bitrate)设置 `videoBitrate` 的值。如果设置的码率超出合理范围，Agora 服务器会在合理区间内自动调整码率值。

### 7. 新建直播流

新建直播流 \(`startLiveStreaming`\)

```javascript
client.setLiveTranscoding(coding);
// 如果在 enableTranscoding 里设置为 true，那么必须在调用 setLiveTranscoding 之后再调用 startLiveStreaming。
client.startLiveStreaming(url, true)
```

### 8. 删除直播流

删除直播流 \(`stopLiveStreaming`\)

```javascript
client.stopLiveStreaming(url);
```

### 9. 退出频道

离开 AgoraRTC 频道 \(`leave`\)

```javascript
leave(onSuccess, onFailure)
```
