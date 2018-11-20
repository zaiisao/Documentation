
---
title: Agora Web SDK API
description: 
platform: Web
updatedAt: Tue Sep 25 2018 05:14:28 GMT+0800 (CST)
---
# Agora Web SDK API
# Agora Web SDK API

Agora Web SDK 库包含以下类:

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td>AgoraRTC</td>
<td>用于创建客户端 (Client) 和音视频流 (Stream) 对象</td>
</tr>
<tr><td>Client</td>
<td>提供 AgoraRTC 核心功能的 web 客户端对象</td>
</tr>
<tr><td>Stream</td>
<td>通话中的本地或远程音视频流</td>
</tr>
</tbody>
</table>



## AgoraRTC 方法

用于创建客户端 \(Client\) 和音视频流 \(Stream\) 对象。

### 检查浏览器兼容性（checkSystemRequirements）

```
checkSystemRequirements()
```

该方法检查 Web SDK 对正在使用的浏览器的适配情况。

**Note:** 

你需要在 [创建音视频对象 \(createClient\)](#) 之前调用该方法，用以检查 Web SDK 对正在使用的浏览器的适配情况。

<table>
<colgroup>
<col/>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td><strong>名称</strong></td>
<td><strong>类型</strong></td>
<td><strong>描述</strong></td>
</tr>
<tr><td>返回值</td>
<td>Boolean</td>
<td><ul>
<li>true：Web SDK 与当前使用的浏览器适配</li>
<li>false：Web SDK 与当前使用的浏览器不适配</li>
</ul>
</td>
</tr>
</tbody>
</table>



**Note:** 

对于一些 Chrome 内核的浏览器（如：QQ 浏览器，360 浏览器等），我们暂时未做全量测试，如需使用，可以进行尝试。在接下来的数个版本中，我们会逐渐完成大部分主流浏览器的适配与测试。

### 创建音视频对象 \(createClient\)

```
createClient(<config>)
```

该方法用于创建客户端。该代码在每次会话里仅调用一次。包含以下属性：

<table>
<colgroup>
<col/>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td><strong>名称</strong></td>
<td><strong>类型</strong></td>
<td><strong>描述</strong></td>
</tr>
<tr><td>config</td>
<td>Object</td>
<td><p>包含以下属性：</p>
<div><ul>
<li>mode：频道模式，可在两种场景中选择：<ul>
<li>live：直播模式（与 Native SDK 互通）</li>
<li>rtc：通信模式（暂不与 Native SDK 互通）</li>
</ul>
</li>
<li>codec：浏览器使用的编解码 Codec，有以下两种选择：<ul>
<li>vp8：浏览器使用 VP8 编解码</li>
<li>h264：浏览器使用 H264 编解码</li>
</ul>
</li>
<li>proxyServer：(可选) 设有企业防火墙的用户使用该参数，可以部署 Nginx 服务器，以将信令消息通过 Nginx 服务器传到 Agora SD-RTN，然后使用 Agora 的服务，详细用法见 <a href="../../cn/Video/live_video_web.html.md"><span>设置代理服务器</span></a> 。</li>
<li>turnServer：(可选) 设有企业防火墙的用户使用该参数，可以部署 TURN 服务器，以将音视频数据通过 TURN 服务器传到 Agora SD-RTN，然后使用 Agora 的服务，详细用法见 <a href="../../cn/Video/live_video_web.html.md"><span>设置代理服务器</span></a> 。</li>
</ul>
</div>
</td>
</tr>
</tbody>
</table>

对于使用 2.3 版本之前的用户，新老版本的兼容关系如下：

- `createClient({mode: "interop"})` = `createClient({mode: "live", codec: "vp8"})`
- `createClient({mode: "h264_interop"})` = `createClient({mode: "live", codec: "h264"})`

> - 该接口的 mode 参数不能缺省。
> - 如果场景中有 Safari 浏览器，无论何种模式，请将 `codec` 参数 设为 `h264` 。
> - 如果需要与 Native SDK 互通，请将 `mode` 参数设为 `live` 。
> - `mode` 参数设为 `rtc` 时，不支持 Agora 录制 SDK。
> - `mode` 参数设为 `live` 时，直播场景下还需要在 Native SDK 端调用 `enableWebSdkInteroperability` 接口。

#### 设置代理服务器

```
var config = {
  mode: 'live',
  codec: 'vp8',
  proxyServer: 'YOUR NGINX PROXY SERVER IP',
  turnServer: {
      turnServerURL: 'YOUR TURNSERVER URL',
      username: 'YOUR USERNAME',
      password: 'YOUR PASSWORD',
      udpport: 'THE UDP PORT YOU WANT TO ADD',
      tcpport: 'THE TCP PORT YOU WANT TO ADD',
      forceturn: false
  }
}
var client = AgoraRTC.createClient(config);
```

**Note:** 

- 在使用 Firefox 浏览器时，跨运营商代理速度会比较慢。Agora 建议使用同运营商进行代理。如果必须使用跨运营商代理，则避免使用 Firefox 浏览器。
- 请确保在 [加入 AgoraRTC 频道 \(join\)](../../cn/API%20Reference/live_video_web.md) 方法前设置该参数。

完整的网页端部署 Proxy 服务器指南，请参考 [进阶：企业部署代理服务器](../../cn/Quickstart%20Guide/proxy_web.md) 。

### 枚举系统设备 \(getDevices\)

```
var getDevices = function(onSuccess, onFailure)
```

该方法枚举系统中的摄像头或麦克风设备。

<table>
<colgroup>
<col/>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td><strong>名称</strong></td>
<td><strong>类型</strong></td>
<td><strong>描述</strong></td>
</tr>
<tr><td>onSuccess</td>
<td>Function</td>
<td>方法调用成功</td>
</tr>
<tr><td>onFailure</td>
<td>Function</td>
<td>方法调用不成功</td>
</tr>
</tbody>
</table>



该方法如果调用成功，会返回与麦克风和摄像头对应的对象，其中每个对象都包含 3 个属性：

<table>
<colgroup>
<col/>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td>属性</td>
<td>类型</td>
<td>描述</td>
</tr>
<tr><td>deviceId</td>
<td>String</td>
<td>该设备所独有的设备 ID</td>
</tr>
<tr><td>label</td>
<td>String</td>
<td>能够区分设备的设备名字。如果用户没有打开摄像头和麦克风的权限，label 就会被设为一个空字节</td>
</tr>
<tr><td>kind</td>
<td>String</td>
<td>设备类型，根据选用的音频输入和视频输入设备，有 <em>audioInput</em> 和 <em>videoInput</em> 两种类型</td>
</tr>
</tbody>
</table>



示例代码

```
AgoraRTC.getDevices (function(devices) {
var devCount = devices.length;

var id = devices[0].deviceId;
});
```

### 创建音视频流对象 \(createStream\)

```
createStream(<spec>)
```

该方法创建并返回音视频流对象。

该对象包含以下属性:

<table>
<colgroup>
<col/>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td><strong>名称</strong></td>
<td><strong>类型</strong></td>
<td><strong>描述</strong></td>
</tr>
<tr><td>spec</td>
<td>Object</td>
<td><p>该对象包含以下属性：</p>
<ul>
<li>streamID：number 类型，音视频流 ID，通常设置为 uid，可通过 <code><span>client.join</span></code> 方法获取</li>
<li>audio：boolean 类型，指定该音视频流是否包含音频资源</li>
<li>video：boolean 类型，指定该音视频流是否包含视频资源 <a href="#f1" id="id3">[1]</a></li>
<li>screen：boolean 类型，指定该音视频流是否包含屏幕共享功能 <a href="#f1" id="id4">[1]</a></li>
<li>audioSource：Object 类型，指定音视频流的音频源</li>
<li>videoSource：Object 类型，指定音视频流的视频源</li>
<li>mirror：boolean 类型，指定本地视频流在本地显示的时候是否为镜像（不适用于屏幕共享），默认为 <code><span>true</span></code>。建议在使用前置摄像头时开启，使用后置摄像头时关闭</li>
<li>mediaSource：string 类型，如果你使用的是 Firefox 浏览器，设置该属性可以开启屏幕共享。详细用法见 <a href="../../cn/Video/live_video_web.html.md"><span>Firefox 浏览器开启屏幕共享</span></a> 。该属性包含三个参数：<ul>
<li>windows：共享某一个应用的某一个窗口</li>
<li>screen：共享屏幕</li>
<li>application: 共享某一个应用的所有窗口</li>
</ul>
</li>
<li>extensionId：string 类型，安装 Chrome 扩展插件后，插件会在本地生成一个对应的扩展 ID，详见 <a href="../../cn/Quickstart%20Guide/chrome_screensharing_plugin.html.md"><span>安装插件并获取插件 ID</span></a> 。</li>
<li>audioProcessing：Object 类型，指定是否开启音效处理，包含以下属性：<ul>
<li>AGC：boolean 类型，自动增益控制，默认为 <code><span>true</span></code>，即开启。如果你不需要启用自动增益控制，将 AGC 设为 <code><span>false</span></code>，即：audioProcessing: {AGC: false}</li>
<li>AEC：boolean 类型，声学回声消除，默认为 <code><span>true</span></code>，即开启。如果你不需要启用声学回声消除，将 AEC 设为 <code><span>false</span></code>，即：audioProcessing: {AEC: false}</li>
<li>ANS：boolean 类型，自动噪声抑制，默认为 <code><span>true</span></code>，即开启。如果你不需要启用自动噪声抑制，将 ANS 设为 <code><span>false</span></code>，即：audioProcessing: {ANS: false}</li>
</ul>
</li>
<li>attributes：(可选) 包含以下属性：<ul>
<li>resolution: 分辨率</li>
<li>minFrameRate: 最小视频帧率</li>
<li>maxFrameRate: 最大视频帧率</li>
</ul>
</li>
<li>cameraId：string 类型，通过 getDevices 方法获取的摄像头设备 ID</li>
<li>microphoneId：string 类型，通过 getDevices 方法获取的麦克风设备 ID</li>
</ul>
</td>
</tr>
</tbody>
</table>



**Note:** 不要同时将 video 和 screen 设置为 *true*。

**示例代码**

#### 创建包含音频和视频资源的流

创建音视频流有两种方法：

##### 设置 audio、 video 及 screen 属性

```
var stream = AgoraRTC.createStream({
  streamID: uid,
  audio:true,
  video:true,
  screen:false
});
```

##### 设置 audioSource 和 videoSource 属性

与前一种方法相比，设置 audioSource 和 videoSource 属性可以指定创建的音视频流要使用的音视频 Track。使用这种方法可以在创建音视频流之前对音视频进行处理。

通过 mediaStream 方法从 MediaStreamTrack 获得音视频 Track，然后指定 audioSource 和 videoSource，如下所示：

```
navigator.mediaDevices.getUserMedia(
    {video: true, audio: true}
).then(function(mediaStream){
    var videoSource = mediaStream.getVideoTracks()[0];
    var audioSource = mediaStream.getAudioTracks()[0];
    // 对 videoSource 和 audioSource 进行处理后
    var localStream = AgoraRTC.createStream({
        videoSource: videoSource,
        audioSource: audioSource
    });
    localStream.init(function(){
        client.publish(localStream, function(e){
            //...
        });
    });
});
```

**Note:** 

- MediaStreamTrack 对象是指浏览器原生支持的 MediaStreamTrack 对象，具体用法和浏览器支持状况请参考 [MediaStreamTrack API 说明](https://developer.mozilla.org/zh-CN/docs/Web/API/MediaStreamTrack)
- 目前音视频前处理功能仅支持 Chrome 浏览器。

#### Chrome 浏览器开始屏幕共享

```
var stream = AgoraRTC.createStream({
  streamID: uid,
  audio:false,
  video:false,
  screen:true,
  extensionId:"minllpmhdgpndnkomcoccfekfegnlikg"
});
```

#### Firefox 浏览器开启屏幕共享

```
localStream = AgoraRTC.createStream({
     streamID: uid,
     audio: false,
     video: false,
     screen: true,
     mediaSource: 'screen',
   });
```

完整的网页端屏幕共享指南，请参考 [进阶：实现网页端屏幕共享](../../cn/Quickstart%20Guide/screensharing_web.md) 。

**Note:** 

- 不要同时将 video 和 screen 设置为 *true*。
- 使用 Firefox 浏览器屏幕共享功能时，请确保 screen 参数设置为 *true*，并且通过 mediaSource 参数指定屏幕共享类型。

### 设置日志级别 \(setLogLevel\)

```
AgoraRTC.Logger.setLogLevel(AgoraRTC.Logger.(logLevel));
```

该方法设置 SDK 的输出日志级别。

<table>
<colgroup>
<col/>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td><strong>名称</strong></td>
<td><strong>类型</strong></td>
<td><strong>描述</strong></td>
</tr>
<tr><td>logLevel</td>
<td>Number</td>
<td><p>开发者设置的日志过滤级别，其中：</p>
<ul>
<li>logLevel = NONE：不输出日志信息</li>
<li>logLevel = ERROR：输出 ERROR 级别的日志信息</li>
<li>logLevel = WARN：输出 WARN 和 ERROR 级别的日志信息</li>
<li>logLevel = INFO：输出 WARN 和 ERROR 级别的日志信息，以及其它相关的日志信息</li>
<li>logLevel = DEBUG：输出所有 API 日志信息</li>
</ul>
</td>
</tr>
</tbody>
</table>



日志级别顺序依次为 *NONE*、*ERROR*、*WARNING*、*INFO* 和 *DEBUG* 。选择一个级别，你就可以看到在该级别以上所有级别的日志信息。

例如，如果你输入代码 AgoraRTC.Logger.setLogLevel\(AgoraRTC.Logger.INFO\);，就可以看到在 *ERROR* 和 *WARNING* 级别上的所有日志信息。

### 开启日志上传（Logger.enableLogUpload\)

调用本方法开启日志上传，开启后 SDK 的日志会上传到 Agora 的服务器。

日志上传功能默认为关闭状态，如果你需要开启此功能，请确保在所有方法之前调用本方法。

**Note:** 

如果没有成功加入频道，则服务器上无法查看日志信息。

**示例代码**

```
AgoraRTC.Logger.enableLogUpload();
```

### 关闭日志上传（Logger.disableLogUpload）

该方法用于关闭日志上传。

日志上传默认是关闭状态，如果你调用了 [开启日志上传（Logger.enableLogUpload\)](#)，可以通过本方法停止上传日志。

**示例代码**

```
AgoraRTC.Logger.disableLogUpload();
```

## Client 方法和回调

提供 AgoraRTC 核心功能的 web 客户端对象。

### Client 方法

#### 初始化客户端对象 \(init\)

```
init(<appId>, <onSuccess>, <onFailure>)
```

使用该方法初始化客户端对象。

示例代码

```
client.init(appId, function() {
    console.log("client initialized");
    //join channel
    //……
}, function(err) {
    console.log("client init failed ", err);
    //error handling
});
```

#### 加入 AgoraRTC 频道 \(join\)

```
join(<token/channelKey>, <channel>, <uid>, <onSuccess>, <onFailure>)
```

该方法让用户加入 AgoraRTC 频道。

示例代码

```
client.join(<token>, '1024', null, function(uid) {
    console.log("client" + uid + "joined channel");
    //create local stream
    //……
}, function(err) {
    console.error("client join failed ", err);
    //error handling
});
```

#### 离开 AgoraRTC 频道 \(leave\)

```
leave(<onSuccess>, <onFailure>)
```

该方法让用户离开 AgoraRTC 频道。

<table>
<colgroup>
<col/>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td><strong>名称</strong></td>
<td><strong>类型</strong></td>
<td><strong>描述</strong></td>
</tr>
<tr><td>onSuccess</td>
<td>Function</td>
<td>（非必选项）方法调用成功时执行的回调函数</td>
</tr>
<tr><td>onFailure</td>
<td>Function</td>
<td>（非必选项）方法调用失败时执行的回调函数</td>
</tr>
</tbody>
</table>

示例代码

```
client.leave(function() {
    console.log("client leaves channel");
    //……
}, function(err) {
    console.log("client leave failed", err);
    //error handling
});
```

#### 发布本地音视频流\(publish\)

```
publish(<stream>, <onFailure>)
```

该方法将本地音视频流发布至 SD-RTN。

**Note:** 

在直播场景里，调用该 API 的用户即为主播。

<table>
<colgroup>
<col/>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td><strong>名称</strong></td>
<td><strong>类型</strong></td>
<td><strong>描述</strong></td>
</tr>
<tr><td>stream</td>
<td>Object</td>
<td>本地音视频流对象</td>
</tr>
<tr><td>onFailure</td>
<td>Function</td>
<td>(非必选项)方法调用失败时执行的回调函数</td>
</tr>
</tbody>
</table>

示例代码

```
client.publish(stream, function(err) {
    console.log("stream published");
    //……
})
```

#### 取消发布本地音视频流 \(unpublish\)

```
unpublish(<stream>, <onFailure>)
```

该方法取消发布本地音视频流。

<table>
<colgroup>
<col/>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td><strong>名称</strong></td>
<td><strong>类型</strong></td>
<td><strong>描述</strong></td>
</tr>
<tr><td>stream</td>
<td>Object</td>
<td>本地音视频流对象</td>
</tr>
<tr><td>onFailure</td>
<td>Function</td>
<td>(非必选项)方法调用失败时执行的回调函数</td>
</tr>
</tbody>
</table>

示例代码

```
client.unpublish(stream, function(err) {
    console.log("stream unpublished");
    //……
})

```

#### 订阅远程音视频流 \(subscribe\)

```
subscribe(<stream>, <onFailure>)
```

该方法从服务器端接收远程音视频流。

<table>
<colgroup>
<col/>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td><strong>名称</strong></td>
<td><strong>类型</strong></td>
<td><strong>描述</strong></td>
</tr>
<tr><td>stream</td>
<td>Object</td>
<td>远端音视频流对象</td>
</tr>
<tr><td>onFailure</td>
<td>Function</td>
<td>(非必选项)方法调用失败时执行的回调函数</td>
</tr>
</tbody>
</table>

示例代码

```
client.subscribe(stream, function(err) {
    console.error("stream subscribe failed", err);
    //……
})

```

#### 取消订阅远程音视频流 \(unsubscribe\)

```
unsubscribe(<stream>, <onFailure>)

```

该方法取消接收远程音视频流。

<table>
<colgroup>
<col/>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td><strong>名称</strong></td>
<td><strong>类型</strong></td>
<td><strong>描述</strong></td>
</tr>
<tr><td>stream</td>
<td>Object</td>
<td>远端音视频流对象</td>
</tr>
<tr><td>onFailure</td>
<td>Function</td>
<td>(非必选项)方法调用失败时执行的回调函数</td>
</tr>
</tbody>
</table>

示例代码

```
client.unsubscribe(stream, function(err) {
    console.log("stream unsubscribed");
    //……
});

```

#### 开启双流模式 \(enableDualStream\)

```
enableDualStream(<onSuccess>, <onFailure>)
```

该方法在 Publish 端，即推流端，开启双流模式。

<table>
<colgroup>
<col/>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td><strong>名称</strong></td>
<td><strong>类型</strong></td>
<td><strong>描述</strong></td>
</tr>
<tr><td>onSuccess</td>
<td>Function</td>
<td>（非必选项）成功时执行的回调函数</td>
</tr>
<tr><td>onFailure</td>
<td>Function</td>
<td>（非必选项）失败时执行的回调函数</td>
</tr>
</tbody>
</table>

示例代码

```javascript
client.enableDualStream(function() {
  console.log('Enable dual stream success!')
}, function(err) {
  console,log(err)
})
```

该方法对以下场景无效：

- 音频通话 （audio: true, video: false）
- Safari 浏览器
- 共享屏幕的场景

#### 设置小流视频参数 \(setLowStreamParameter\)

```
client.setLowStreamParameter({
  width: 320,
  height: 240,
  framerate: 15,
  bitrate: 200,
})
```

如果你开启了双流模式，该方法设置小流的视频参数。

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td><strong>参数</strong></td>
<td><strong>描述</strong></td>
</tr>
<tr><td>width</td>
<td>（可选）小流视频帧的宽度。<em>width</em> 和 <em>height</em> 参数互相绑定，只有两个都填才有效，否则视为缺省，SDK 会自动适配一个默认的值</td>
</tr>
<tr><td>height</td>
<td>（可选）小流视频帧的高度。<em>width</em> 和 <em>height</em> 参数互相绑定，只有两个都填才有效，否则视为缺省，SDK 会自动适配一个默认的值</td>
</tr>
<tr><td>framerate</td>
<td>（可选）小流视频帧的帧率。如果不填，则视为缺省，SDK 会自动适配一个默认的值</td>
</tr>
<tr><td>bitrate</td>
<td>（可选）小流视频帧的码率。如果不填，则视为缺省，SDK 会自动适配一个默认的值</td>
</tr>
</tbody>
</table>

**Note:** 

- 由于不同的浏览器对于视频参数设置有不同的限制，通过该接口设置的视频参数不一定都会生效。目前发现的未能充分适配的浏览器有 Firefox 浏览器，对其设置帧率不生效，浏览器本身会把帧率固定在 30 fps
- 由于设备和浏览器的限制，部分浏览器设置视频分辨率可能不会生效，这种情况下浏览器会自动调整分辨率，计费也将按照实际分辨率计算。
- 该方法必须在 [加入 AgoraRTC 频道 \(join\)](../../cn/API%20Reference/live_video_web.md) 方法之后调用才能生效
- 屏幕共享只支持大流模式

#### 设置视频大小流 \(setRemoteVideoStreamType\)

```
setRemoteVideoStreamType(<stream>, <streamType>);
```

如果发送端选择发送视频双流 \(大流或小流\)，该方法可以在订阅端选择接收大流还是小流。如果不设置，订阅端默认接收大流。

<table>
<colgroup>
<col/>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td><strong>名称</strong></td>
<td><strong>类型</strong></td>
<td><strong>描述</strong></td>
</tr>
<tr><td>stream</td>
<td>Object</td>
<td>远端视频流对象</td>
</tr>
<tr><td>streamType</td>
<td>Number</td>
<td><p>设置远端视频流大小。视频流类型如下：</p>
<blockquote>
<div><ul>
<li>0：请求视频大流</li>
<li>1：请求视频小流</li>
</ul>
</div></blockquote>
</td>
</tr>
</tbody>
</table>

示例代码

```
switchStream = function (){
  if (highOrLow === 0) {
    highOrLow = 1
    console.log("Set to low");
  }
  else {
    highOrLow = 0
    console.log("Set to high");
  }

  client.setRemoteVideoStreamType(stream, highOrLow);
}
```

某些浏览器对双流模式不完全适配。目前发现的适配问题有：

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td>浏览器</td>
<td>可能出现的问题</td>
</tr>
<tr><td>Firefox</td>
<td>小流帧率固定为 30 fps</td>
</tr>
<tr><td>macOS Safari</td>
<td>大小流帧率、分辨率一致</td>
</tr>
<tr><td>iOS Safari</td>
<td>Safari 11 当前不支持大小流切换</td>
</tr>
</tbody>
</table>



#### 设置音视频流回退策略 \(setStreamFallbackOption\)

```
setStreamFallbackOption(<stream>, <fallbackType>)
```

该接口用于设置订阅端在弱网情况下音视频流的回退策略。网络不理想的情况下，为保证通话质量，可以选择自动订阅视频小流或者仅订阅音频流。

**Note:** 

该接口只可在发送端 [开启双流模式 \(enableDualStream\)](../../cn/API%20Reference/live_video_web.md) 的情况下使用。

<table>
<colgroup>
<col/>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td><strong>参数</strong></td>
<td><strong>类型</strong></td>
<td><strong>描述</strong></td>
</tr>
<tr><td>stream</td>
<td>Object</td>
<td>远端音视频流对象</td>
</tr>
<tr><td>fallbackType</td>
<td>Number</td>
<td><blockquote>
<div>回退选项：</div></blockquote>
<ul>
<li>0: 关闭回退策略，弱网时不对音视频流作回退处理。</li>
<li>1: 弱网时允许自动订阅视频小流。</li>
<li>2: 弱网时允许自动订阅视频小流或只订阅音频流。</li>
</ul>
</td>
</tr>
</tbody>
</table>

#### 关闭双流模式 \(disableDualStream\)

```
disableDualStream(<onSuccess>, <onFailure>)
```

该方法关闭双流模式。

<table>
<colgroup>
<col/>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td><strong>名称</strong></td>
<td><strong>类型</strong></td>
<td><strong>描述</strong></td>
</tr>
<tr><td>onSuccess</td>
<td>Function</td>
<td>（非必选项）成功时执行的回调函数</td>
</tr>
<tr><td>onFailure</td>
<td>Function</td>
<td>（非必选项）失败时执行的回调函数</td>
</tr>
</tbody>
</table>

示例代码

```
client.disableDualStream(function() {
  console.log('Disable dual stream success!')
}, function(err) {
  console.log(err)
})
```

#### 启用说话者音量提示 (enableAudioVolumeIndicator)

```
enableAudioVolumeIndicator()
```

该方法允许 SDK 定期向应用程序反馈当前谁在说话以及说话者的音量。 

启用该方法后，无论频道中有没有人说话，SDK 都会在说话者音量提示 (volume-indicator) 回调中每两秒返回音量提示。

Note

- 建议在 `AgoraRTC.createStream` 时同时开启 DTX
- 在同一台 PC 上 开多个通话页面可能会导致该功能不准或失效

示例代码

```javascript
client.enableAudioVolumeIndicator(); //每2秒
```

#### 配置直播旁路推流 \(configPublisher\)

```
configPublisher(<width>, <height>, <framerate>, <bitrate>, <publisherUrl>)

```

该方法在加入频道前配置旁路直播推流服务。

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td><strong>名称</strong></td>
<td><strong>描述</strong></td>
</tr>
<tr><td>width</td>
<td>设置旁路直播的输出码流的宽度。默认值为 360</td>
</tr>
<tr><td>height</td>
<td>设置旁路直播的输出码流的高度。默认值为 640</td>
</tr>
<tr><td>framerate</td>
<td>设置旁路直播的输出码流的帧率。默认值为 15 fps</td>
</tr>
<tr><td>bitrate</td>
<td>设置旁路直播的输出码流的码率。默认值为 500 Kbps</td>
</tr>
<tr><td>publisherUrl</td>
<td>设置合图的推流地址，默认为 null</td>
</tr>
</tbody>
</table>



代码示例

```
client.configPublisher（{
  width: width,
  height: height,
  framerate: framerate,
  bitrate: bitrate,
  publishUrl: "rtmp://xxx/xxx/"
});

```

#### 新建直播流 \(startLiveStreaming\)

```
startLiveStreaming(<url>, <enableTranscoding>);

```

该方法新建直播流。

<table>
<colgroup>
<col/>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td><strong>名称</strong></td>
<td><strong>类型</strong></td>
<td><strong>描述</strong></td>
</tr>
<tr><td>url</td>
<td>String</td>
<td>（必选项）直播推流的地址</td>
</tr>
<tr><td>enableTranscoding</td>
<td>Boolean</td>
<td>(非必选项) 是否启用直播转码</td>
</tr>
</tbody>
</table>



示例代码

```
client.setLiveTranscoding(<coding>);
//如果 enableTranscoding 设为 true, 需在 startLiveStreaming 之前调用 setLiveTranscoding
client.startLiveStreaming(<url>, true)

```

#### 设置直播转码 \(setLiveTranscoding\)

```
setLiveTranscoding(<coding>);

```

该方法设置直播转码。

<table>
<colgroup>
<col/>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td><strong>名称</strong></td>
<td><strong>类型</strong></td>
<td><strong>描述</strong></td>
</tr>
<tr><td>coding</td>
<td>Object</td>
<td>(必选项)直播转码的设置</td>
</tr>
</tbody>
</table>



示例代码

```
var LiveTranscoding = {
  width: 640,
  height: 360,
  videoBitrate: 400,
  videoFramerate: 15,
  lowLatency: false,
  audioSampleRate: AgoraRTC.AUDIO_SAMPLE_RATE_48000,
  audioBitrate: 48,
  audioChannels: 1,
  videoGop: 30,
  videoCodecProfile: AgoraRTC.VIDEO_CODEC_PROFILE_HIGH,
  userCount: 0,
  userConfigExtraInfo: {},
  backgroundColor: 0x000000,
  transcodingUsers: [],
};

```

<table>
<colgroup>
<col/>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td><strong>属性</strong></td>
<td><strong>类型</strong></td>
<td><strong>描述</strong></td>
</tr>
<tr><td>width</td>
<td>Integer</td>
<td>画布宽度，默认 640</td>
</tr>
<tr><td>height</td>
<td>Integer</td>
<td>画布高度，默认 360</td>
</tr>
<tr><td>videoBitrate</td>
<td>Integer</td>
<td>旁路直播输出视频流码率，默认 400 kbps</td>
</tr>
<tr><td>videoFramerate</td>
<td>Integer</td>
<td>旁路直播输出视频流帧率，默认 15 fps</td>
</tr>
<tr><td>lowLatency</td>
<td>Boolean</td>
<td>开启低延迟，默认 false</td>
</tr>
<tr><td>audioSampleRate</td>
<td>Integer</td>
<td><p>音频采样率，默认 48 kHz</p>
<blockquote>
<div><ul>
<li>AgoraRTC.AUDIO_SAMPLE_RATE_32000 (32 kHz)</li>
<li>AgoraRTC.AUDIO_SAMPLE_RATE_44100 (44.1 kHz)</li>
<li>AgoraRTC.AUDIO_SAMPLE_RATE_48000 (48 kHz)</li>
</ul>
</div></blockquote>
</td>
</tr>
<tr><td>audioBitrate</td>
<td>Integer</td>
<td>音频码率，默认 48</td>
</tr>
<tr><td>audioChannels</td>
<td>Integer</td>
<td>音频通道数量，默认 1</td>
</tr>
<tr><td>videoGop</td>
<td>Integer</td>
<td>视频 GOP，默认 30</td>
</tr>
<tr><td>videoCodecProfile</td>
<td>Integer</td>
<td><p>视频编码类型，默认 high profile (100)，还有 main profile (77) 以及 base profile (66)：</p>
<blockquote>
<div><ul>
<li>AgoraRTC.VIDEO_CODEC_PROFILE_BASELINE</li>
<li>AgoraRTC.VIDEO_CODEC_PROFILE_MAIN</li>
<li>AgoraRTC.VIDEO_CODEC_PROFILE_HIGH</li>
</ul>
</div></blockquote>
</td>
</tr>
<tr><td>userCount</td>
<td>Integer</td>
<td>参与合图的用户数量，默认 0</td>
</tr>
<tr><td>userConfigExtraInfo</td>
<td>Object</td>
<td>预留参数：用户配置的额外信息</td>
</tr>
<tr><td>backgroundColor</td>
<td>Integer</td>
<td>背景色，默认 0x000000</td>
</tr>
<tr><td>transcodingUsers</td>
<td>Array</td>
<td>与用户相关的音视频转码设置。详细定义如下</td>
</tr>
</tbody>
</table>



**TranscodingUser 定义如下**

```
var TranscodingUser =
{ uid: 0, x: 0, y: 0, width: 0, height: 0, zOrder: 0, alpha: 1.0, };

```

<table>
<colgroup>
<col/>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td><strong>属性</strong></td>
<td><strong>类型</strong></td>
<td><strong>描述</strong></td>
</tr>
<tr><td>uid</td>
<td>Integer</td>
<td>CDN 主播的 userID</td>
</tr>
<tr><td>x</td>
<td>Integer</td>
<td>视频帧左上角的横轴位置</td>
</tr>
<tr><td>y</td>
<td>Integer</td>
<td>视频帧左上角的纵轴位置</td>
</tr>
<tr><td>width</td>
<td>Integer</td>
<td>视频帧宽度</td>
</tr>
<tr><td>height</td>
<td>Integer</td>
<td>视频帧高度</td>
</tr>
<tr><td>zOrder</td>
<td>Integer</td>
<td>视频帧所处层数</td>
</tr>
<tr><td>alpha</td>
<td>Double</td>
<td>视频帧的透明度。默认值为 1.0。</td>
</tr>
</tbody>
</table>



#### 删除直播流 \(stopLiveStreaming\)

```
stopLiveStreaming(<url>);

```

该方法停止并删除直播流。

<table>
<colgroup>
<col/>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td><strong>名称</strong></td>
<td><strong>类型</strong></td>
<td><strong>描述</strong></td>
</tr>
<tr><td>url</td>
<td>String</td>
<td>(必选项) 直播推流的地址</td>
</tr>
</tbody>
</table>



示例代码

```
client.stopLiveStreaming(<url>);

```

**Note:** 

- 如需使用直播 1.0 的功能，在 createStream 后调用 configPublisher 方法即可。
- 如需使用直播 2.0 的功能，在 createStream 后调用 startLiveStreaming 和 setLiveTranscoding 方法即可。详见 [进阶：推流到 CDN](../../cn/Quickstart%20Guide/push_stream_web.md) 。

#### 部署 Nginx 服务器 \(setProxyServer\)

```
client.setProxyServer(<domainName>);

```

<table>
<colgroup>
<col/>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td><strong>名称</strong></td>
<td><strong>类型</strong></td>
<td><strong>描述</strong></td>
</tr>
<tr><td>domainName</td>
<td>String</td>
<td>你的 Nginx 服务器域名</td>
</tr>
</tbody>
</table>



**Note:** 

- 在使用 Firefox 浏览器时，跨运营商代理速度会比较慢。Agora 建议使用同运营商进行代理。如果必须使用跨运营商代理，则避免使用 Firefox 浏览器。
- 请确保在 [加入 AgoraRTC 频道 \(join\)](../../cn/API%20Reference/live_video_web.md) 前调用该方法。

#### 部署 TURN 服务器 \(setTurnServer\)

```
client.setTurnServer(<config>);

```

<table>
<colgroup>
<col/>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td><strong>名称</strong></td>
<td><strong>类型</strong></td>
<td><strong>描述</strong></td>
</tr>
<tr><td>config</td>
<td>Object</td>
<td><p>TURN 服务器配置：</p>
<blockquote>
<div><ul>
<li>turnServerURL：你的 TURN 服务器 URL 地址</li>
<li>username：你在 TURN 服务器上注册并使用的用户名</li>
<li>password：你在 TURN 服务器上使用的密码</li>
<li>udpport：你想要添加的 UDP 端口</li>
<li>forceturn：是否启用强制中转：<ul>
<li>true：强制所有流走中转</li>
<li>false：（默认值）不强制走中转</li>
</ul>
</li>
<li>tcpport：（可选）你想要添加的 TCP 端口</li>
</ul>
</div></blockquote>
</td>
</tr>
</tbody>
</table>

**Note:** 

请确保在 [加入 AgoraRTC 频道 \(join\)](../../cn/API%20Reference/live_video_web.md) 之前调用该方法。

#### 设置 AES 加密密码 \(setEncryptionSecret\)

```
client.setEncryptionSecret(<secret>);

```

<table>
<colgroup>
<col/>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td><strong>名称</strong></td>
<td><strong>类型</strong></td>
<td><strong>描述</strong></td>
</tr>
<tr><td>secret</td>
<td>String</td>
<td>加密密码</td>
</tr>
</tbody>
</table>

**Note:** 

请确保在 [加入 AgoraRTC 频道 \(join\)](../../cn/API%20Reference/live_video_web.md) 之前调用该方法。

#### 设置 AES 加密方案 \(setEncryptionMode\)

```
client.setEncryptionMode(<encryptionMode>);

```

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td><strong>名称</strong></td>
<td><strong>描述</strong></td>
</tr>
<tr><td>encrptionMode</td>
<td><p>加密方案，包含以下几种：</p>
<ul>
<li>aes-128-xts：设置 AES128XTS 加密方案</li>
<li>aes-256-xts：设置 AES256XTS 加密方案</li>
<li>aes-128-ecb：设置 AES128ECB 加密方案</li>
</ul>
</td>
</tr>
</tbody>
</table>

**Note:** 

请确保在 [加入 AgoraRTC 频道 \(join\)](../../cn/API%20Reference/live_video_web.md) 之前调用该方法。

#### 更新 Token \(renewToken\)

```
renewToken(<token>)

```

该方法更新 Token。如果启用了 Token 机制，过一段时间后密钥会失效。 当收到 onTokenPrivilegeWillExpire 或 onTokenPrivilegeDidExpire 消息时，调用该 API 以更新 Token，否则 SDK 会无法和服务器建立连接。

<table>
<colgroup>
<col/>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td><strong>名称</strong></td>
<td><strong>类型</strong></td>
<td><strong>描述</strong></td>
</tr>
<tr><td>token</td>
<td>String</td>
<td>指定新的 Token</td>
</tr>
</tbody>
</table>

#### 更新 Channel Key \(renewChannelKey\)

```
renewChannelKey(<channelKey>, <onSuccess>, <onFailure>)

```

该方法更新 Channel Key。如果启用了 Channel Key 机制，过一段时间后密钥会失效。 当 onFailure 回调报告 DYNAMIC\_KEY\_TIMEOUT 时，调用该 API 以更新 Channel Key，否则 SDK 会无法和服务器建立连接。

<table>
<colgroup>
<col/>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td><strong>名称</strong></td>
<td><strong>类型</strong></td>
<td><strong>描述</strong></td>
</tr>
<tr><td>channelKey</td>
<td>String</td>
<td>指定新的 Channel Key</td>
</tr>
<tr><td>onSuccess</td>
<td>Function</td>
<td>（非必选项）方法调用成功时执行的回调函数</td>
</tr>
<tr><td>onFailure</td>
<td>Function</td>
<td>（非必选项）方法调用失败时执行的回调参数</td>
</tr>
</tbody>
</table>



### 回调事件

#### 本地摄像头／麦克风权限已获取 \(accessAllowed/accessDenied\)

该回调通知应用程序已获取本地摄像头／麦克风使用权限。

示例代码

```
localStream.on("accessAllowed", function(){
  console.log("accessAllowed");
})

localStream.on("accessDenied", function(){
   console.log("accessDenied");
})

```

#### 本地音视频已发布回调事件 \(stream-published\)

该回调通知应用程序本地音视频流已发布。

示例代码

```
client.on('stream-published', function(evt) {
    console.log("local stream published");
    //……
})

```

#### 远程音视频流已添加回调事件 \(stream-added\)

该回调通知应用程序远程音视频流已添加。

示例代码

```
client.on('stream-added', function(evt) {
    var stream = evt.stream;
    console.log("new stream added ", stream.getId());
    //subscribe the stream
    //……
})

```

#### 远程音视频流已删除回调事件 \(stream-removed\)

该回调通知应用程序已删除远程音视频流，即对方调用了 unpublish stream。

示例代码

```
client.on('stream-removed', function(evt) {
    var stream = evt.stream;
    console.log("remote stream was removed", stream.getId());
    //……
});

```

#### 远程音视频流已订阅回调事件 \(stream-subscribed\)

该回调通知应用程序已接收远程音视频流。

示例代码

```
client.on('stream-subscribed', function(evt) {
    var stream = evt.stream;
    console.log("new stream subscribed ", stream.getId());
    //play the stream
    //……
})

```

#### 对方用户已离开会议室回调事件 \(peer-leave\)

该回调通知应用程序对方用户已离开频道，即对方调用了 client.leave\(\)。

示例代码

```
client.on('peer-leave', function(evt) {
    var uid = evt.uid;
    console.log("remote user left ", uid);
    //……
});

```

#### 对方用户已开启语音通话静音 \(mute-audio\)

该回调通知应用程序对方用户在语音通话中将自己的声音关掉。

示例代码

```
client.on('mute-audio', function(evt) {
  var uid = evt.uid;
  console.log("mute audio:" + uid);
  //alert("mute audio:" + uid);
});

```

#### 对方用户已取消语音通话静音 \(unmute-audio\)

该回调通知应用程序对方用户在语音通话中将自己的声音打开。

示例代码

```
client.on('unmute-audio', function (evt) {
  var uid = evt.uid;
  console.log("unmute audio:" + uid);
});

```

#### 对方用户已关闭视频 \(mute-video\)

该回调通知应用程序对方用户在视频通话中将自己的视频关掉。

示例代码

```
client.on('mute-video', function (evt) {
  var uid = evt.uid;
  console.log("mute video" + uid);
  //alert("mute video:" + uid);
})

```

#### 对方用户已开启视频 \(unmute-video\)

该回调通知应用程序对方用户在视频通话中将自己的视频打开。

示例代码

```
client.on('unmute-video', function (evt) {
  var uid = evt.uid;
  console.log("unmute video:" + uid);
})

```

#### 用户已被踢且被封禁（client-banned）

该回调通知用户已经在聊天过程中被踢，或没有进入频道就被封禁。当前只有被踢的用户会收到这个回调。

示例代码

```
client.on('client-banned', function (evt) {
   var uid = evt.uid;
   var attr = evt.attr;
   console.log(" user banned:" + uid + ", banntype:" + attr);
   alert(" user banned:" + uid + ", banntype:" + attr);
});

```

#### 提示说话人事件 \(active-speaker\)

该回调通知应用程序频道内谁在说话。

示例代码

```
client.on('active-speaker', function(evt) {
     var uid = evt.uid;
     console.log("update active speaker: client " + uid);
  });

```

#### 说话者音量提示 (volume-indicator)

该回调提示频道内谁在说话以及说话者的音量。默认禁用。可以通过 启用说话者音量提示 (enableAudioVolumeIndicator) 方法开启；开启后，无论频道内是否有人说话，都会定期返回提示音量。

音量范围为0~100之间的整数。通常在列表中音量大于5的用户为持续说话的人。

示例代码

```javascript
client.on("volume-indicator", function(evt){
    evt.attr.forEach(function(volume, index){
    console.log(`#{index} UID ${volume.uid} Level ${volume.level}`);
  });
});
```

#### 推流成功 \(liveStreamingStarted\)

```
client.on('liveStreamingStarted', args);

