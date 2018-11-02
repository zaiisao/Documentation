
---
title: 进行屏幕共享
description: 
platform: Web
updatedAt: Fri Nov 02 2018 04:04:56 GMT+0000 (UTC)
---
# 进行屏幕共享
要实现屏幕共享的功能，只需要在创建流的时候设置相关的属性（Chrome 和 Firefox 的实现方式不同）。建流的过程中浏览器会询问需要共享哪些屏幕，根据用户的选择去获取屏幕图像。

本页包含以下内容：

- [Chrome 屏幕共享](#chrome)
- [Firefox 屏幕共享](#ff)
- [同时共享屏幕和开启视频](#both)

在开始屏幕共享前，请确保你已经完成环境准备、安装包获取等步骤，详见 [设置开发环境](../../cn/Interactive%20Broadcast/web_prepare.md) 。

## <a name = "chrome"></a>Chrome 屏幕共享

在 Chrome 上使用屏幕共享功能需要安装 Agora 提供的 [Chrome 屏幕共享插件](../../cn/Quickstart%20Guide/chrome_screensharing_plugin.md) ，在建流的时候把插件的 **extensionId** 填进字段。

```javascript
screenStream = AgoraRTC.createStream({
  streamID: uid,
  audio: false,
  video: false,
  screen: true,
  //chrome extension ID
  extensionId: 'minllpmhdgpndnkomcoccfekfegnlikg'
});
```

> - 因为一个 Stream 只能有一路视频流，所以 `video` 和 `screen` 属性不能同时为 true。
> - `audio` 属性建议设置为 false，避免订阅端收到的两路流中都有音频，导致回声。

## <a name = "ff"></a>Firefox 屏幕共享

Firefox 屏幕共享不需要安装插件，但是需要通过设置 `mediaSource` 指定分享屏幕的类型，`mediaSource` 的选择如下：

- `screen`：分享整个显示器屏幕
- `application`：分享某个应用的所有窗口
- `window`：分享某个应用的某个窗口

```javascript
screenStream = AgoraRTC.createStream({
  streamID: uid,
  audio: false,
  video: false,
  screen: true,
  mediaSource: 'screen' // 'screen', 'application', 'window'
});
```

> - 因为一个 Stream 只能有一路视频流，所以 `video` 和 `screen` 属性不能同时为 true。
> - `audio` 属性建议设置为 false，避免订阅端收到的两路流中都有音频，导致回声。
> - Firefox 在 Windows 平台不支持 application 模式。

## <a name = "both"></a>同时共享屏幕和开启视频

因为每个 Client 对象只能发送一路 Stream 流，所以如果要在一个发送端同时分享屏幕和开启视频，需要创建两个 Client，一路发送屏幕共享流，一路发送视频流。

```javascript
// 创建用于发送屏幕共享流的Client
var screenClient = AgoraRTC.createClient({mode: 'rtc', codec: 'vp8'});
screenClient.init(key, function() {
 screenClient.join(channelKey, channel, null, function(uid) {
  // 创建屏幕共享流 screenStream
  ...
  screenClient.publish(screenStream);
 }
  }

// 创建用于发送视频流的Client
var videoClient = AgoraRTC.createClient({mode: 'rtc', codec: 'vp8'});
videoClient.init(key, function() {
videoClient.join(channelKey, channel, null, function(uid) {
  // 创建视频流 videoStream
  ...
  videoClient.publish(videoStream);
 }
}
```

自己订阅自己，会产生额外的计费，如图：

<img alt="../_images/screensharing_streams.png" src="https://web-cdn.agora.io/docs-files/cn/screensharing_streams.png" style="width: 500px;" />

Agora 建议，为避免重复计费，每个 Client 成功加入频道以后，把返回的 uid 存在列表里。每次监听到 `’stream-added’` 事件的时候，先判断加入的流是否是本地流，如果是，则不订阅。

```javascript
var localStreams = [];
...

screenClient.join(channelKey, channel, null, function(uid) {
 // 保存本地流的uid
 localStreams.push(uid);
}
...

screenClient.on('stream-added', function(evt) {
 var stream = evt.stream;
 var uid = stream.getId()
 // 收到流加入频道的事件后，先判定是不是本地的uid
 if(!localStreams.includes(uid)) {
  console.log('subscribe stream: ' + uid);
  // 拉流
  screenClient.subscribe(stream);
 }
})
```

完整代码如下：

```javascript
var key = “********************************”;
var channel = “screen_video”;
var channelKey = null;

AgoraRTC.Logger.setLogLevel(AgoraRTC.Logger.INFO);

var localStreams = [];

var screenClient = AgoraRTC.createClient({mode: 'rtc', codec: 'vp8'});
screenClient.init(key, function() {
screenClient.join(channelKey, channel, null, function(uid) {
// 保存本地流的uid
localStreams.push(uid);
// 创建屏幕共享流
screenStream = AgoraRTC.createStream({
streamID: uid,
audio: false, // 设置屏幕共享不带音频，避免订阅端收到的两路流中都有音频，导致回声
video: false,
screen: true,
// Chrome
extensionId: 'minllpmhdgpndnkomcoccfekfegnlikg',
// Firefox
mediaSource: 'window' // 'screen', 'application', 'window'
});
// 初始化流
screenStream.init(function() {
// 播放流
screenStream.play('Screen');
// 推流
screenClient.publish(screenStream);

// 监听流（用户）加入频道事件
screenClient.on('stream-added', function(evt) {
var stream = evt.stream;
var uid = stream.getId()

// 收到流加入频道的事件后，先判定是不是本地的uid
if(!localStreams.includes(uid)) {
  console.log('subscribe stream:' + uid);
  // 拉流
  screenClient.subscribe(stream);
  }
  })

}, function (err) {
  console.log(err);
});

}, function (err) {
  console.log(err);
})
});

var videoClient = AgoraRTC.createClient({mode: 'rtc', codec: 'vp8'});
videoClient.init(key, function() {
videoClient.join(channelKey, channel, null, function(uid) {
// 保存本地流的uid
localStreams.push(uid);

// 创建视频流
videoStream = AgoraRTC.createStream({
streamID: uid,
audio: true,
video: true,
screen: false
});
return;

// 初始化流
videoStream.init(function() {
// 播放流
videoStream.play('Video');
// 推流
videoClient.publish(videoStream);
//监听流（用户）加入频道事件
videoClient.on('stream-added', function(evt){
var stream = evt.stream;
var uid = stream.getId();
// 收到流加入频道的事件后，先判定是不是本地的uid
if(!localStreams.includes(uid)) {
console.log('subscribe stream:' + uid);
// 拉流
screenClient.subscribe(stream);
}
})
}, function (err) {
  console.log(err);
  });

  }, function (err) {
  console.log(err);
  })
});
```
