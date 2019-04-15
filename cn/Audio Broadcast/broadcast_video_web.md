
---
title: 实现视频直播
description: 
platform: Web
updatedAt: Mon Apr 01 2019 09:20:55 GMT+0800 (CST)
---
# 实现视频直播
# 入门: 实现视频直播

本页介绍如何使用 Agora Web SDK 快速实现网页端视频直播。

本页以声网提供的 [示例项目](https://github.com/AgoraIO/Basic-Video-Call/tree/master/One-to-One-Video/Agora-Web-Tutorial-1to1) 为例说明 API 的使用。如需了解示例代码具体如何实现和运行，请查看示例项目中的 **README** 文件。

## 环境准备

具体的开发环境要求、如何获取 App ID 以及 SDK 集成方法，详见 [设置开发环境](../../cn/Quickstart%20Guide/web_prepare.md)。

## 使用核心 API 实现网页端视频通话功能

实现网页端视频通话主要步骤为：

1. [创建 Client 对象](#createClient)
2. [初始化 Client 对象](#initClient)
3. [加入频道](#join)
4. [创建音视频流](#createStream)
5. [初始化音视频流](#initStream)
6. [发布本地音视频流](#pub)
7. [订阅远端音视频流](#sub)
8. [播放音视频流](#play)
9. [离开频道](#leave)

示意图：

<img alt="../_images/web-sdk-workflow.jpg" src="https://web-cdn.agora.io/docs-files/cn/web-sdk-workflow.jpg" style="width: 431.2px; height: 423.5px;"/>

示例项目中的 `index.html` 文件包含了使用以下 API 的完整代码，供参考。这些 API 也可以在其他项目中用于实现视频通话。

### <a name = "createClient"></a>创建 Client 对象

用 `AgoraRTC.createClient` 方法创建 Client 对象，设置 `mode` 和 `codec` 。

```javascript
var client = AgoraRTC.createClient({mode: 'live', codec: "h264"});
```

> 发送端 Client 对象每个通话仅可创建一次。

### <a name = "initClient"></a>初始化 Client 对象

创建 Client 对象后，将项目的 App ID 填入 `client.init` 方法，即可初始化 Client。

```javascript
client.init(<APPID>, function () {
  console.log("AgoraRTC client initialized");

}, function (err) {
  console.log("AgoraRTC client init failed", err);
});
```

### <a name = "join"></a>加入频道

初始化 Client 对象完成后， 在成功的回调中调用 `client.join` 方法。

在  `client.join` 方法中填入以下参数值：

- `DYNAMIC_KEY`：如果没有开启动态密钥，设置为 null。
- `CHANNEL_NAME`：频道名称。
- `UID`：用户的 ID， **整数，需保证唯一性**。 如果不指定，即用户 ID 设置为 null，回调会返回一个服务器分配的 uid。

```javascript
client.join(<DYNAMIC_KEY>, <CHANNEL_NAME>, <UID>, function(uid) {
  console.log("User " + uid + " join channel successfully");

}, function(err) {
  console.log("Join channel failed", err);
});
```

> 使用不同 App ID 的用户即使加入同一个频道也无法互相通话。

### <a name = "createStream"></a>创建音视频流

使用 `client.createStream` 方法创建音视频流对象。

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


### <a name = "initStream"></a>初始化音视频流

接下来调用 `stream.init` 方法初始化创建的音视频流。

```javascript
localStream.init(function() {
  console.log("getUserMedia successfully");
  localStream.play('agora_local');

}, function (err) {
  console.log("getUserMedia failed", err);
});
```

### <a name = "pub"></a>发布本地音视频流

初始化完成后，在成功的回调中通过 `client.publish` 方法发布音视频流。

```javascript
client.publish(localStream, function (err) {
  console.log("Publish local stream error: " + err);
});

client.on('stream-published', function (evt) {
  console.log("Publish local stream successfully");
});
```

### <a name = "sub"></a>订阅远端音视频流

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
  var remoteStream = evt.stream;
  console.log("Subscribe remote stream successfully: " + stream.getId());
  stream.play('agora_remote' + stream.getId());
})
```

### <a name = "play"></a>播放音视频流

在初始化流成功和订阅流成功后，都可以在回调中调用 `stream.play` 在页面上播放流。 `stream.play` 接受一个 dom 元素的 id 作为参数，SDK 会在这个 dom 下面创建一个 `<video>` 标签并播放音视频。

#### 初始化流成功时，播放本地流

```javascript
localStream.init(function() {
  console.log("getUserMedia successfully");
  // 这里使用agora_local作为dom元素的id。
  localStream.play('agora_local');

}, function (err) {
  console.log("getUserMedia failed", err);
});
```

#### 订阅流成功时，播放远端流

```javascript
client.on('stream-subscribed', function (evt) {
  var remoteStream = evt.stream;
  console.log("Subscribe remote stream successfully: " + stream.getId());
  // 这里使用agora_remote + stream.getId()作为dom元素的id。
  remoteStream.play('agora_remote' + stream.getId());
})
```

### <a name = "leave"></a>离开频道

使用 `client.leave` 方法让用户离开当前通话（频道）。

```javascript
client.leave(function () {
  console.log("Leave channel successfully");
}, function (err) {
  console.log("Leave channel failed");
});
```

## 完整代码

完整代码请参考示例项目中的 `index.html` 文件。

现在可以运行项目，输入频道名称，开启视频直播。

> 请确保在视频直播之前开启摄像头和麦克风权限。

## 参考

在上述视频直播过程中，我们使用了 Agora SDK 提供的部分 API 功能：

- 创建音视频对象：`AgoraRTC.createClient`
- 初始化客户端对象：`client.init`
- 加入 AgoraRTC 频道：`client.join`
- 创建音视频流对象：`client.createStream`
- 初始化音视频对象：`stream.init`
- 发布本地音视频流：`client.publish`
- 订阅远程音视频流：`client.subscribe`
- 播放音视频流：`stream.play`
- 离开 AgoraRTC 频道：`client.leave`

在实现视频直播的过程中，可以使用以下功能:

- [企业部署代理服务器](../../cn/Quickstart%20Guide/proxy_web.md)
- [实现网页端屏幕共享](../../cn/Quickstart%20Guide/screensharing_web.md)

更多功能实现，请参考 [Agora Web SDK API](https://docs.agora.io/cn/Audio%20Broadcast/API%20Reference/web/index.html) 中各 API 的功能及描述。