```

该回调通知应用程序直播推流成功。

#### 推流失败 \(liveStreamingFailed\)

```
client.on('liveStreamingFailed', args);

```

该回调通知应用程序直播推流失败。

#### 停止推流 \(liveStreamingStopped\)

```
client.on('liveStreamingStopped', args);

```

该方法通知应用程序直播推流已停止。

#### 主播转码更新 \(liveTranscodingUpdated\)

```
client.on('liveTranscodingUpdated', args);

```

该方法通知应用程序主播转码已更新。

#### Token 服务即将过期事件 （onTokenPrivilegeWillExpire）

在 Token 过期前 30 秒，会收到该事件通知。

一般情况下，在收到该消息之后，应向服务端重新申请 Token，并调用 [更新 Token \(renewToken\)](../../cn/API%20Reference/live_video_web.md) 方法。

示例代码

```
client.on('onTokenPrivilegeWillExpire', function(){
  //进行重新申请 Token 操作后
  client.renewToken(<token>);
});

```

#### Token 服务已过期事件 （onTokenPrivilegeDidExpire）

在 Token 过期后，会收到该事件通知。

一般情况下，在收到该消息之后，应向服务端重新申请 Token，并调用 [更新 Token \(renewToken\)](../../cn/API%20Reference/live_video_web.md) 方法。

示例代码

```
client.on('onTokenPrivilegeDidExpire', function(){
  //进行重新申请 Token 操作后
  client.renewToken(<token>);
});

