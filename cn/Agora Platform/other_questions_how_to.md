
---
title: 其它常见问题
description: 
platform: 其它常见问题
updatedAt: Tue Nov 20 2018 10:55:48 GMT+0000 (UTC)
---
# 其它常见问题
## Android 平台常见问题

### 安卓平台上，可以将 SDK 代码与我自己的代码混用吗？

不能，否则无法回调 `.so` 文件。

### 集成 SDK 后，点击 App 中视频小窗口没有反应，也无法拖动，我该怎么办?

检查系统上有没有开启悬浮窗权限。如果没有开启该权限，App 是无法启动小窗口的。

### 可以在 Android 设备上做 64 位兼容吗？

取决于你使用的 Agora Native SDK 版本：

* 如果你使用的是 1.7.4 版 (或更高版本) Agora Native SDK：
  Agora Native SDK 目前支持 64 位的 ARM 架构，只需将 arm64-v8a 里面的文件(位于SDK包内)拷贝至项目对应的 arm64-v8a 路径下。目前不支持 x86_64 架构直接运行，不过可以以 x86 方式兼容运行。具体做法就是在 64 位的 x86 设备上保证 APK 里面没有   x86_64 目录。

* 如果你使用的是 1.7.4 版之前的 Agora Native SDK:
   Agora Native SDK 目前只提供 32 位 native 库（armeabi-v7a），在 64 位设备上，Android 支持以 32 位进程模式启动 APK，因此 Agora Native SDK 也可以在 64 位 Android 设备上使用，但必须保证 APK 的 arm64 目录为空。否则 Android 系统将以 64 位进程模式加载 APK，由于 Agora Native SDK 没有提供 64 位库，APK 启动会失败。

由于目前 Android 的主流还是 32 位设备，一般各厂商都会提供 32 位库，因此一般来说，在 64 位 Android 设备以 32 位进程模式启动一般不会有问题。但是如果在某些 64 位平台上出现了这样的错误: java.lang.UnsatisfiedLinkError: dlopen failed: ”libHDACEngine.so”

但是如果 64 位平台上出现以下报错: java.lang.UnsatisfiedLinkError: dlopen failed: "libHDACEngine.so"

可能的原因：

在安装 APK 的时候，系统会按照 `Build.SUPPORTED_ABIS` 去查找 APK 的 lib 目录下的 native 库的目录（现有的 ABI: armeabi, armeabi-v7a,arm64-v8a, x86, x86_64, mips64, mips）。如果在 app 中有兼容 64-bit 的目录但是又缺少库文件的话，并不会使用其他 ABI 目录下的库文件替换所缺少的库文件进行安装，这些库不混合使用，也就是说需要为每个架构提供对应的库文件。Android 在加载 native 库的时候有回退（fallback）机制，在64位系统上如果 app 并不存在 arm64-v8a 的目录，则会尝试寻找armeabi-v7a下面的库进行加载，一般来说是向下兼容的。

解决方案:
* 方法 1: 在构建应用程序的时候，在工程 (project) 里删除所有 arm64-v8a 下面的库以及该目录；在生成 app 后，确认 apk 的包内 lib 下没有 arm64-v8a 的目录。
* 方法 2: 在 gradle 构建文件中设置 abiFilters，只打包 32 位架构的库:
 
 ```
 android {
          ...
          defaultConfig {
          ...
          ndk {
          abiFilters "armeabi-v7a","x86"
          }
          }
          }
```

### 为什么我会收到到错误消息: Failed to crunch file?

当您在使用 Android Open Video Call demo 时，如果遇到以下错误消息，例如:
Error: Failed to crunch file E:\Rock\videoIM\Agora_Native_SDK_for_Android_v1_7_4_FULL\Agora_Native_SDK_for_Android_FULL\samples\OpenVideoCall_Android\app\build\intermediates\exploded-aar\com.android.support\appcompat-v7\25.0.0\res\drawable-xhdpi-v4\abc_ab_share_pack_mtrl_alpha.9.png
into
E:\Rock\videoIM\Agora_Native_SDK_for_Android_v1_7_4_FULL\Agora_Native_SDK_for_Android_FULL\samples\OpenVideoCall_Android\app\build\intermediates\res\merged\debug\drawable-xhdpi-v4\abc_ab_share_pack_mtrl_alpha.9.png

该问题是由于 demo 引用的文件名过长造成的。

## 登录类常见问题

### 为什么提示我被踢出了设备？

如果一个已经登录的用户，在另一个设备登录，则当前用户会被踢。

### SDK 有断线重连机制吗？机制是什么？

断线分两种情况：断网、进程被杀。

#### 断网

假设有 A， B 两个用户处于同一个频道内进行音视频直播 / 通话，通话过程中 A 用户失去了网络连接。

