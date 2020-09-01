
---
title: 屏幕共享
description: 
platform: Web
updatedAt: Tue Sep 01 2020 09:05:45 GMT+0800 (CST)
---
# 屏幕共享
## 功能简介

在视频通话或互动直播中进行屏幕共享，可以将说话人或主播的屏幕内容，以视频画面的方式分享给其他说话人或观众观看，以提高沟通效率。

<div class="alert note">Agora Web SDK 不支持在移动端设备上进行屏幕共享。</div>

屏幕共享在如下场景中应用广泛：

- 视频会议场景中，屏幕共享可以将讲话者本地的文件、数据、网页、PPT 等画面分享给其他与会人；
- 在线课堂场景中，屏幕共享可以将老师的课件、笔记、讲课内容等画面展示给学生观看。

在开始屏幕共享前，请确保你已了解如何[实现音视频通话](../../cn/Video/start_call_web.md)或[实现互动直播](../../cn/Video/start_live_web.md)。


<div class="alert info">点击<a href="https://webdemo.agora.io/agora-web-showcase/examples/Agora-Screen-Sharing-Web/">在线体验</a>试用屏幕共享功能。</div>

## 工作原理

Web 端屏幕共享，实际上是通过创建一个屏幕共享的流来实现的。开始屏幕共享前，你需要在创建流的时候配置某些属性。不同浏览器在创建流的时候，相关的属性是不同的。建流的过程中浏览器会询问需要共享哪些屏幕，根据用户的选择去获取屏幕信息。

- 如果只使用屏幕共享，则在新建流的时候，把 `video` 字段设为 `false`， `screen` 字段设为 `true` 即可。
- 如果在使用屏幕共享的同时，还开启本地视频，则需要创建两个 Client 对象，一路发送屏幕共享流，一路发送视频流。新建流的时候，屏幕共享流的 `video` 字段设为 `false`， `screen` 字段设为 `true`；本地视频流的 `video` 字段设为 `true`，`screen` 字段设为 `false`。由于共享流也是一路流，因此也会占用一个 UID。

## <a name="setScreenProfile"></a>设置共享屏幕的视频属性

共享屏幕默认的视频属性为：分辨率（宽 × 高） 1920 × 1080，帧率 5 fps。如果你需要使用其他的视频属性，可以调用 `Stream.setScreenProfile` 设置共享屏幕的视频属性。

```javascript
// After creating a stream for screen sharing
screenStream.setScreenProfile("720p_1");
```

SDK 支持设置的视频属性如下表所示：

| 屏幕视频属性 | 分辨率（宽 × 高） | 帧率   |
| :----------- | :---------------- | :----- |
| `480p_1`     | 640 × 480         | 5 fps  |
| `480p_2`     | 640 × 480         | 30 fps |
| `720p_1`     | 1280 × 720        | 5 fps  |
| `720p_2`     | 1280 × 720        | 30 fps |
| `1080p_1`    | 1920 × 1080       | 5 fps  |
| `1080p_2`    | 1920 × 1080       | 30 fps |

<div class="alert note">注意事项：<br/><li>屏幕视频属性必须在初始化流 (<code>Stream.init</code>) 之前设置。</li><li>修改屏幕视频属性可能会影响计费。由于设备和浏览器的限制，部分浏览器对设置的屏幕视频属性不一定能全部适配。这种情况下浏览器会自动调整分辨率，计费也将按照实际分辨率计算。</li></div>

## <a name = "chrome"></a>Chrome 屏幕共享

### 无插件屏幕共享

在 Chrome 上屏幕共享直接在 `createStream` 时把 `video` 字段设为 `false`， `screen` 字段设为 `true` 即可。

<div class="alert note">该功能要求 Agora Web SDK 2.6.0 或以上版本，Chrome 72 或以上版本。如果你使用的软件版本不满足此要求，请<a href="#ext">使用屏幕共享插件</a>实现在 Chrome 上共享屏幕。</div>

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

### <a name="ext"></a>使用屏幕共享插件