```

#### 报错 \(error\)

该回调通知应用程序有出错信息，需要进行处理。

示例代码

```
client.on('error', function(err) {
    console.log("Got error msg:", err.reason);
  });

```

具体错误及原因详见 [错误代码和警告代码](../../cn/API%20Reference/the_error_web.md) 。

## Stream 方法

### 初始化音视频对象 \(init\)

```
init(<onSuccess>, <onFailure>)

```

该方法初始化音视频流对象。

如果调用失败，回调函数会返回错误信息，例如：
`{type: "error", msg: "MEDIA_OPTION_INVALID"}`

可能出现的错误码信息如下：

- MEDIA_OPTION_INVALID: 摄像头被占用或者分辨率不支持（早期浏览器）
- DEVICES_NOT_FOUND: 没有找到设备
- NOT_SUPPORTED: 浏览器不支持获取获取摄像头和麦克风
- PERMISSION_DENIED: 浏览器禁用设备或者用户拒绝打开设备
- CONSTRAINT_NOT_SATISFIED: 配置参数不合法（早期浏览器）
- UNDEFINED: 未定义错误

示例代码

```
localStream.init(function() {
     console.log("local stream initialized");
     // publish the stream
     //……
 }, function(err) {
     console.error("local stream init failed ", err);
     //error handling
 });

