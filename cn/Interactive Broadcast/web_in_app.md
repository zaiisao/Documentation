
---
title: H5 实时直播
description: 
platform: Web
updatedAt: Mon Sep 23 2019 05:46:57 GMT+0800 (CST)
---
# H5 实时直播
## 功能简介

Agora Web SDK 新增 H5 实时直播插件，支持在移动端网页播放音视频流。该功能可以实现通过在社交 app 内（如微信群、微信公众号、钉钉）分享网页链接，让用户在 app 中打开链接就能直接观看实时视频，降低了分享门槛，方便扩大目标受众范围。

### H5 实时直播兼容性

#### **iOS**
在 iOS 平台上，微信使用的是系统内置的 WebView，因此对 H5 实时直播的支持只与 iOS 系统版本有关。

<table>
  <tr>
    <th>iOS 微信</th>
    <th>iOS Safari</th>
  </tr>
  <tr>
    <td bgcolor="grey"><font color="white">iOS 11.0 以前不支持</font></td>
    <td bgcolor="grey"><font color="white">iOS 11.0 以前不支持</font></td>
  </tr>
  <tr>
    <td bgcolor="#3ab7f8"><font color="white">iOS 11.0 及以后支持</font></td>
    <td bgcolor="#3ab7f8"><font color="white">iOS 11.0 及以后支持</font></td>
  </tr>
</table>

#### **Android**
Android 平台支持自定义 WebView，Android 微信使用的是自研的 WebView，因此对 H5 实时直播的支持与应用版本有关。

<table>
  <tr>
    <th>Android 微信</th>
    <th>Android Chrome</th>
  </tr>
  <tr>
    <td bgcolor="grey"><font color="white">微信 7.0 以前不支持</font></td>
    <td bgcolor="grey"><font color="white">Chrome 58.0 以前不支持</font></td>
  </tr>
  <tr>
    <td bgcolor="#3ab7f8"><font color="white">微信 7.0 及以后支持</font></td>
    <td bgcolor="#3ab7f8"><font color="white">Chrome 58.0 及以后支持</font></td>
  </tr>
</table>

#### **发送端**

使用 H5 实时直播插件观看视频对发送端有以下要求：

- 支持 Agora Native SDK 和 Chrome 浏览器上的 Agora Web SDK 作为发送端，不支持 Safari、Firefox 等其他浏览器。
- 请确保发送端使用直播模式。
- 如果发送端也使用 Agora Web SDK，请确保 `createClient` 中的 `codec` 设置为 `"h264"`。
- 如果使用 Agora Native SDK 2.3.2 或以后版本，必须在加入频道前调用 `setParameters` 进行以下设置：
  ```cpp
setParameters("{\"che.hardware_encoding\":0}")
setParameters("{\"che.video.h264Profile\":66}")
	```
- 发送的视频分辨率设置不要超过 480P。

## 实现方法

### 下载并集成 SDK