安装 Agora 提供的 [Chrome 屏幕共享插件](../../cn/Video/chrome_screensharing_plugin.md) ，并获取插件的 `extensionId`，在建流的时候填入 `extensionId`。

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

### <a name = "screenAudio"></a>分享音频

Agora Web SDK 从 3.0.0 版本起支持在 Windows 平台的 Chrome 浏览器 74 及以上版本同时共享屏幕和本地播放的背景音，在 `createStream` 时把 `screen` 字段和 `screenAudio` 字段都设为 `true` 即可。

```javascript
screenStream = AgoraRTC.createStream({
  streamID: uid,
  audio: false,
  video: false,
  screen: true,
  screenAudio: true
});
```

<div class="alert note">注意事项：
	<ul>
		<li>在创建屏幕共享的流时要注意区分 <code>audio</code> 和 <code>screenAudio</code> 字段：
			<ul>
				<li><code>audio</code> 字段用于控制创建的流中是否包含由本地音频输入设备采集的音频。</li>
				<li><code>screenAudio</code> 字段用于控制创建的流中是否包含本地播放的声音。</li>
			</ul>
			我们建议在屏幕共享的流中将 <code>audio</code> 设为 <code>false</code>。如果 <code>screenAudio</code> 和 <code>audio</code> 都设置为 <code>true</code>，屏幕共享流中只会包含本地播放的背景音。</li>
		<li>设置了 <code>screenAudio</code> 为 <code>true</code> 后，还需要在屏幕共享的弹出框上勾选<b>分享音频</b>才能真正生效。</li>
		<li>如果选择共享单个应用窗口无法使用分享音频功能。</li>
		</ul>
</div>

