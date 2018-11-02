
---
title: 其它常见问题
description: 
platform: 其它常见问题
updatedAt: Thu Nov 01 2018 08:05:41 GMT+0000 (UTC)
---
# 其它常见问题
# 其它常见问题

### 为什么 Open Live 编译无法通过？

Open Live 的编译需要用户提供自己申请的 App ID 才能通过。请在提示出错的地方，补上自己申请的 App ID。

### 从哪里可以获取 App ID？

在 dashboard.agora.io 注册后，即可在 Dashboard 默认或新建的项目下获取 App ID。 详见[密钥说明](../../cn/Agora%20Platform/token.md)。 

### 为什么在 Open Live 代码里填好 App ID 后，进不去频道，也看不到本地视频?

请检查一下：在 dashboard 中，是否启用了 App Certificate, 一旦启用 App Certificate, 则必须使用 Token 加入频道。

关于 App ID 与 Token 的区别，详见 [密钥说明](../../cn/Agora%20Platform/token.md)。

### Token 的时效多长？过期时是否对正在直播或看播产生影响，比如中断？

Token 有两个跟时间相关的字段：

* 授权时间戳 ：Token 本身的有效期，有效期为 24 个小时，如果超过 24 小时用户没有进入频道，则该 Token 将无法再加入频道。
* 服务过期时间戳：允许用户在频道内的最大时长。在到时间时用户将被踢出频道。默认该事件为无穷大，但用户可以自定义。

### Agora SDK 支持 ReplayKit 吗？

ReplayKit 是 iOS 10 内置的录屏功能, 目前 Agora SDK 不支持 ReplayKit。但用户可以自行采集视频，交给 Agora SDK 实现。

### 当 PC 端有多个摄像头用于多角度拍摄时，是否可以通过切换不同的视频原始码流并送到 SDK 里来进行主播大窗口的切换？

对于单台 PC 外接多 USB 摄像头，可以通过自行采集不同摄像头原始视频流再输入 SDK 的方案进行切换；也可以直接在APP端通过热切换摄像头的方式进行主播画面切换，SDK 本身是支持 PC 端多个摄像头热切换的。

### 哪些回调函数建议做 App 业务逻辑？哪些不建议做?

App 经常关心的是用户状态相关的回调，如 `userJoined`，`userOffline` 等，鉴于所有与用户相关的逻辑都是以 UDP 的方式传输，并不可靠。 同样原理，其他与远程用户相关的回调也是不可靠的。

### Error Code 里哪些可以做 APP 的业务逻辑，哪些不可以？

Error 类的可以做 APP 的业务逻辑，但是 Warning 类的无需处理。

### 哪些回调可以用于提示用户断开服务器，或者重连？

rtcEngineConnectionDidLost 或 connectionLostBlock 可用户提示用户断开服务器, 断开事件后，SDK会主动重连服务器。

### 多次调用 initWithAppId 会有什么不良后果吗？

有，可能会闪退或者导致摄像头方向不正常等。

### 加解密接口（算法）对系统影响有多大？

复杂的加解密对 CPU 影响很大，会造成加解密线程 CPU 负载很大，出现延迟。需要根据实际情况来测试。

### 通话时，为什么旋转手机，图像有时跟着旋转，有时不？

手机支持转屏和不转屏两种。这个在系统里可以设置。如果设置了允许转屏，应用可以重新布局，窗口会变宽或者窄。如果系统不允许转屏，应用就无法重新布局。这就是为什么旋转手机，图像有时候会跟着旋转，有时候不会。

### 网络重新连接后，为什么对方视频图像仍然为静止状态，没有恢复？

网络连接后，视频通讯需要一点延时才能连接上。这是所有视频通讯都有的现象。网络比较差的情况下，延时会长一些。

### SDK 的用户 id 是 32 位无符号整型，而我们现有用户的 id 是 String 值， 我该怎么办？

这个是信令层的逻辑和协议，建议客户服务器在 string 和 int 间映射。

### 怎么让音量显示的回调生效？

由于远端音频流音量回调默认关闭，所以必须在加入频道前后立刻调用相关 API 开启该回调: RtcEngineParameters:: enableAudioVolumeIndication(int interval, int smooth)。

### XP 系统初始化失败，我该怎么办？

Agora 的 SDK 从 1.1 版本开始，编译器默认采用 VC2013，依旧会支持 XP 系统，但由于操作系统未部署相关运行时库，需从微软下载，地址如下(该地址可能由于微软官方变更而失效): https://www.microsoft.com/en-us/download/details.aspx?id=40784

请勿从其他第三方转载地址下载，很多下载站所提供版本并非最新版，即使安装也无法正常运行。

### 当调用 setVideoProfile 设置视频属性时，为什么有些属性的帧率相同，码率不相同?

因为不同用户对画质有不同的要求, 例如：

### 当调用 setVideoProfile 设置视频属性时，为什么有些属性的帧率相同，码率不相同?

因为不同用户对画质有不同的要求, 例如：