1. A 失去网络连接（A 用户在 4 秒内没有收到服务器数据）：
   
	 * 如果 A 是 Android， Windows，或 Linux 平台，A 有回调： onConnectionInterrupted
   * 如果 A 是 iOS 或 mac 平台： rtcEngineConnectionDidInterrupted
   * 如果 A 是 Web 平台，A 没有回调。

2. 失去连接后， A 尝试重连其他服务器,直到重连成功：
   
	 * 如果 A 在 10 秒内未连接成功，会有回调：（10 秒这个参数可以更改，具体请咨询 support@agora.io）
      * 如果 A 是 Android， Windows，或 Linux 平台： onConnectionLost
      * 如果 A 是 iOS 或 mac 平台： rtcEngineConnectionDidLost
      * 如果 A 是 Web 平台，A 没有回调。

   * 如果 B 在一定时间内没有收到 A 的包，B 有回调：
      * 如果 B 是 Android， Windows，或 Linux 平台，20 秒内没有收到 A 的包： onUserOffline
      * 如果 B 是 iOS 或 mac 平台，20 秒内没有收到 A 的包： didOfflineOfUid
      * 如果 B 是 Web 平台，10 秒内没有收到 A 的包：client.on('stream-removed')

   * 如果 A 重连成功：
      * 如果 A 是 Android， Windows，或 Linux 平台，A 有回调： didRejoinChannel
      * 如果 A 是 iOS 或 mac 平台，A 有回调： didRejoinChannel
      * 如果 A 是 Web 平台，A 没有回调。

   * 如果此前 B 未收到 A 重连失败的回调，B 不会因为 A 重连成功而收到回调；如果此前 B 已经收到 A 重连失败的回调，则此时 B 有回调：
      * 如果 B 是 Android， Windows，或 Linux 平台：onUserJoined
      * 如果 B 是 iOS 或 mac 平台： didJoinedOfUid
      * 如果 B 是 Web 平台： client.on('stream-added')

#### 进程被杀

进程被杀包括以下各种情况：

* 通信模式/直播模式
* 打开/关闭 voip 模式
* 前台/后台运行时进程被杀
* 网页关闭（Web 端）

假设有 A， B 两个用户处于同一个频道内进行音视频直播/通话。

当 A 进程被杀：
   * 如果 A 是 iOS 或 mac 平台：A 自动触发 leaveChannel，B 有回调：
      * 如果 B 是 Android， Windows，或 Linux 平台：onUserOffline
      * 如果 B 是 iOS 或 mac 平台： didOfflineOfUid
      * 如果 B 是 Web 平台： client.on('peer-leave')
	 * 如果 A 是 Android， Windows，或 Linux 平台，且 B 使用的是 Native SDK：
      * 如果 20 秒内， A 没有重启 app 并加入原频道，B 有回调：（20 秒这个参数可以更改，具体请咨询 support@agora.io）
         * 如果 B 是 Android， Windows，或 Linux 平台： onUserOffline
         * 如果 B 是 iOS 或 mac 平台：didOfflineOfUid
      * 如果 20 秒内， A 重启 app 并加入原频道，B 不会收到回调。
  * 如果 A 是 Android， Windows，或 Linux 平台，且 B 使用的是 Web SDK：
      * 如果 10 秒内， A 没有重启 app 并加入原频道，B 有回调：client.on('stream-removed')
      * 如果 10 秒内， A 重启 app 并加入原频道，B 不会收到回调。
	 * 如果 A 使用的是 Web SDK，进程被杀与掉线的行为是一样的。
   * 如果 A 是频道内最后一个用户，服务端会在 10s 后销毁频道。

## 频道类常见问题

### 网络环境差时，SDK 会强行让用户自动退出频道么？

SDK 不会让用户自动退出频道，除非用户自己主动退出，例如，应用程序调用 `leaveChannel`。

### 在每个房间，每个频道内，通话中是否有管理员？

没有管理员的概念，管理员是属于信令层的范围。可以由信令层实现，由信令服务器主动下发命令，调用SDK的接口来实现通话管理。

### 客户端是否需要维护频道？

频道是自动创建和删除的，客户端无需处理和维护。当所有客户端都离开一个频道时，频道自动被删除。

### 什么是 App ID 和 Token? 我如何使用？

更多详细介绍， 以及关于如何获取和使用 App ID 和 Token 的步骤，请参考 [校验用户权限](../../cn/voice/token.md)。

### 如果同时使用 App ID 和 Token, 以哪个为准？

Token。

### 用户直播时，如何保持房间名/频道名的唯一性？

由 App 端自行区分，例如加上前后缀。如果指定相同的频道名，则进入同一频道。

### 如何监听频道内谁在说话？

以下各平台回调方法提示了频道内谁在说话，以及说话者的音量：

* Android/Windows: onAudioVolumeIndication
* iOS/macOS: reportAudioVolumeIndicationOfSpeakers

