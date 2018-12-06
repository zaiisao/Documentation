
---
title: 推流到 CDN
description: 
platform: Web
updatedAt: Thu Dec 06 2018 06:58:46 GMT+0000 (UTC)
---
# 推流到 CDN
## 功能描述

旁路推流功能用于将主播的上行音视频流转化为 RTMP 流分发，供 Web 端或流媒体播放器端观看。

> 请联系 [sales@agora.io](mailto:sales@agora.io) 开通推流功能。


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
  width: 640,
  height: 360,
  videoBitrate: 400,
  videoFramerate: 15,
  lowLatency: false,
  audioSampleRate: 48000,
  audioBitrate: 48,
  audioChannels: 1,
  videoGop: 30,
  videoCodecProfile: 100,
  userCount: 0,
  userConfigExtraInfo: {},
  backgroundColor: 0x000000,
  transcodingUsers: [],
};
```

> 在 `AgoraRTC.createClient({mode: ‘interop’})` 模式下，如果使用单 Web 主播进行推流，需要将 Web 单主播的码流进行转码后再进行推流，否则会出现没有视频的现象。
>
> 若要对 Web 单主播直接进行推流，请使用 `AgoraRTC.createClient({mode: ‘h264_interop’})` 模式。
>
> 影响：Agora 转码需要收取转码费用。
> 
### 7. 新建直播流

新建直播流 \(`startLiveStream`\)

```javascript
client.setLiveTranscoding(coding);
//if enableTranscoding is set to true, setLiveTranscoding must be called before _startLiveStreaming
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
