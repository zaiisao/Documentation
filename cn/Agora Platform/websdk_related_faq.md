
---
title: Web SDK 相关
description: 
platform: Web
updatedAt: Thu Aug 29 2019 10:06:42 GMT+0800 (CST)
---
# Web SDK 相关
### H.264 模式下，用户在 macOS 上用 Firefox 浏览器加入通话，打开小流时，获取的分辨率与大流一致？
H.264模式下，在 macOS 设备上使用 Firefox 打开小流时，获取的分辨率与大流相同；如果是 VP8 模式，则没有此问题。Agora 会持续关注并更新在该问题上的进展。

### 用户在 macOS 上用 Firefox 浏览器 v59.01 版本加入通话，只能看到本地视频，看不到对方视频？
这个问题目前在 Web SDK 2.1 Hotfix 版本上已解决。

### 两台电脑和一台手机互通时，手机出现屏幕卡住的情况，按 home 键也无法退出?
当 Safari 浏览器、 Chrome 浏览器和移动端加入通话时，有可能会在移动端出现页面冻结或卡住的情况。这是由 Chrome 浏览器发出的 H.264 视频编码导致的。这个问题在使用 Safari 台式电脑与 Safari iOS 互通时不会出现。

详见：
http://bugs.webkit.org/show_bug.cgi?id=176439
http://bugs.webkit.org/show_bug.cgi?id=178357

### 用户在网页端视频过程中，接了个 QQ 电话，再继续网页视频时，就发送不出音频?
在 Web 端使用浏览器通话过程中，如果有第三方 App 占用音频设备（如系统接听 QQ 电话等），再切换回 Web 端通话时，将无法发送音频信号。建议您退出后重新连接。

### Mac 和 Windows 上均使用 Chrome 浏览器通话，如果 Windows 端切换 Wi-Fi，Mac 端画面卡住？
Mac 端需刷新界面后才能恢复画面。Agora 会持续关注并更新在该问题上的进展。

### 无法加入 channel，报 websocket error，同时会产生类似 ddos 的攻击，短时间巨量的 ws 的连接？
Web SDK join channel 参数数据类型因版本变化有差异。

* Web SDK 1.12 及之后的版本中，Join() 增加了Channelkey的属性，详见：https://docs.agora.io/cn/Video/API%20Reference/web/index.html。
* Web SDK 1.12 之前的版本中 Join() 中无此属性，详见：https://docs.agora.io/cn/1.8/user_guide/API/webrtc_api.html。

### h264_interop 模式下，Firefox 浏览器作为发送端时，接收端切换小流失败？
小流切换与浏览器类型、分辨率、编解码类型相关。各浏览器会根据自己浏览器的内部算法进行调整，因此在进行小流切换时，某些分辨率下会出现小流切换与预计不一致的现象。

### 直播场景中，在 AgoraRTC.createClient({mode: ‘interop’}) 模式下，Web 单主播如果不设置转码推流，就没有视频？
在 `AgoraRTC.createClient({mode: ‘interop’})` 模式下，如果使用单 Web 主播进行推流，需要将 Web 单主播的码流进行转码后再进行推流，否则会出现没有视频的现象。

若要对 Web 单主播直接进行推流，请使用 `AgoraRTC.createClient({mode: ‘h264_interop’})` 模式。

请注意，Agora 转码需要收取转码费用。

### 直播过程中，Safari 浏览器播放第三方音乐后直播音频异常？
经测试分析，使用第三方应用播放音频之后，再切回音视频通话，Safari 的音视频通话会受影响。我们会持续关注该问题，详见：https://bugs.webkit.org/show_bug.cgi?id=179964。

### 在 Web 端使用 Firefox 浏览器设置视频编码属性进入频道不生效？
在 Web 端使用 Firefox 浏览器时，我们发现部分机器设置的视频编码属性会不生效。这可能是由电脑和浏览器的兼容问题导致的。

目前统计的出现此问题的设备包括：
* Macbook Pro(13-inch, 2016, Two Thunderbolt 3 ports) 
* Windows 10 (MI)

我们会持续关注此问题的最新进展并更新异常设备列表。如果您在使用中遇到此类问题，请将问题反馈给 Agora 技术支持。

### iOS 端无法使用 getAudioLevel 方法获取音量？
经测试分析，iOS Safari 端无法使用 `getAudioLevel` 方法来获取音量，这是由浏览器本身的限制所造成的，我们会持续关注此问题的最新进展。

### 当 Agora Native SDK 和 Web 用户加入同一直播频道时，PC 或移动端用户可以看见本地和远端视频，但网页端用户看到的远端视频为黑屏，为什么？
PC 或移动端用户（使用 Agora Native SDK 的用户）在直播场景下必须调用 `enableWebSdkInteroperability` 打开互通功能。

### 设置视频分辨率为小流模式，但费用却没有按照小流模式计算？
由于设备和浏览器的限制，你设置的分辨率可能不会生效。 在这种情况下，Agora 将根据实际分辨率计算费用。

### 使用 Firefox 浏览器和 Native SDK 互通时，通信模式下，Firefox 浏览器看到的视频方向会旋转，如何解决？
由于浏览器自身的限制，通信模式下建议使用 Chrome 浏览器与 Native SDK 互通。

### 在 Firefox 浏览器 v59.0.2 上订阅远端流失败？
该问题是由于浏览器自身限制导致的，建议更新 Firefox 浏览器版本。

### 调用 `stream.init` 报错 `Media access:SecurityError`？
请使用 HTTPS 协议。

### 调用 `stream.init` 报错 `Media access:NotFoundError`？
找不到指定的媒体流。检查你的麦克风和摄像头是否正常工作。

### Safari 浏览器只接收流不发送流时，听不到声音？
如果 Safari 浏览器设置没有打开自动播放，需要在 `Stream.play` 之前调用方法 `navigator.mediaDevices.getUserMedia` 获取设备权限，否则会听不到声音。
