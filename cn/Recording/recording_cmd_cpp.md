
---
title: 命令行录制
description: How to start recording using cmd
platform: Linux CPP
updatedAt: Mon Jul 15 2019 06:19:00 GMT+0800 (CST)
---
# 命令行录制
本文介绍如何通过命令行进行录制。 

Agora 录制 SDK 提供一个 recorder demo，直接在命令行中输入命令运行 demo 就可以开始录制。

开始前请确保你已经完成录制 SDK 的环境准备和集成工作，详见[集成客户端](../../cn/Recording/recording_integrate_cpp.md)。

> 当录制 SDK 加入频道时，相当于一个哑客户端加入频道，因此需要跟 Agora Native/Web SDK 加入相同的频道，并使用相同的 App ID 和频道模式。

## 编译代码示例

打开命令行工具，到 SDK 包的 **samples/cpp** 的目录下执行以下命令进行编译。

```
make
```

编译成功后，在该目录下会生成一个 `recorder_local` 可执行程序，如图所示。

![img](https://web-cdn.agora.io/docs-files/1544522109941)

## 开始录制

在命令行中输入 `./recorder_local` 加上必要的参数设置，即可快速开始录制，例如：

```bash
./recorder_local --appId <你的 App ID> --channel <频道名> --uid 0 --channelProfile <0 通信模式，1 直播模式> --appliteDir Agora_Recording_SDK_for_Linux_FULL/bin
```

其中：

- `appId`，`channel` 和 `channelProfile` 的设置必须与 Agora Native/Web SDK 一致。
- `appliteDir` 必须设置为 `AgoraCoreServices` 存放的路径，SDK 包内该文件位于 **bin** 文件夹下。

## 设置录制选项

除上面示例中的参数外，录制 demo 还提供很多参数设置选项，在 **samples/cpp** 目录下执行 `./recorder_local` 命令，即可看到录制 demo 全部的参数和选项，你可以参考下表根据需要自行设置。

> `appID`，`uid`，`channel` 和 `appliteDir` 这几个参数必须设置，如不设置则无法录制。

| 参数                      | 描述                                                         |
| --------------------------- | ------------------------------------------------------------ |
| appId                       | App ID，必须和 Agora Native/Web SDK 的 App ID 一致，详见[获取 App ID](../../cn/Recording/token.md)。 |
| uid                         | 用户 ID，32 位无符号整数。建议设置范围：1 到（2<sup>32</sup>-1），并保证唯一性。<br/>有两种设置方式：<li>设置为 0，系统将自动分配一个用户 ID <li>设置一个唯一的用户 ID（不能与频道内的任何 uid 重复） |
| channel                     | 希望录制的通话或直播的频道名                                 |
| appliteDir                  | 必须设置为 `AgoraCoreServices` 存放的目录，SDK 包内该文件默认路径为：**Agora_Recording_SDK_for_Linux_FULL/bin/** |
| channelKey                  | 安全要求较高的用户可以使用 Token 或 Channel Key，详见[校验用户权限](../../cn/Recording/token.md)。 |
| channelProfile              | 用于设置频道模式。<li>0：通信模式 (默认)，即常见的 1 对 1 单聊或群聊，频道内任何用户可以自由说话 <li>1：直播模式，有两种用户角色，主播和观众，只有主播可以自由发言。<br/>**频道模式必须与 Agora Native/Web SDK 一致，否则可能导致问题。** |
| isAudioOnly                 | <li>0（默认）：音视频同时录制<li>1：仅启用音频录制功能，关闭视频录制<br />**`isAudioOnly` 和 `isVideoOnly` 不能同时设置为 1。** |
| isVideoOnly                 | <li>0（默认）：音视频同时录制<li>1：仅启用视频录制功能，关闭音频录制<br />**`isAudioOnly` 和 `isVideoOnly` 不能同时设置为 1。** |
| isMixingEnabled             | 是否启用合流模式，合流是指将频道内所有用户的音频流和视频流分别混合录制到一个文件内。<li>0（默认）：启用单流模式录制。录制文件的声道数和码率与原始音频流的声道数和码率保持一致。<li> 1：启用合流模式录制。录制文件的采样率，码率和声道数与录制前各音视频流的最高值保持一致。 |
| mixResolution | 如果 `isMixingEnabled` 设为 1，可以通过该参数设置合图视频的分辨率，格式为：width，high，fps，Kbps，从左至右分别对应合图的宽、高、帧率和码率。关于合图推荐设置的分辨率，详见 [mixResolution](https://docs.agora.io/cn/Recording/API%20Reference/recording_cpp/structagora_1_1recording_1_1_recording_config.html#a522a74ca1a09cecf04c5e127cd70eddf)。 |
| mixedVideoAudio             | 如果 `isMixingEnabled` 设为 1 ，该参数可以设置是否实时混合语音和视频:<li>0（默认）：不混合音频和视频。<li>1：音频和视频混合成一个文件，录制文件格式为 MP4，但播放器支持有限。<li>2：音频和视频混合成一个文件，录制文件格式为 MP4，支持更多播放器。<br>具体的播放器支持，详见 [MIXED_AV_CODEC_TYPE](https://docs.agora.io/cn/Recording/API%20Reference/recording_cpp/namespaceagora_1_1linuxsdk.html#af8f3a6529f57ccfa3621014808d1283a)。 |
| decryptionMode              | 频道加密时，录制 SDK 可以启用内置的解密功能。解密方式必须与频道设置的加密方式一致。目前支持以下几种解密方式：<li>“aes-128-xts”：128 位 AES 加密，XTS 模式<li>“aes-128-ecb”：128 位 AES 加密，ECB 模式<li>“aes-256-xts”：256 位 AES 加密，XTS 模式 |
| secret                      | 启用解密模式后，设置的解密密码。                             |
| idle                        | 设置空闲频道超时退出时间，单位为秒，最小值为 3 秒，默认值为 300 秒。如果频道内无用户的状态持续超过该时间，录制程序会自动退出。<br />**idle 的时间也会纳入计费。** |
| recordFileRootDir           | 设置录制文件存放的根目录。设置该参数后，会按照录制日期自动生成子目录保存录制文件。 |
| lowUdpPort                  | 最低 UDP 端口。所设置的端口必须为正整数，且最高 UDP 端口与最低 UDP 端口差值不能小于 4。 |
| highUdpPort                 | 最高 UDP 端口。所设置的端口必须为正整数，且最高 UDP 端口与最低 UDP 端口差值不能小于 4。 |
| getAudioFrame               | 录制音频格式<li> 0：默认音频文件格式<li>1：原始音频数据 AAC 帧格式 <li> 2：原始音频数据 PCM 帧格式<li> 3：原始音频数据 PCM 帧混音格式<br>**该参数设为 1，2 或 3 （即录制原始音频数据）时，不可将 `isMixingEnabled` 设为 1。** |
| getVideoFrame               | 录制视频格式<li> 0：默认视频文件格式 <li>1：原始视频数据 H.264 帧格式<li>2：原始视频数据 YUV 帧格式<li>3：原始视频数据 JPG 帧格式<li> 4：JPG 文件格式<li> 5：原始视频数据 JPG 帧 + MP4 视频文件格式<br>该参数设置有以下限制：<li>设为 1，2，3 或 4 时，不可将 `isMixingEnabled` 设为 1。<li>Web 端录制不支持 H.264 格式的原始视频数据，支持 YUV 的原始视频数据。 |
| captureInterval             | 截屏的时间间隔，最小值为 1 秒，默认值为 5 秒，只有在 `getVideoFrame` 设为 3，4 或 5时生效。 |
| cfgFilePath                 | 指定配置文件的路径。在该配置文件里，你可以设置保存录制文件的绝对路径，但不会自动生成子目录。配置文件的内容必须为 JSON 格式，例如：`{“Recording_Dir” : “recording path”}`，其中 `Recording_Dir` 是固定的，不能改动。 |
| streamType                  | 设置录制的视频流类型，只有在 Agora Native/Web SDK 开启了双流模式时该设置才会生效。<li>0（默认）：录制视频大流<li>1：录制视频小流 |
| triggerMode                 | 选择录制启动模式。<ul><li>0：自动模式，加入频道自动开始录制，离开频道自动停止录制。<li>1：手动模式，手动输入命令开始和结束录制。<ul><li>开始： `killall -s 10 recorder_local` <li>结束：`killall -s 12 recorder_local`</ul></li></ul> |
| proxyServer                 | 代理服务器的 IP 地址和端口号，如 “127.0.0.1:1080”。          |
| audioProfile                | 设置音频编码配置，包括采样率、码率和声道数。<li>0：音频默认设置，采样率 48k，单声道，编码码率为 48 Kbps。<li>1：高音质，采样率 48k，单声道，编码码率 128 Kbps。<li>2：高音质立体声，采样率 48k，双声道，编码码率 192 Kbps。<br> **高音质设置仅在 `isMixingEnabled` 设为 1 时生效。<br>双声道不支持原始音频数据。** |
| audioIndicationInterval     | 说话者监测的时间间隔。默认禁用。<br /><li>0（默认）：禁用说话者监测的功能。<li> \> 0：说话者监测的时间间隔，单位为 ms 。建议设置时间间隔大于 200 ms。一旦检测到频道内有人说话，命令行中会打印出说话者的 uid。 |
| defaultVideoBg              | 默认画布背景图片的路径。                                     |
| defaultUserBg               | 默认用户背景图片的路径。                                     |
| logLevel                    | 设置日志过滤等级，只有等级不高于所设等级的日志才会被生成。默认为 5。<li>1：日志等级为 Fatal。<li>2：日志等级为 Error。<li>3：日志等级为 Warn。<li>4：日志等级为 Notice。<li>5：日志等级为 Info。<li>6：日志等级为 Debug。 |
| layoutMode                  | 设置合图布局。<li>0：（默认）悬浮布局。第一个加入频道的用户在屏幕上会显示为大视窗，铺满整个画布，其他用户的视频画面会显示为小视窗，从下到上水平排列，最多 4 行，每行 4 个画面，最多支持共 17 个录制画面。</li><li>1：自适应布局。根据用户的数量自动调整每个画面的大小，每个用户的画面大小一致，最多支持 17 个录制画面。</li><li>2：垂直布局。指定一个用户在屏幕左侧显示大视窗画面，其他用户的小视窗画面在右侧垂直排列，最多两列，一列 8 个画面，最多支持共 17 个录制画面。</li> |
| maxResolutionUid            | 如果 `layoutMode` 设为 2（垂直布局），用该参数设置显示大流画面的用户 UID。 |


## 相关文档

录制完成后，你可能需要使用转码脚本将录制的文件进行合成，详见[使用转码脚本](../../cn/Recording/recording_voice_video.md)。 

录制过程中，如果出现错误码或警告码，请参考[警告代码](https://docs.agora.io/cn/Recording/API%20Reference/recording_cpp/namespaceagora_1_1linuxsdk.html#a11cab69078db26c1f166c68e469dcfcf)和[错误代码](https://docs.agora.io/cn/Recording/API%20Reference/recording_cpp/namespaceagora_1_1linuxsdk.html#a5f37e3fa14fad2982f248d247d76996b)。