```

### 播放音视频流 \(play\)

```
play(<elementID>)


```

该方法播放视频流或音频流。

示例代码

```
stream.play('agora_remote'); // stream will be played in the element with the ID agora_remote


```

### 停止音视频流 \(stop\)

```
stop()


```

该方法停止播放音视频流。如需关闭摄像头或麦克风，请使用 Stream.close。

### 关闭音视频流 \(close\)

```
close()


```

该方法关闭视频流或音频流，停止采集音视频数据。

### 设置音频属性（setAudioProfile）

```
setAudioProfile(<profile>)


```

该方法设置音频属性，为非必选项，且须在 stream.init\(\) 之前调用。默认值为 music\_standard 。

<table>
<colgroup>
<col/>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td><strong>名称</strong></td>
<td><strong>类型</strong></td>
<td><strong>描述</strong></td>
</tr>
<tr><td>profile</td>
<td>String</td>
<td><p>音频属性字符串，包含以下参数：</p>
<blockquote>
<div><ul>
<li>speech_standard: 指定 32 KHz 采样率，单声道，编码码率约 24 kbps</li>
<li>music_standard: 指定 48 KHz 采样率，单声道，编码码率约 40 kbps</li>
<li>standard_stereo: 指定 48 KHz 采样率，双声道，编码码率约 64 kbps</li>
<li>high_quality: 指定 48 KHz 采样率，单声道，编码码率约 128 kbps</li>
<li>high_quality_stereo: 指定 48 KHz 采样率，双声道，编码码率约 192 kbps</li>
</ul>
</div></blockquote>
</td>
</tr>
</tbody>
</table>



**Note:** 

由于各浏览器的限制，部分浏览器对设置的 Audio Profile 不一定能全部适配：

- Firefox 不支持设置音频编码码率
- Safari 不支持双声道功能
- Chrome 当前版本不支持播放双声道音乐，可支持发送双声道音频流

### 设置视频属性 \(setVideoProfile\)

```
setVideoProfile(<profile>)


