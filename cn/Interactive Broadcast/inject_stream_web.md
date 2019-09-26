
---
title: 输入在线媒体流
description: 
platform: Web
updatedAt: Thu Sep 26 2019 09:56:37 GMT+0800 (CST)
---
# 输入在线媒体流
## 简介

**输入在线媒体流**功能可以将媒体流作为一个发送端接入正在进行的直播房间。通过将正在播放的音频添加到直播中，主播和观众可以在一起收听媒体流的同时实时互动。

Agora Web SDK 从 v2.5.1 版本开始，新增 `Client.addInjectStreamUrl` 接口，通过该接口：

- 主播可以指定媒体流输入源，作为视频源输入给直播频道内的所有观众。
- 支持主播对输入媒体流的 Video Profile 进行设置。
- 如果主播开启并设置了旁路直播，输入的媒体流也可以直播给所有旁路观众。

## 常见使用场景

在线流媒体输入主要适用于如下场景：

- 赛事直播中，主播直接拉流，实现主播与观众边看比赛边点评的功能。
- 统一直播间内，主播与观众在关上电影、音乐、演出的同时，实时讨论或交流想法。
- 同一直播间内，主播与观众在欣赏音乐的同时，实时讨论或交流想法。

## 注意事项

- 在语音直播中，你只能输入纯音频流，而不能够将视频文件的音频作为在线媒体流输入。
- 频道内同一时间只允许输入一个在线媒体流。
- 只有主播可以输入和移除在线媒体流，连麦主播和普通用户不可以。
- 主播在直播过程中启用输入在线媒体流。观众需要订阅主播才能收听外部音频。
- 支持的媒体流格式包括：RTMP、HLS、FLV。
- 如果媒体流输入成功，该媒体流会出现在频道中，并收到 `peer-online` 和 `first-video-frame-decode` 回调，其中 `uid` 为 666。
- 如果媒体流输入失败，会返回错误码。可能会出现的错误码及处理方法如下：

  - 2：输入的 URL 为空。请重新调用该方法，并确认输入的媒体流的 URL 是有效的
  - 7：引擎没有初始化。请确认调用该方法前已创建对象并完成初始化
  - 3：没有加入频道。请确认 App 在频道内

## 实现方法

实现在线媒体流输入首先需要用户以主播身份加入一个直播频道。如果你对如何初始化引擎对象和加入直播频道不了解，请参考 [快速开始](../../cn/Interactive%20Broadcast/web_prepare.md)。

- 输入在线媒体流：

	直播频道的主播可以使用 `Client.addInjectStreamUrl` ，指定一个在线媒体流作为连麦端接入房间。

	```javascript
	var InjectStreamConfig = {
	 width: 0,
	 height: 0,
	 videoGop: 30,
	 videoFramerate: 15,
	 videoBitrate: 400,
	 audioSampleRate: 44100,
	 audioChannels: 1,
	});
	
	Client.addInjectStreamUrl(url, config);
	```

	你可以通过修改 `config` 的参数值控制接入媒体流的音频采样率、音频码率和音频频道数。详见 [InjectStreamConfig 参数说明](https://docs.agora.io/cn/Interactive%20Broadcast/API%20Reference/web/interfaces/agorartc.injectstreamconfig.html) 。
	
- 移除在线媒体流

	频道内的主播可以使用 `Client.removeInjectStreamUrl` 接口，移除一个已经接入的在线媒体流。
	
	```javascript
	Client.removeInjectStreamUrl(url);
	```

	> 主播退出频道后，无需再调用 `Client.removeInjectStreamUrl` 接口。


## 工作原理
- 频道中的主播通过 Video Inject 服务器，将在线媒体流拉取到 Agora SD-RTN 上，推送到直播频道内，频道内的连麦主播、普通观众都可以接收到对应的媒体流。
- 如果主播开启了 CDN 推流，对应的媒体流也会被推送到 CDN 上，CDN 观众就也可以听到这路媒体流。

