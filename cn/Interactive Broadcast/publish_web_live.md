
---
title: 发布和订阅音视频流
description: Web平台上发布音视频流
platform: Web
updatedAt: Thu Dec 13 2018 09:46:40 GMT+0000 (UTC)
---
# 发布和订阅音视频流
在发布和订阅音视频流前，请确保你已完成环境准备、安装包获取等步骤，并成功加入频道，详见[客户端集成](../../cn/Interactive%20Broadcast/web_prepare.md)。

## 实现方法
### 创建音视频流
使用 `AgoraRTC.createStream` 方法创建音视频流对象。

示例代码中设置了以下参数：

- `streamID`：音视频流 ID。设置为用户 ID （uid），可通过 `client.join` 的回调获取。
- `audio`: 是否开启音频。
- `video`: 是否开启视频。
- `screen`: 是否开启屏幕共享功能。请勿将 `video` 和 `screen` 同时设置为 true 。

```javascript
var localStream = AgoraRTC.createStream({
    streamID: uid,
    audio: true,
    video: true,
    screen: false}
);
```

### 初始化音视频流
接下来调用 `stream.init` 方法初始化创建的音视频流。

```javascript
localStream.init(function() {
  console.log("getUserMedia successfully");
  localStream.play('agora_local');

}, function (err) {
  console.log("getUserMedia failed", err);
});
```

### 发布本地音视频流
初始化完成后，在成功的回调中通过 `client.publish` 方法发布音视频流。

```javascript
client.publish(localStream, function (err) {
  console.log("Publish local stream error: " + err);
});

client.on('stream-published', function (evt) {
  console.log("Publish local stream successfully");
});
```

### 订阅远端音视频流
订阅远端音视频流步骤如下：

1. 监听 `client.on('stream-added')` 事件, 当有人发布音视频流到频道里时，会收到该事件。
2. 收到事件后，在回调中调用 `client.subscribe` 方法订阅远端音视频流。

```javascript
client.on('stream-added', function (evt) {
  var stream = evt.stream;
  console.log("New stream added: " + stream.getId());

  client.subscribe(stream, function (err) {
    console.log("Subscribe stream failed", err);
  });
});
client.on('stream-subscribed', function (evt) {
  var stream = evt.stream;
  console.log("Subscribe remote stream successfully: " + stream.getId());
  stream.play('agora_remote' + stream.getId());
})
```

### 播放音视频流
在初始化流成功和订阅流成功后，都可以在回调中调用 `stream.play` 在页面上播放流。 `stream.play` 接受一个 dom 元素的 id 作为参数，SDK 会在这个 dom 下面创建一个 `<video>` 标签并播放音视频。

- 初始化流成功时，播放本地流

	```javascript
	localStream.init(function() {
		console.log("getUserMedia successfully");
		// 这里使用agora_local作为dom元素的id。
		localStream.play('agora_local');

	}, function (err) {
		console.log("getUserMedia failed", err);
	});
	```

- 订阅流成功时，播放远端流

	```javascript
	client.on('stream-subscribed', function (evt) {
		var stream = evt.stream;
		console.log("Subscribe remote stream successfully: " + stream.getId());
		// 这里使用agora_remote + stream.getId()作为dom元素的id。
		stream.play('agora_remote' + stream.getId());
	})
	```

## 相关文档
你已成功开始互动直播。直播结束后，可以使用 Agora SDK 退出当前频道：
* [离开频道](../../cn/Interactive%20Broadcast/leave_web.md)

直播过程中，你还可以实现如下功能：
* [开启与 Native SDK 的互通](../../cn/Interactive%20Broadcast/interop_web.md)
* [推流到 CDN](../../cn/Interactive%20Broadcast/push_stream_web.md)