```

该方法设置视频属性，为非必选项，且须在 stream.init\(\) 之前调用。默认值为 480p\_1 。

<table>
<colgroup>
<col/>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td><strong>名称</strong></td>
<td><strong>类型</strong></td>
<td><strong>描述</strong></td>
</tr>
<tr><td>profile</td>
<td>String</td>
<td>视频属性（Profile）字符串。详见下表的定义。</td>
</tr>
</tbody>
</table>



**视频属性定义**

<table>
<colgroup>
<col/>
<col/>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td><strong>视频属性</strong></td>
<td><strong>分辨率（宽x高）</strong></td>
<td><strong>帧率</strong></td>
<td><strong>码率</strong></td>
</tr>
<tr><td>120P</td>
<td>160x120</td>
<td>15</td>
<td>65</td>
</tr>
<tr><td>120P_1</td>
<td>160x120</td>
<td>15</td>
<td>65</td>
</tr>
<tr><td>120P_3</td>
<td>120x120</td>
<td>15</td>
<td>50</td>
</tr>
<tr><td>180P</td>
<td>320x180</td>
<td>15</td>
<td>140</td>
</tr>
<tr><td>180P_1</td>
<td>320X180</td>
<td>15</td>
<td>140</td>
</tr>
<tr><td>180P_3</td>
<td>180x180</td>
<td>15</td>
<td>100</td>
</tr>
<tr><td>180P_4</td>
<td>240x180</td>
<td>15</td>
<td>120</td>
</tr>
<tr><td>240P</td>
<td>320x240</td>
<td>15</td>
<td>200</td>
</tr>
<tr><td>240P_1</td>
<td>320X240</td>
<td>15</td>
<td>200</td>
</tr>
<tr><td>240P_3</td>
<td>240x240</td>
<td>15</td>
<td>140</td>
</tr>
<tr><td>240P_4</td>
<td>424x240</td>
<td>15</td>
<td>220</td>
</tr>
<tr><td>360P</td>
<td>640x360</td>
<td>15</td>
<td>400</td>
</tr>
<tr><td>360P_1</td>
<td>640X360</td>
<td>15</td>
<td>400</td>
</tr>
<tr><td>360P_3</td>
<td>360x360</td>
<td>15</td>
<td>260</td>
</tr>
<tr><td>360P_4</td>
<td>640x360</td>
<td>30</td>
<td>600</td>
</tr>
<tr><td>360P_6</td>
<td>360x360</td>
<td>30</td>
<td>400</td>
</tr>
<tr><td>360P_7</td>
<td>480x360</td>
<td>15</td>
<td>320</td>
</tr>
<tr><td>360P_8</td>
<td>480x360</td>
<td>30</td>
<td>490</td>
</tr>
<tr><td>360P_9</td>
<td>640x360</td>
<td>15</td>
<td>800</td>
</tr>
<tr><td>360P_10</td>
<td>640x360</td>
<td>24</td>
<td>800</td>
</tr>
<tr><td>360P_11</td>
<td>640x360</td>
<td>24</td>
<td>1000</td>
</tr>
<tr><td>480P</td>
<td>640x480</td>
<td>15</td>
<td>500</td>
</tr>
<tr><td>480P_1</td>
<td>640x480</td>
<td>15</td>
<td>500</td>
</tr>
<tr><td>480P_2</td>
<td>648x480</td>
<td>30</td>
<td>1000</td>
</tr>
<tr><td>480P_3</td>
<td>480x480</td>
<td>15</td>
<td>400</td>
</tr>
<tr><td>480P_4</td>
<td>640x480</td>
<td>30</td>
<td>750</td>
</tr>
<tr><td>480P_6</td>
<td>480x480</td>
<td>30</td>
<td>600</td>
</tr>
<tr><td>480P_8</td>
<td>848x480</td>
<td>15</td>
<td>610</td>
</tr>
<tr><td>480P_9</td>
<td>848x480</td>
<td>30</td>
<td>930</td>
</tr>
<tr><td>480P_10</td>
<td>640x480</td>
<td>10</td>
<td>400</td>
</tr>
<tr><td>720P</td>
<td>1280x720</td>
<td>15</td>
<td>1130</td>
</tr>
<tr><td>720P_1</td>
<td>1280x720</td>
<td>15</td>
<td>1130</td>
</tr>
<tr><td>720P_2</td>
<td>1280x720</td>
<td>15</td>
<td>2080</td>
</tr>
<tr><td>720P_3</td>
<td>1280x720</td>
<td>30</td>
<td>1710</td>
</tr>
<tr><td>720P_5</td>
<td>960x720</td>
<td>15</td>
<td>910</td>
</tr>
<tr><td>720P_6</td>
<td>960x720</td>
<td>30</td>
<td>1380</td>
</tr>
<tr><td>1080P</td>
<td>1920x1080* <a href="#f5" id="id15">[5]</a></td>
<td>15</td>
<td>2080</td>
</tr>
<tr><td>1080P_1</td>
<td>1920x1080* <a href="#f5" id="id16">[5]</a></td>
<td>15</td>
<td>2080</td>
</tr>
<tr><td>1080P_2</td>
<td>1920x1080* <a href="#f5" id="id17">[5]</a></td>
<td>30</td>
<td>3000</td>
</tr>
<tr><td>1080P_3</td>
<td>1920x1080* <a href="#f5" id="id18">[5]</a></td>
<td>30</td>
<td>3150</td>
</tr>
<tr><td>1080P_5</td>
<td>1920x1080* <a href="#f5" id="id19">[5]</a></td>
<td>60</td>
<td>4780</td>
</tr>
<tr><td>1440P</td>
<td>2560x1440* <a href="#f5" id="id20">[5]</a></td>
<td>30</td>
<td>4850</td>
</tr>
<tr><td>1440P_1</td>
<td>2560x1440* <a href="#f5" id="id21">[5]</a></td>
<td>30</td>
<td>4850</td>
</tr>
<tr><td>1440P_2</td>
<td>2560x1440* <a href="#f5" id="id22">[5]</a></td>
<td>60</td>
<td>7350</td>
</tr>
<tr><td>4K</td>
<td>3840x2160* <a href="#f5" id="id23">[5]</a></td>
<td>30</td>
<td>8910</td>
</tr>
<tr><td>4K_1</td>
<td>3840x2160* <a href="#f5" id="id24">[5]</a></td>
<td>30</td>
<td>8910</td>
</tr>
<tr><td>4K_3</td>
<td>3840x2160* <a href="#f5" id="id25">[5]</a></td>
<td>60</td>
<td>13500</td>
</tr>
</tbody>
</table>



**Note:** 视频能否达到 1080P 以上的分辨率取决于设备的性能，在性能配备较低的设备上有可能无法实现。如果采用720P分辨率而设备性能跟不上，则有可能出现帧率过低的情况。声网将继续在后续版本中为较低端设备进行视频优化。如设备有特别需求，请联系 [support@agora.io](mailto:support@agora.io)。

**Note:** 

由于各浏览器的限制，部分浏览器对设置的 Video Profile 不一定能全部适配。

*Opera 浏览器 52 和 53 因自身限制，仅支持如下 Video Profile*

<table>
<colgroup>
<col/>
<col/>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td>视频属性</td>
<td>分辨率（宽x高）</td>
<td>帧率</td>
<td>码率</td>
</tr>
<tr><td>480P</td>
<td>640x480</td>
<td>15</td>
<td>500</td>
</tr>
<tr><td>480P_1</td>
<td>640x480</td>
<td>15</td>
<td>500</td>
</tr>
<tr><td>480P_2</td>
<td>648x480</td>
<td>30</td>
<td>1000</td>
</tr>
<tr><td>480P_3</td>
<td>480x480</td>
<td>15</td>
<td>400</td>
</tr>
<tr><td>480P_4</td>
<td>640x480</td>
<td>30</td>
<td>750</td>
</tr>
<tr><td>480P_6</td>
<td>480x480</td>
<td>30</td>
<td>600</td>
</tr>
<tr><td>480P_8</td>
<td>848x480</td>
<td>15</td>
<td>610</td>
</tr>
<tr><td>480P_9</td>
<td>848x480</td>
<td>30</td>
<td>930</td>
</tr>
<tr><td>480P_10</td>
<td>640x480</td>
<td>10</td>
<td>400</td>
</tr>
<tr><td>720P</td>
<td>1280x720</td>
<td>15</td>
<td>1130</td>
</tr>
<tr><td>720P_1</td>
<td>1280x720</td>
<td>15</td>
<td>1130</td>
</tr>
<tr><td>720P_2</td>
<td>1280x720</td>
<td>15</td>
<td>2080</td>
</tr>
<tr><td>720P_3</td>
<td>1280x720</td>
<td>30</td>
<td>1710</td>
</tr>
<tr><td>720P_5</td>
<td>960x720</td>
<td>15</td>
<td>910</td>
</tr>
<tr><td>720P_6</td>
<td>960x720</td>
<td>30</td>
<td>1380</td>
</tr>
</tbody>
</table>



*网页端 Safari 浏览器支持分辨率*

<table>
<colgroup>
<col/>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td>视频属性</td>
<td>分辨率（宽x高）</td>
<td>码率</td>
</tr>
<tr><td>4K</td>
<td>3840 x 2160</td>
<td>8910</td>
</tr>
<tr><td>4K_1</td>
<td>3840 x 2160</td>
<td>8910</td>
</tr>
<tr><td>4K_3</td>
<td>3840 x 2160</td>
<td>13500</td>
</tr>
<tr><td>1440P</td>
<td>2560 x 1440</td>
<td>4850</td>
</tr>
<tr><td>1440P_1</td>
<td>2560 x 1440</td>
<td>4850</td>
</tr>
<tr><td>1440P_2</td>
<td>2560 x 1440</td>
<td>7350</td>
</tr>
<tr><td>1080P</td>
<td>1920 x 1080</td>
<td>2080</td>
</tr>
<tr><td>1080P_1</td>
<td>1920 x 1080</td>
<td>2080</td>
</tr>
<tr><td>1080P_2</td>
<td>1920 x 1080</td>
<td>3000</td>
</tr>
<tr><td>1080P_3</td>
<td>1920 x 1080</td>
<td>3150</td>
</tr>
<tr><td>1080_5</td>
<td>1920 x 1080</td>
<td>4780</td>
</tr>
<tr><td>720P</td>
<td>1280 x 720</td>
<td>1130</td>
</tr>
<tr><td>720P_1</td>
<td>1280 x 720</td>
<td>1130</td>
</tr>
<tr><td>720P_2</td>
<td>1280 x 720</td>
<td>2080</td>
</tr>
<tr><td>720P_3</td>
<td>1280 x 720</td>
<td>1710</td>
</tr>
<tr><td>480P</td>
<td>640 x 480</td>
<td>500</td>
</tr>
<tr><td>480P_1</td>
<td>640 x 480</td>
<td>500</td>
</tr>
<tr><td>480P_4</td>
<td>640 x 480</td>
<td>750</td>
</tr>
</tbody>
</table>



**Note:** 

Safari 浏览器视频帧率为 30 fps，不支持自定义视频帧率。

示例代码

```
setVideoProfile('480p_1');