该提醒默认为关闭状态。如需启用，请调用 `enableAudioVolumeIndication` 方法进行配置。

### Windows 上关于几个初始化参数的初始化时间点有哪些注意事项？

* 在加入频道之前，必须保证调用过一次: virtual int IRtcEngine::initialize(const RtcEngineContext& context)
* 如果需要开启视频功能，则必须在加入频道前调用: virtual int IRtcEngine::enableVideo()
* 如果要使用同频道消息透传，必须在加入频道前调用: virtual int IRtcEngine::enableVendorMessage()

## 示例代码与 API 相关问题

### 为什么 Open Live 编译无法通过？

Open Live 的编译需要用户提供自己申请的 App ID 才能通过。请在提示出错的地方，补上自己申请的 App ID。

### 从哪里可以获取 App ID？

在 dashboard.agora.io 注册后，即可在 Dashboard 默认或新建的项目下获取 App ID。 详见 [校验用户权限](../../cn/voice/token.md)。 

### 为什么在 Open Live 代码里填好 App ID 后，进不去频道，也看不到本地视频?

请检查一下：在 dashboard 中，是否启用了 App Certificate, 一旦启用 App Certificate, 则必须使用 Token 加入频道。

关于 App ID 与 Token 的区别，详见 [校验用户权限](../../cn/voice/token.md)。

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

### 在使用和集成 SDK 过程中会返回哪些错误代码，哪里可以找到对应的描述?

详见：
Native SDK 错误码及警告码
Web SDK 错误码及警告码
Signaling SDK 错误码及警告码
Gaming SDK 错误码及警告码

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

```
LONG styleValue = ::GetWindowLong(parentWindow.GetSafeWnd(), GWL_STYLE);
::SetWindowLong(parentWindow.GetSafeHwnd(), styleValue | WS_CLIPSIBLINGS | WS_CLIPCHILDREN);
```

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

## 其他常见问题

### 无法加入频道

查看是否有错误码，以及查看是哪种错误码引起的加入频道失败。
如果错误码提示是因为没有退出上次通话引起的，先调用 `leaveChannel` ，然后再次加入频道。

### 无法通过 Dynamic Key/Token 验证

检查：
* Dynamic Key/Token 生成的算法是否正确。
* App ID 和 App Certificate 是否填写正确，注意区分大小写。
* `expireTime` 是否晚于当前时间。

### 当用户加入频道后切换到后台模式一段时间后(例如 30 分钟)，发现 iOS App 崩溃了

该问题出现的原因是当集成 Agora SDK 时，没有在 Background Modes 里选择 Audio, AirPlay, and Picture in Picture 。

![](https://web-cdn.agora.io/docs-files/1542709974181)

你仅需要在 Xcode 勾选该选项即可。

### 呼叫成功，但是没有声音

* 初始化信令时，有初始化媒体（使用 `getInstanceWithMediaKey`）。
* 呼叫成功，仅代表 A 发出了呼叫邀请，且 B 接受了邀请。A 和 B 还需要加入频道才能听见声音。

### 呼叫失败

1. 请确保您已登录。只有在登录状态才能发起呼叫。
2. 检查对方是否为在线状态，如果对方为离线，则无法呼叫成功。

### 当 App 在 iOS 上进入后台模式后，通话中断。

在 Xcode 中开启后台模式即可。在您的App上：

* 选择 Project > Capabilities
* 打开 Background Modes， 选择 Audio, AirPlay and Picture in Picture 和 Voice over ip 选项。

### 日志打印中出现 Failed to decode frame xxx, return error code is -1

这个是解码失败，最常见的情形是网络不好时丢包导致收不到完整帧，引起解码出错，如果日志打印中出现这段信息，检查一下当前的网络环境。

### XP 系统初始化失败

Agora 的 SDK 从 1.1 版本开始，编译器默认采用 VC2013，依旧会支持 XP 系统，但由于操作系统未部署相关运行时库，需从微软下载，地址如下(该地址可能由于微软官方变更而失效)：https://www.microsoft.com/en-us/download/details.aspx?id=40784 。

请勿从其他第三方转载地址下载，很多下载站所提供版本并非最新版，即使安装也无法正常运行。

### 启动 app 时出现 Exception 信息

当集成 Agora SDK 以后，启动 app 出现下面的 Exception 信息:
```
dalvik.system.PathClassLoader[DexPathList[[zip file xxxxx.apk],nativeLibraryDirectories=[/data/app/xxxxx/lib/arm,/vendor/lib/,system/lib]]]
couldn’t find "libHDACEngine.so" java.lang.Runtime.loadLibrary(Runtime.java:366)…
```
这种情况是由于集成时没有将 .so 文件放置在正确的平台路径下, 导致 App 启动后找不到 `.so` 文件导致的。打包 APK 时请注意平台和路径。

