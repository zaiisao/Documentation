
---
title: 进行屏幕共享
description: 
platform: Web
updatedAt: Thu Sep 19 2019 10:05:11 GMT+0800 (CST)
---
# 进行屏幕共享
## 功能简介

在视频通话或互动直播中进行屏幕共享，可以将说话人或主播的屏幕内容，以视频画面的方式分享给其他说话人或观众观看，以提高沟通效率。

屏幕共享在如下场景中应用广泛：

- 视频会议场景中，屏幕共享可以将讲话者本地的文件、数据、网页、PPT 等画面分享给其他与会人；
- 在线课堂场景中，屏幕共享可以将老师的课件、笔记、讲课内容等画面展示给学生观看。

## 工作原理

Web 端屏幕共享，实际上是通过创建一个屏幕共享的流来实现的。

- 如果只使用屏幕共享，则在新建流的时候，把 `video` 字段设为 false， `screen` 字段设为 true 即可。
- 如果在使用屏幕共享的同时，还开启本地视频，则需要创建两个 Client 对象，一路发送屏幕共享流，一路发送视频流。新建流的时候，屏幕共享流的 `video` 字段设为 false， `screen` 字段设为 true；本地视频流的 `video` 字段设为 `true`，`screen` 字段设为 `false`。由于共享流也是一路流，因此也会占用一个 UID。


## 实现方法

在开始屏幕共享前，请确保你已完成环境准备、安装包获取等步骤，详见 [集成客户端](https://docs.agora.io/cn/Interactive%20Broadcast/web_prepare?platform=Web)。

开始屏幕共享前，你需要在创建流的时候配置某些属性。Chrome 和 Firefox 浏览器在创建流的时候，相关的属性是不同的。建流的过程中浏览器会询问需要共享哪些屏幕，根据用户的选择去获取屏幕信息。

### <a name = "chrome"></a>Chrome 屏幕共享

#### 使用插件进行屏幕共享

在 Chrome 上使用屏幕共享功能需要安装 Agora 提供的 [Chrome 屏幕共享插件](../../cn/Interactive%20Broadcast/chrome_screensharing_plugin.md) ，并获取插件的 `extensionId`，在建流的时候填入 `extensionId`。

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

#### 无插件屏幕共享

Agora Web SDK 从 2.6.0 版本起，支持在 Chrome 72 及以上版本不安装插件直接共享屏幕，在 `createStream` 时不填写 `extensionId` 参数即可。

```javascript
// Check if the browser supports screen sharing without an extension
Number.tem = ua.match(/(Chrome(?=\/))\/?(\d+)/i);
if(parseInt(tem[2]) >= 72  && navigator.mediaDevices.getDisplayMedia ) {
 // Create the stream for screensharing
	screenStream = AgoraRTC.createStream({
		streamID: uid,
		audio: false,
		video: false,
		screen: true,
	});
}
```

> - 因为一个 Stream 只能有一路视频流，所以 `video` 和 `screen` 属性不能同时为 `true`。
> - `audio` 属性建议设置为 `false`，避免订阅端收到的两路流中都有音频，导致回声。


### Electron 屏幕共享

Electron 屏幕共享不需要安装插件，但选择界面需要你自行绘制, Electron 仅提供用于获取共享源的接口。

Electron 屏幕共享主要通过如下步骤实现：

1. 调用 SDK 提供的  `AgoraRTC.getScreenSources` 方法获取可共享的屏幕信息。

   ```javascript
   AgoraRTC.getScreenSources(function(err, sources) {
   	console.log(sources)
   }
   ```

    `sources` 是一个 `source` 对象的列表，`source` 里包含了分享源的信息和 `sourceId`，`source` 的属性如下：

   ![](https://web-cdn.agora.io/docs-files/1547455349613)

   - `id`： 即 `sourceId`
   - `name`：屏幕源的名字
   - `thumbnail`：屏幕源的快照

2. 根据 `source` 的属性，（用 html 和 css）绘制选择界面，让用户选择要共享的屏幕源。为方便快速集成，Agora 提供默认的选择界面。

   `source` 的属性与 Chrome 屏幕共享的选择界面对应关系如下：

   ![](https://web-cdn.agora.io/docs-files/1547456888707)

3. 获取用户选择的 `sourceId`。

4. 调用 `AgoraRTC.createStream`， 将 `screen` 设置为 `true` 并填入 `sourceId`，就能创建相应的屏幕共享流了。

   ```javascript
   localStream = AgoraRTC.createStream({
       streamID: UID,
       audio: false,
       video: false,
       screen: true,
       sourceId: sourceId
   });
   localStream.init(function(stream) {})
   ```

   如果未填写 `sourceId`，在调用 `localStream.init` 时，SDK 会提供自带的默认界面。默认的选择界面与 Chrome 的选择界面类似，如下图所示：

   ![](https://web-cdn.agora.io/docs-files/1547455511311)

> - `getScreenSources` 方法是对 Electron 提供的 `desktopCapturer.getSources` 进行的封装，详情可参考 [desktopCapturer](https://electronjs.org/docs/api/desktop-capturer)。
> - 在非 Electron 下传入 `sourceId` 会被忽略。

### <a name = "ff"></a>Firefox 屏幕共享

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

### <a name = "both"></a>同时共享屏幕和开启视频

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

### 完整代码

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

## 开发注意事项

- 屏幕共享流的 UID 尽量不要固定在同一个值，否则某些场景下同 UID 的共享流可能会引起互踢。
- 在本地共享的时候，本地流的 Client **不要订阅本地的分享流**，否则会增加计费。
- 创建屏幕共享流的时候，`video`/`audio` 必须设置为 `false`。
- 在 Windows 平台上进行屏幕共享时，如果共享的是 QQ 聊天窗口会导致黑屏。