```

### 设置屏幕属性 \(setScreenProfile\)

```
setScreenProfile(<profile>)


```

该方法设置屏幕的显示属性。

<table>
<colgroup>
<col/>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td><strong>名称</strong></td>
<td><strong>参数</strong></td>
<td><strong>描述</strong></td>
</tr>
<tr><td>profile</td>
<td>String</td>
<td>屏幕属性（Profile）字符串。详见下表的定义</td>
</tr>
</tbody>
</table>



**屏幕属性定义**

<table>
<colgroup>
<col/>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td><strong>屏幕属性</strong></td>
<td><strong>分辨率</strong></td>
<td><strong>帧率</strong></td>
</tr>
<tr><td>480p_1</td>
<td>640x480</td>
<td>5</td>
</tr>
<tr><td>480p_2</td>
<td>640x480</td>
<td>30</td>
</tr>
<tr><td>720p_1</td>
<td>1280x720</td>
<td>5</td>
</tr>
<tr><td>720p_2</td>
<td>1280x720</td>
<td>30</td>
</tr>
<tr><td>1080p_1</td>
<td>1920x1080</td>
<td>5</td>
</tr>
<tr><td>1080p_2</td>
<td>1920x1080</td>
<td>30</td>
</tr>
</tbody>
</table>


### 获取当前音量 \(getAudioLevel\)

```
getAudioLevel();