![](https://web-cdn.agora.io/docs-files/1574070243757)

## Electron 屏幕共享

Electron 屏幕共享的选择界面需要你自行绘制，为方便快速集成，我们提供一个默认的选择界面。

1. 调用 `AgoraRTC.createStream`， 将 `screen` 设置为 `true` 创建屏幕共享流。

  ```javascript
  localStream = AgoraRTC.createStream({
    streamID: UID,
    audio: false,
    video: false,
    screen: true
  });
  localStream.init(function(stream) {})
  ```

2. 调用 `localStream.init` ，SDK 会提供自带的默认界面让用户选择要共享的屏幕或窗口，如下图所示：

 ![](https://web-cdn.agora.io/docs-files/1571629977600)

如果你需要自定义选择界面，参考以下步骤：

1. 调用 SDK 提供的  `AgoraRTC.getScreenSources` 方法获取可共享的屏幕信息。

   ```javascript
   AgoraRTC.getScreenSources(function(err, sources) {
   	console.log(sources)
   })
   ```

    `sources` 是一个 `source` 对象的列表，`source` 里包含了分享源的信息和 `sourceId`，`source` 的属性如下：

   ![](https://web-cdn.agora.io/docs-files/1547455349613)

   - `id`： 即 `sourceId`
   - `name`：屏幕源的名字
   - `thumbnail`：屏幕源的快照

2. 根据 `source` 的属性，（用 html 和 css）绘制选择界面，让用户选择要共享的屏幕源。

   `source` 的属性与屏幕共享的选择界面对应关系如下：

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


> - `getScreenSources` 方法是对 Electron 提供的 `desktopCapturer.getSources` 进行的封装，详情可参考 [desktopCapturer](https://electronjs.org/docs/api/desktop-capturer)。
> - 在非 Electron 下传入 `sourceId` 会被忽略。

## <a name="ff"></a>Firefox 屏幕共享

Firefox 屏幕共享需要通过设置 `mediaSource` 指定分享屏幕的类型，`mediaSource` 的选择如下：

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

> Firefox 在 Windows 平台不支持 application 模式。

## <a name="both"></a>同时共享屏幕和开启视频

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

// 创建用于发送视频流的 Client
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

## 示例代码

下面的示例代码实现了同时共享屏幕和发送本地视频流，同时，我们在 GitHub 提供一个开源的[示例项目](https://github.com/AgoraIO/Advanced-Video/tree/master/Web/Agora-Screen-Sharing-Web-Webpack)，你可以[在线体验](https://webdemo.agora.io/agora-web-showcase/examples/Agora-Screen-Sharing-Web/)或者下载参考  [`rtc-client.js`](https://github.com/AgoraIO/Advanced-Video/blob/master/Web/Agora-Screen-Sharing-Web-Webpack/src/rtc-client.js) 和 [`index.js`](https://github.com/AgoraIO/Advanced-Video/blob/master/Web/Agora-Screen-Sharing-Web-Webpack/src/index.js) 文件的源代码。

<div class="alert note">下面的代码用了 <code>isFirefox</code> 和 <code>isCompatibleChrome</code> 来判断浏览器类型，你需要自己实现，也可以参考 <a href="https://github.com/AgoraIO/Advanced-Video/blob/master/Web/Agora-Screen-Sharing-Web-Webpack/src/common.js#L29"><code>common.js</code></a> 中的代码。</div>

```javascript
//TODO: 填入你的项目的 App ID
var appID = "<yourAppID>";
var channel = "screen_video";
var channelKey = null;

AgoraRTC.Logger.setLogLevel(AgoraRTC.Logger.INFO);

var localStreams = [];

var screenClient = AgoraRTC.createClient({
    mode: 'rtc',
    codec: 'vp8'
});
screenClient.init(appID, function() {
    screenClient.join(channelKey, channel, null, function(uid) {
        // 保存本地流的uid
        localStreams.push(uid);
        // 创建屏幕共享流
        const streamSpec = {
            streamID: uid,
            audio: false,
            video: false,
            screen: true
          }
          // 根据浏览器类型设置相关属性
          // 注意你需要自己实现判断浏览器的功能
          if (isFirefox()) {
            streamSpec.mediaSource = 'window';
          } else if (!isCompatibleChrome()) {
            streamSpec.extensionId = 'minllpmhdgpndnkomcoccfekfegnlikg';
          }
        screenStream = AgoraRTC.createStream(streamSpec);
        // 初始化流
        screenStream.init(function() {
            // 播放流
            screenStream.play('Screen');
            // 发布流
            screenClient.publish(screenStream);

            // 监听流（用户）加入频道事件
            screenClient.on('stream-added', function(evt) {
                var stream = evt.stream;
                var uid = stream.getId()

                // 收到流加入频道的事件后，先判定是不是本地的uid
                if (!localStreams.includes(uid)) {
                    console.log('subscribe stream:' + uid);
                    // 订阅流
                    screenClient.subscribe(stream);
                }
            })

        }, function(err) {
            console.log(err);
        });

    }, function(err) {
        console.log(err);
    })
});

var videoClient = AgoraRTC.createClient({
    mode: 'rtc',
    codec: 'vp8'
});
videoClient.init(appID, function() {
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
            videoClient.on('stream-added', function(evt) {
                var stream = evt.stream;
                var uid = stream.getId();
                // 收到流加入频道的事件后，先判定是不是本地的uid
                if (!localStreams.includes(uid)) {
                    console.log('subscribe stream:' + uid);
                    // 订阅流
                    screenClient.subscribe(stream);
                }
            })
        }, function(err) {
            console.log(err);
        });

    }, function(err) {
        console.log(err);
    })
});
```

## 开发注意事项

- 创建屏幕共享流的时候，`video` 必须设置为 `false`。
- 如果创建了多个 stream，我们建议只将其中一路流的 `audio` 属性设为 `true`。
- 屏幕共享流的 UID 尽量不要固定在同一个值，否则某些场景下同 UID 的共享流可能会引起互踢。
- 在屏幕共享的时候，本地流的 Client **不要订阅本地的屏幕共享流**，否则会增加计费。
- 在 Windows 平台上进行屏幕共享时，如果共享的是 QQ 聊天窗口会导致黑屏。

## 相关链接

实现屏幕共享过程中，你还可以参考如下文档：

- [如何切换屏幕共享流和摄像头视频流？](https://docs.agora.io/cn/faq/switch_screen_camera_web)