下载[包含 H5 实时直播插件的 Web SDK](http://download.agora.io/sdk/release/rts-v2.8.0.zip)。集成方式与原有的 Agora Web SDK 相同，请参考[集成客户端](https://docs.agora.io/cn/Interactive%20Broadcast/web_prepare?platform=Web)。

### 检查系统兼容性

检查系统是否支持 H5 实时直播：

```javascript
AgoraRTS.checkSystemRequirements()
```

调用该方法返回 `true` 表示支持，返回 `false` 表示不支持。

### 引入 H5 实时直播插件

在创建 Client 后，需要加上两行代码：

```javascript
var client = AgoraRTC.createClient({ mode: "live", codec: "h264"});
AgoraRTS.init(AgoraRTC);
AgoraRTS.proxy(client);
```

> - 请确保 `createClient` 中 `mode` 设为 `"live"`， `codec` 设为 `"h264"`。
> - 请确保在创建 Client 之后立即引入 H5 实时直播插件。

`AgoraRTS.init(AgoraRTC);` 这行代码用于初始化 H5 实时直播插件。
`AgoraRTS.proxy(client);` 这行代码通过代理 Client 中订阅相关的方法，支持对订阅的流软件解码。

引入插件之后，可以按照原来的方法初始化 Client 并加入频道。需要注意的是，在和订阅流相关的事件中，SDK 返回的音视频流都是 rtsStream 对象，例如：

- 新增远端流

 ```javascript
client.on("stream-added", function(e) {
    var stream = e.stream; // 该 stream 为 rtsStream
    client.subscribe(stream, { video: true, audio: true});
});
```

- 已订阅远端流

 ```javascript
client.on("stream-subscribed", function (e) {
    var stream = e.stream; // 该 stream 为 rtsStream
});
```

rtsStream 对象是一个特殊的用于接收和播放软解码流的对象，与 SDK 原有的 Stream 对象完全不同。

- 原有 SDK 中的 Stream 对象实际是封装了 WebRTC 的 [MediaStream](https://developer.mozilla.org/zh-CN/docs/Web/API/MediaStream) 音视频流。 
- rtsStream 通过我们实现的软件解码器输出音视频流，不支持 MediaStream 相关的方法和功能。

rtsStream 对象中的 API 是完全对照 SDK 原有 Stream 的 API 实现的，但是只实现了与订阅流相关的方法，详见[开发注意事项](#audience)。

### 订阅 rtsStream

在订阅 rtsStream 对象时，必须设置 [`subscribe`](https://docs.agora.io/cn/Voice/API%20Reference/web/interfaces/agorartc.client.html#subscribe) 方法的 `options` 参数，例如：

```javascript
client.subscribe(stream, { video: true, audio: true }, console.log);
```

## 工作原理

Agora Web SDK 是基于 WebRTC 实现音视频通信的，因此依赖于微信浏览器对 WebRTC 的支持。

尽管微信浏览器支持 WebRTC，但是在不同的平台上，微信浏览器对解码的支持仍有限制，无法解码也就无法观看接收的视频。

微信浏览器对解码格式的支持如下表所示：

|       | Android | iOS                   |
| :---- | :------ | :-------------------- |
| H.264 | 不支持  | 12.1.4 及以后版本支持 |
| VP8   | 支持    | 12.2 及以后版本支持   |

从表中可以看出，有很多设备无法支持解码，因此我们在该版本的 SDK 中新增了 H5 实时直播插件实现软件解码。实现软件解码后，可以在 Android 和 iOS 12.1.4 及以前版本支持 H.264 解码。

我们建议对表格中支持解码的平台，使用原有的订阅方式，对表格中不支持解码的平台，通过引入插件订阅 rtsStream。

## <a name="note"></a>开发注意事项

- 在移动网页端观看视频，支持最多订阅两路 480P 或者四路 240P 的视频流。
- 在代理 Client 以后，Client 的事件中，没有 `"active-speaker"`。
- 使用 H5 实时直播插件时，不要调用会长时间阻塞主线程的方法，如 `Window.alert()`。
- 受浏览器策略影响，在 iOS 平台所有的网页端以及 Android 平台的 Chrome 70+ 浏览器上，音频不会自动播放，我们建议通过用户手势触发播放订阅的流，详情请参考 [Autoplay Policy Changes](https://developers.google.com/web/updates/2017/09/autoplay-policy-changes)。
- rtsStream 对象不同于 Agora Web SDK 原有的 Stream 对象：
  - rtsStream 没有事件抛出。
  - rtsStream 不支持 `getTrack` 和 `enableAudioVolumeIndicator` 方法。
  - rtsStream 支持的方法如下：
    - `init`
    - `play`
    - `stop`
    - `isPlaying`
    - `unmuteAudio`
    - `muteAudio`
    - `hasAudio`
    - `getAudioLevel`
    - `setAudioVolume`
    - `unmuteVideo`
    - `muteVideo`
    - `hasVideo`
    - `getId`
    - `getStats`

  - rtsStream 流通过 Canvas + AudioContext 的方式播放，所以没有播放器控制栏，需要你自己实现。