```

该方法获取的是当前时刻的音量：

```
setInterval(function() {
  var audioLevel = stream.getAudioLevel();
  // use audioLevel to render UI
}, 100)

```

如果你想表示本地或远端音量的变化，我们建议你使用 setTimeout 或者 setInterval 方法实时获取。

该方法对没有音频数据的流是无效的，并且会产生警告。

**Note:** 

在 Chrome 70 及以上版本，受浏览器策略影响，首次调用此方法须由用户点击触发，详见 [https://goo.gl/7K7WLu](https://goo.gl/7K7WLu)

### 调节音量大小 (setAudioVolume)

```
setAudioVolume(volume);

```

该方法可以调节音量大小，volume 为 number 类型，取值范围是 0 ～ 100。

### 设置音频输出 (setAudioOutput)

```
setAudioOutput(deviceId)

```

该方法可以在语音场景下设置音频输出设备，在通话时切换麦克风和扬声器。

deviceId 可以通过 枚举系统设备 (getDevices) 获得。

Note

目前只有 Chrome 49 以上的浏览器支持该方法。

### 获取视频 flag \(hasVideo\)

```
hasVideo()

```

该方法可以获取视频 flag。

### 获取音频 flag \(hasAudio\)

```
hasAudio()

```

该方法可以获取音频 flag。

### 启用视频轨道 \(enableVideo\)

```
enableVideo()