<table>
  <tr>
    <th>视频属性</th>
    <th>枚举值</th>
    <th>分辨率（宽x高）</th>
    <th>帧率（fps）</th>
    <th>码率（kbps）</th>
  </tr>
  <tr>
    <td>360P</td>
    <td>30</td>
    <td>640x360</td>
    <td>15</td>
    <td>400</td>
  </tr>
  <tr>
    <td>360P_9</td>
    <td>38</td>
    <td>640x360</td>
    <td>15</td>
    <td>800</td>
  </tr>
</table>

上表里将码率设为 800 kbps 的用户明显对画质要求比较高。

### 什么是回调?

编程分别两类:

* 系统编程 (System Programing)：编写库。
* 应用编程 (Application Programing)：利用已写好的库来编写具有某种功用的程序，也就是应用。

系统开发程序员留下一些接口给应用开发程序员使用，这些接口即为 API (Application Programming API)。
当程序运行时, 应用程序（Application program）会时常通过API调用库里所预先备好的函数。但有些库函数要求应用先传给它一个函数，好在合适的时候调用，以完成目标任务。这个被传入的、后又被调用的函数成为回调函数 (Callback Function)。

例如: 你走进一家商店，想买某一款香水，但是店员告诉你现在缺货。店员说他们会在有货的时候联系你，但你必须自己提供联系方式。例如: 你提供了电话号码或邮箱地址。联系你的行为是商店提供的，这就类似于库函数;你决定是否提供联系方式，以及提供什么方式，这就类似于回调函数。你让商店给你打电话，或发邮件，或添加微信好友联系你的动作，就类似于登记回调函数。

以下为基本逻辑：

![](https://web-cdn.agora.io/docs-files/1539245285003)

### 当我在 Windows 上进行调试时，提示我需要一个 agorartc.pdb 文件，这个文件在哪里可以拿到？

请忽视该提示，不影响使用。

### 为什么在 Windows 平台上显示视频的窗口不是平铺的，且这些视频窗口不是同一个父窗口?

如果显示视频的窗口不是平铺开的，并且这些视频窗口不是同一个父窗口，那么这些视频窗口绘制时可能会出现互相被刷掉的情况。因此，如果出现这种情况，你需要调用 Win32 API ::SetWindowLong 给父窗口设置如下的 style：

LONG styleValue = ::GetWindowLong(parentWindow.GetSafeWnd(), GWL_STYLE);
::SetWindowLong(parentWindow.GetSafeHwnd(), styleValue | WS_CLIPSIBLINGS | WS_CLIPCHILDREN);

### Agora Cloud 与一般的 CDN RTMP 有何不同?

CDN+RTMP 的直播技术使得用户在网页端安装 flash player 就能观看直播，极大降低了观众的门槛。
有别于市面上最常见的 CDN+RTMP 直播技术，Agora.io 提供的直播方案为在 Agora Cloud、主播、以及高级观众(嘉宾) 端之间达到专线级别的实时通讯质量而使用了：

* 私有音视频编码
* 私有传输协议
* 私有节点部署
* 私有传输算法

详见：

<table>
  <tr>
    <th>条目</th>
    <th>常见的 CDN+RTMP 技术</th>
    <th>Agora Cloud</th>
  </tr>
  <tr>
    <td>视频编解码</td>
    <td>H.264</td>
    <td>私有</td>
  </tr>
  <tr>
    <td>语音编解码</td>
    <td>AAC</td>
    <td>私有</td>
  </tr>
  <tr>
    <td>传输协议</td>
    <td>基于 RTM P的 TCP</td>
    <td>基于 RTM P的 TCP基于 UDP 的私有协议</td>
  </tr>
  <tr>
    <td>传输算法</td>
    <td>TCP</td>
    <td>Agora 私有丢包对抗、带宽自适应</td>
  </tr>
  <tr>
    <td>合图布局</td>
    <td>固定</td>
    <td>可动态调整</td>
  </tr>
</table>

为了照顾很多有需求的厂商，声网 Agora 与多家 CDN 进行了对接，且提供了旁路直播功能（可以进行社交分享)。

### 为什么 SEI 中设置的布局，跟实际得到的布局，有 1 个像素上的偏移？

视频底层解码为 YUV420 格式时，YUV 的采样在宽和高两个方向上的比例为 2:1:1，所以宽跟高必须是偶数，移动的距离上也必须是偶数。
但是由于我们自定义布局采用相对值，所以在计算变换时，会将各个主播的宽高和位置，计算成偶数，导致与用户期望的数据，有 1 个像素的偏差。

### 为什么 HLS 延时比 RTMP 要高很多？

HLS 分为 m3u8 索引文件和 ts 媒体切片文件，Apple 的文档中，ts 最短是 2s 一个切片，m3u8 至少 3 个切片文件的索引，所以最短需要 6s 的延时。

而 RTMP 一般都是直接透传，只需要短暂的缓冲即可，所以延时相对而言要比 HLS 低很多。

