
---
title: 在线媒体流输入
description: 
platform: iOS
updatedAt: Thu Nov 01 2018 08:32:07 GMT+0000 (UTC)
---
# 在线媒体流输入
# 在线媒体流输入

## 简介

**在线媒体流输入**功能可以将媒体流作为一个发送端接入正在进行的直播房间。通过将正在播放的视频添加到直播中，主播和观众可以在一起收听/观看媒体流的同时，实时互动。

Agora SDK 从 v2.1.0 版本开始，新增 `addInjectStreamUrl` 接口，通过该接口：

- 主播可以指定媒体流输入源，作为视频源输入给直播频道内的所有观众。
- 支持主播对输入媒体流的 Video Profile 进行设置。
- 如果主播开启并设置了旁路直播，输入的媒体流也可以直播给所有旁路观众。

## 常见使用场景

在线流媒体输入主要适用于如下场景：

- 赛事直播中，主播直接拉流，实现主播与观众边看比赛边点评的功能。
- 同一直播间内，主播与观众在欣赏电影、音乐、演出的同时，实时讨论或交流想法。
- 无人机或网络摄像头直接把采集到的视频推流出去，作为在线媒体流导入直播。

## 方法实现

实现在线媒体流输入首先需要用户以主播身份加入一个直播频道。如果你对如何初始化引擎对象和加入直播频道不了解，请参考 [快速开始](https://docs.agora.io/cn/Interactive%20Broadcast/ios_video?platform=iOS)。

- 输入在线媒体流：

	直播频道的主播可以使用 `addInjectStreamUrl` ，指定一个在线媒体流作为连麦端接入房间。

	```swift
	//Swift
	let urlPath = "Some online RTMP/HLS url path"
	let config = AgoraLiveInjectStreamConfig.default()
	agoraKit.addInjectStreamUrl(urlPath, config: config)
	```

	你可以通过修改 `config` 的参数值控制接入媒体流的分辨率、码率、帧率、音频采样率等参数。详见 [AgoraLiveInjectStreamConfig 参数说明](https://docs.agora.io/cn/Interactive%20Broadcast/API%20Reference/oc/Classes/AgoraLiveInjectStreamConfig.html)。

- 移除在线媒体流：

	频道内的主播可以使用 `removeInjectStreamUrl` 接口，移除一个已经接入的在线媒体流。

	```swift
	//Swift
	let urlPath = "Some online RTMP/HLS url path"
	agoraKit.removeInjectStreamUrl(urlPath)
	```

	> 主播退出频道后，无需再调用 `removeInjectStreamUrl` 接口。

## 注意事项

- 频道内同一时间只允许输入一个在线媒体流。
- 只有主播可以输入和移除在线媒体流，连麦主播和普通用户不可以。
- 主播在直播过程中启用输入在线媒体流。观众需要订阅主播才能观看外部视频。
- 支持的媒体流格式包括：RTMP、HLS、FLV。纯音频流也可以作为在线媒体流输入。
- 如果媒体流输入成功，该媒体流会出现在频道中，并收到 `didJoinChannel` 和 `firstRemoteVideoDecodedOfUid` 回调，其中 `uid` 为 666。