```

该方法启用视频轨道。在创建视频流时将 video flag 设置为 *true* 即可使用该方法，其功能相当于重启视频。

### 禁用视频轨道 \(disableVideo\)

```
disableVideo()

```

该方法禁用视频轨道。在创建视频流时将 video flag 设置为 *true* 即可使用该方法，其功能相当于暂停视频。

### 启用音频轨道 \(enableAudio\)

```
enableAudio()

```

该方法启用音频轨道。其功能相当于重启音频。

### 禁用音频轨道 \(disableAudio\)

```
disableAudio()

```

该方法禁用音频轨道。其功能相当于暂停音频。

### 获取音视频流 ID \(getId\)

```
getId()


```

该方法可以获取音视频流 ID。

### 获取连接状态 \(getStats\)

```
getStats(function(stats){
  console.log(stats);
});


```

该方法获取该 stream 的连接状态。

如果是发布的流，则调用回调里的 stats 包含以下信息：

<table>
<colgroup>
<col/>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td><strong>参数</strong></td>
<td><strong>类型</strong></td>
<td><strong>描述</strong></td>
</tr>
<tr><td>audioSendBytes</td>
<td>String</td>
<td>发送的音频字节</td>
</tr>
<tr><td>audioSendPackets</td>
<td>String</td>
<td>发送的音频包</td>
</tr>
<tr><td>audioSendPacketsLost</td>
<td>String</td>
<td>发送的音频丢包数</td>
</tr>
<tr><td>videoSendBytes</td>
<td>String</td>
<td>发送的视频字节</td>
</tr>
<tr><td>videoSendPackets</td>
<td>String</td>
<td>发送的视频包</td>
</tr>
<tr><td>videoSendPacketsLost</td>
<td>String</td>
<td>发送的视频丢包数</td>
</tr>
<tr><td>videoSendFrameRate</td>
<td>String</td>
<td>发送的视频帧率</td>
</tr>
<tr><td>videoSendResolutionWidth</td>
<td>String</td>
<td>发送的视频分辨率宽度</td>
</tr>
<tr><td>videoSendResolutionHeight</td>
<td>String</td>
<td>发送的视频分辨率高度</td>
</tr>
<tr><td>accessDelay</td>
<td>String</td>
<td>接入延迟 (ms)</td>
</tr>
</tbody>
</table>



如果是订阅的流，则调用回调里的 stats 包含以下信息：

<table>
<colgroup>
<col/>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td><strong>参数</strong></td>
<td><strong>类型</strong></td>
<td><strong>描述</strong></td>
</tr>
<tr><td>audioReceiveBytes</td>
<td>String</td>
<td>接收的音频字节</td>
</tr>
<tr><td>audioReceivePackets</td>
<td>String</td>
<td>接收的音频包</td>
</tr>
<tr><td>audioReceivePacketsLost</td>
<td>String</td>
<td>接收的音频丢包数</td>
</tr>
<tr><td>videoReceiveBytes</td>
<td>String</td>
<td>接收的视频字节</td>
</tr>
<tr><td>videoReceivePackets</td>
<td>String</td>
<td>接收的视频包</td>
</tr>
<tr><td>videoReceivePacketsLost</td>
<td>String</td>
<td>接收的视频丢包数</td>
</tr>
<tr><td>videoReceiveFrameRate</td>
<td>String</td>
<td>接收的视频帧率</td>
</tr>
<tr><td>videoReceiveDecodeFrameRate</td>
<td>String</td>
<td>接收的视频解码帧率</td>
</tr>
<tr><td>videoReceivedResolutionWidth</td>
<td>String</td>
<td>接收的视频分辨率宽度</td>
</tr>
<tr><td>videoReceivedResolutionHeight</td>
<td>String</td>
<td>接收的视频分辨率高度</td>
</tr>
<tr><td>accessDelay</td>
<td>String</td>
<td>接入延迟 (ms)</td>
</tr>
<tr><td>endToEndDelay</td>
<td>Array</td>
<td>端到端延迟 (ms) <a href="#f2" id="id11">[2]</a></td>
</tr>
<tr><td>videoReceiveDelay</td>
<td>Array</td>
<td>接收视频延迟 (ms) <a href="#f3" id="id12">[3]</a></td>
</tr>
<tr><td>audioReceiveDelay</td>
<td>Array</td>
<td>接收音频延迟 (ms) <a href="#f4" id="id13">[4]</a></td>
</tr>
</tbody>
</table>



**Note:** 从发送到接收的延迟时间

**Note:** 从发送到接收端播放视频的延迟时间，目前仅 Chrome 浏览器支持

**Note:** 从发送到接收端播放音频的延迟时间，目前仅 Chrome 浏览器支持

**Note:** 

目前 Firefox 和 Safari 浏览器在获取部分回调参数时有限制，详情如下：

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td><strong>浏览器</strong></td>
<td><strong>接收不到的回调参数</strong></td>
</tr>
<tr><td>Firefox</td>
<td><ul>
<li>audioSendPacketsLost</li>
</ul>
</td>
</tr>
<tr><td>Safari</td>
<td><ul>
<li>audioSendPacketsLost</li>
</ul>
</td>
</tr>
</tbody>
</table>

## 错误代码和警告代码

详见 [错误代码和警告代码](../../cn/API%20Reference/the_error_web.md)。
