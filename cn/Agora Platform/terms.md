
---
title: 术语库
description: 
platform: All Platforms
updatedAt: Fri Jul 03 2020 11:25:24 GMT+0800 (CST)
---
# 术语库
## A
#### <a name="agora-rtc-sdk"></a>[**<u>Agora RTC SDK</u>**](../../cn/Agora%20Platform/term_agora_rtc_sdk.md)

Agora RTC (Real-Time Communication) SDK 是声网提供的用于实现音视频实时通信的 SDK。

#### <a name="agora-rtm-sdk"></a>[**<u>Agora RTM SDK</u>**](../../cn/Agora%20Platform/term_agora_rtm_sdk.md)

Agora RTM (Real-time Messaging) SDK 是声网提供的用于实现消息通道、呼叫、聊天、状态同步等功能的 SDK。

#### <a name="rtm-native-sdk"></a>**Agora RTM Native SDK**

Android、iOS、macOS、Linux Java、Linux C++ 和 Windows C++ 平台的 [Agora RTM SDK](#agora-rtm-sdk) 通常被统称为 Agora RTM Native SDK。
	
#### <a name="agora-cloud-backup"></a>[**<u>Agora 云备份 (Agora Cloud Backup)</u>**](../../cn/Agora%20Platform/agora_cloud_backup.md)

Agora 云备份是云端录制使用的备份云存储服务，当录制服务无法将录制文件上传至开发者指定的第三方云存储时，会自动将录制文件暂存至备份云。

#### <a name="appid"></a>[**<u>App ID</u>**](../../cn/Agora%20Platform/term_appid.md)

App ID 是 Agora 为开发项目生成的字符串，是项目的唯一标识。

#### <a name="appcertificate"></a>[**<u>App 证书 (App Certificate)</u>**](../../cn/Agora%20Platform/term_app_certificate.md)

App 证书是 Agora 控制台为开发项目生成的字符串，用于开启 Token 鉴权，并作为生成 Token 的参数之一。

## B

#### <a name="on-premise-recording"></a>[**<u>本地服务端录制 (On-premise Recording)</u>**](../../cn/Agora%20Platform/on_premise_recording.md)

本地服务端录制是声网针对音视频通话或直播研发的录制组件，用于部署在本地服务端实现录制功能。

#### <a name="caller"></a>**被叫 (callee)</u>**

被叫是指接收<a href="#call-invitation">呼叫邀请</a>的用户。

## D

#### <a name="high-stream"></a>**大流 (high-quality video stream)**

开启双流模式后，发送端发送的视频双流中，分辨率更大、码率更高的那路视频流就是视频大流。详见[双流模式](#dual-stream)。

#### <a name="individual-recording"></a>[**<u>单流录制模式 (individual recording mode)</u>**](../../cn/Agora%20Platform/individual_recording_mode.md)

单流录制模式指录制服务（包括本地服务端录制和云端录制）分开录制频道内每个 UID 的音频流和视频流。

#### <a name="peer-to-peer"></a>[**<u>点对点消息 (peer-to-peer message)</u>**](../../cn/Agora%20Platform/peer_to_peer_message.md)

点对点消息是指 Agora RTM SDK 中一个用户向另一个在线或离线的用户发送的消息。

#### <a name="sub"></a>[**<u>订阅 (subscribe)</u>**](../../cn/Agora%20Platform/subscribe.md)

对于 Agora RTC SDK，订阅是指频道中的用户接收频道内已发布的音视频流的操作。对于 Agora RTM SDK，订阅是指对一个或多个用户的在线状态进行监控。

#### <a name="packet-loss"></a>[**<u>丢包 (packet loss)</u>**](../../cn/Agora%20Platform/packet_loss.md)

丢包是指数据传输过程中发生的数据包丢失现象。

#### <a name="jitter"></a>[**<u>抖动 (jitter)</u>**](../../cn/Agora%20Platform/jitter.md)

实时音视频通信中，连续传输的数据包之间的延时不一致称为抖动。

## F
#### <a name="pub"></a>[**<u>发布 (publish)</u>**](../../cn/Agora%20Platform/publish.md)

发布是指频道中的用户将音视频数据发送到频道的操作。

## G

#### <a name="audience"></a>[**<u>观众 (audience)</u>**](../../cn/Agora%20Platform/term_audience.md)

观众指频道内没有发流权限的用户。

## H

#### <a name="stream-mixing"></a>[**<u>合流 (stream mixing)</u>**](../../cn/Agora%20Platform/stream_mixing.md)

合流指将多路媒体流合并为一路流的过程，可以包括视频流的合并（合图）和音频流的合并（混音）。

#### <a name="layout"></a>[**<u>合流布局 (video layout)</u>**](../../cn/Agora%20Platform/video_layout.md)

合流布局，指将多个用户的音视频流混合为一路音视频流时，频道内各用户画面的大小及其在视频画布上的位置。

#### <a name="composite--recording"></a>[**<u>合流录制模式 (composite recording mode)</u>**](../../cn/Agora%20Platform/composite_recording_mode.md)

合流录制模式指录制服务（包括本地服务端录制和云端录制）将频道内所有 UID 的音视频混合录制为一个音视频文件。

#### <a name="video-mixing"></a>[**<u>合图 (video mixing)</u>**](../../cn/Agora%20Platform/video_mixing.md)

合图指将多路视频流合并为一路视频流的过程。

#### <a name="call-invitation"></a>**呼叫邀请 (call invitation)**

呼叫邀请是指 Agora 基于 Agora RTM SDK 的点对点消息功能封装的一套会话协议，可实现发起、结束、接受、拒绝会话的功能。详见[呼叫邀请](../../cn/Real-time-Messaging/rtm_invite_android.md)。

#### <a name="loopback-test"></a>**回路测试 (loopback test)**

回路测试是指通信设备发出信号、信号经传输后又回到该设备的一种测试方式，通常用于测试通信设备是否能正常工作。详见[音视频设备测试](https://docs.agora.io/cn/Interactive%20Broadcast/test_switch_device_android)。

#### <a name="audio-mixing"></a>[**<u>混音 (audio mixing)</u>**](../../cn/Agora%20Platform/audio_mixing.md)

混音是指将两路以上的音频流混合成一路音频流的过程。

## J

#### <a name="mirror"></a>[**<u>镜像 (mirror)</u>**](../../cn/Agora%20Platform/mirror.md)

镜像是视频画面呈现的一种效果。

## K

#### <a name="freeze"></a>[**<u>卡顿 (freeze)</u>**](../../cn/Agora%20Platform/freeze.md)

卡顿是实时音视频传输过程中，因网络条件、设备性能受限等原因，引起的音频或视频播放断续、不流畅、甚至定格等现象。

#### <a name="console"></a>[**<u>控制台 (Agora Console)</u>**](../../cn/Agora%20Platform/agora_console.md)

控制台是声网提供给开发者管理声网各项服务的工具。

## L

#### <a name="last-mile"></a>[**<u>last mile</u>**](../../cn/Agora%20Platform/last_mile.md)

Last mile 指 Agora 边缘服务器和终端用户设备之间的网络。

#### <a name="message_history"></a>[**<u>历史消息 (message history)</u>**](../../cn/Agora%20Platform/message_history.md)

历史消息是指用户已发送的点对点消息或频道消息。

#### <a name="offline"></a>[**<u>离线 (offline)</u>**](../../cn/Agora%20Platform/offline.md)

离线是指用户成功登出 Agora RTM 系统或与 Agora RTM 系统断开连接 30 秒之后的状态。

#### <a name="offline_message"></a>[**<u>离线消息 (offline message)</u>**](../../cn/Agora%20Platform/offline_message.md)

离线消息是指用户向一个离线用户发送的点对点消息。

#### <a name="co-hosting"></a>[**<u>连麦 (co-hosting)</u>**](../../cn/Agora%20Platform/co_hosting.md)

连麦指在直播场景中，两个及以上主播进行互动的过程。

#### <a name="fallback"></a>[**<u>流回退 (stream fallback)</u>**](../../cn/Agora%20Platform/stream_fallback.md)

在多人实时音视频场景中，如果网络环境不理想，无法同时保证音频和视频的质量，实时音视频的质量就会下降。

## M
#### <a name="media-player"></a>[**<u>媒体播放器组件 (MediaPlayer Kit)</u>**](../../cn/Agora%20Platform/mediaplayer_kit.md)

媒体播放器组件是 Agora RTC SDK 的一个组件，适用于直播场景下播放本地或在线媒体资源，并将主播播放的媒体流发送给其他用户。

#### <a name="media-stream"></a>[**<u>媒体流 (media stream)</u>**](../../cn/Agora%20Platform/media_stream.md)

媒体流是指包含媒体数据的对象。

## P
#### <a name="channel"></a>[**<u>频道 (channel)</u>**](../../cn/Agora%20Platform/channel.md)

对于 Agora RTC SDK，频道是由开发者调用 Agora 提供的 API 创建的用于传输实时数据的通道。对于 Agora RTM SDK，频道是用来接收<a href="#channel-message">频道消息</a>的用户组。

#### <a name="channel-profile"></a>[**<u>频道场景 (channel profile)</u>**](../../cn/Agora%20Platform/channel_profile.md)

频道场景是 Agora 为了对不同的实时音视频场景进行针对性算法优化而提供的一种设置选项。

#### <a name="channel-attribute"></a>[**<u>频道属性 (channel attribute)</u>**](../../cn/Agora%20Platform/channel_attribute.md)

频道属性是指 Agora RTM SDK 中为 RTM 频道添加的标签信息，包含属性名、属性值、最后一次更新属性的用户 ID、最后一次更新时间。

#### <a name="channel-message"></a>[**<u>频道消息 (channel message)</u>**](../../cn/Agora%20Platform/channel_message.md)

频道消息是指 Agora RTM SDK 中一个用户向所在频道内所有用户发送的消息。

## Q
#### <a name="slice"></a>**切片 (slice)**

切片指在录制过程中将音视频数据按照一定规则进行定期切割，生成多个录制文件的行为。切片后会生成多个切片文件（如 TS 或 WebM 文件）以及用于存储切片文件索引的 M3U8 文件。详见[管理录制文件](https://docs.agora.io/cn/cloud-recording/cloud_recording_manage_files)。

## S
#### <a name="sd-rtn"></a>[**<u>SD-RTN™</u>**](../../cn/Agora%20Platform/sd_rtn.md)

SD-RTN™ 是 Software Defined Real-time Network 的缩写，即软件定义实时网，这是声网自建的底层实时传输网络。

#### <a name="become-host"></a>[**<u>上麦 (becoming a host)</u>**](../../cn/Agora%20Platform/becoming_a_host.md)

上麦指直播场景中的观众切换用户角色成为主播这一行为。

#### <a name="rtsa"></a>[**<u>实时码流加速 (RTSA, Real-Time Streaming Acceleration)</u>**](../../cn/Agora%20Platform/rtsa.md)

实时码流加速是声网提供的用于实现自定义音视频码流在互联网上的实时传输的产品。

#### <a name="video-profile"></a>[**<u>视频属性 (video profile)</u>**](../../cn/Agora%20Platform/video_profile.md)

视频属性指本地编码视频的分辨率、码率、帧率等属性，又称为 “视频编码属性”。
	
#### <a name="render-first-video"></a>[**<u>首帧出图 (render the first video frame)</u>**](../../cn/Agora%20Platform/render_the_first_video_frame.md)

首帧出图指视频的第一帧在本地设备上渲染显示。
	
#### <a name="dual-stream"></a>[**<u>双流模式 (dual-stream mode)</u>**](../../cn/Agora%20Platform/dual_stream_mode.md)

双流模式是指 Agora RTC SDK 在发送一路视频流的同时，额外发送一路低分辨率、低码率的视频流。

#### <a name="inject-stream"></a>**输入在线媒体流 (Inject Online Media Stream)**

输入在线媒体流指直播场景下，输入在线音视频流至 Agora 频道内供所有用户欣赏。Agora RTC SDK 为开发者提供接口允许主播输入一路在线音视频流或纯音频流至 Agora 频道。详见进阶功能文档[输入在线媒体流](https://docs.agora.io/cn/Interactive%20Broadcast/inject_stream_android)。
	
#### <a name="agora-analytics"></a>[**<u>水晶球 (Agora Analytics)</u>**](../../cn/Agora%20Platform/agora_analytics.md)

水晶球是声网提供给开发者对全周期通话质量进行监测、回溯和分析的工具。

## T

#### <a name="sound-localization"></a>[**<u>听声辨位 (sound localization)</u>**](../../cn/Agora%20Platform/sound_localization.md)

听声辨位，是指通过双耳间的音量差、时间差和音色辨别声音的距离和方位。

#### <a name="token"></a>[**<u>Token</u>**](../../cn/Agora%20Platform/term_token.md)

Token 也称为动态密钥，用于在生产环境等安全要求更高的环境下对 app 用户在加入 RTC 频道或登录 RTM 系统时进行动态鉴权。

#### <a name="push-stream-cdn"></a>[**<u>推流到 CDN (Push streams to CDN)</u>**](../../cn/Agora%20Platform/push-stream-cdn.md)

推流到 CDN（Content Delivery Network），也称“旁路推流”或“CDN 直播推流”，指 Agora 服务器将 RTC 频道内的流由 Agora 私有协议转换为标准协议，然后推到第三方 CDN。

#### <a name="file_message"></a>**图片消息 (image message)**

图片消息是指包含图片的文件名、文件大小、尺寸、缩略图、media ID 等相关信息的点对点消息或频道消息。用户可以使用图片消息向其他用户发送图片。详见[上传和下载图片或文件](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_java/index.html#multimedia)。

## W

#### <a name="file_message"></a>**文件消息 (file message)**

文件消息是指包含文件名、文件大小、缩略图、media ID 等相关信息的点对点消息或频道消息。用户可以使用文件消息向其他用户发送文件。详见[上传和下载图片或文件](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_java/index.html#multimedia)。

## X
#### <a name="become-audience"></a>[**<u>下麦 (becoming an audience)</u>**](../../cn/Agora%20Platform/becoming_an_audience.md)

下麦指直播场景中的主播切换用户角色成为观众这一行为。

#### <a name="low-stream"></a>**小流 (low-quality video stream)**

开启双流模式后，发送端发送的视频双流中，分辨率跟小、码率更低的那路视频流就是视频小流。详见[双流模式](#dual-stream)。

## Y

#### <a name="delay"></a>[**<u>延时 (delay)</u>**](../../cn/Agora%20Platform/delay.md)

实时音视频通信中，延时是指数据从发送端到接收端需要的时间。

#### <a name="audio-profile"></a>[**<u>音频编码属性 (audio profile)</u>**](../../cn/Agora%20Platform/audio_profile.md)

音频编码属性是指本地编码音频的采样率、编码策略、声道数、码率等属性。

#### <a name="audio-route"></a>[**<u>音频路由 (audio route)</u>**](../../cn/Agora%20Platform/term_audio_route.md)

音频路由是指 app 在播放音频时使用的音频输出设备。

#### <a name="username"></a>**用户 ID (user ID)**

在加入 RTC [频道](#channel)时需要传入用户 ID 用于标识频道中的用户。在登录 [RTM](#agora-rtm-sdk) 系统时需要传入用户 ID 用于标识 RTM 系统中的用户。RTC 频道的用户 ID 和 RTM 系统的用户 ID 是相互独立的。

#### <a name="user-role"></a>[**<u>用户角色 (user role)</u>**](../../cn/Agora%20Platform/user_role.md)

用户角色用于定义频道内用户是否有发流权限。

#### <a name="user_attribute"></a>[**<u>用户属性 (user attribute)</u>**](../../cn/Agora%20Platform/user_attribute.md)

用户属性是指 Agora RTM SDK 中为用户添加的标签信息，包含属性名和属性值。

#### <a name="raw-data"></a>[**<u>原始音视频数据 (raw data)</u>**](../../cn/Agora%20Platform/raw_data.md)

原始音视频数据，又称音视频裸数据，是指音视频传输过程中获取到的纯音视频数据。

#### <a name="cloud-proxy"></a>[**<u>云代理 (Cloud Proxy)</u>**](../../cn/Agora%20Platform/term_cloud_proxy.md)

云代理是 Agora 提供的用于穿透防火墙限制，使用固定 IP 连接到 Agora 服务器的代理服务。

#### <a name="cloud-recording"></a>[**<u>云端录制 (Cloud Recording)</u>**](../../cn/Agora%20Platform/cloud_recording.md)

云端录制是声网针对音视频通话和直播研发的录制组件，提供云端录制 RESTful API 供开发者实现录制功能，并将录制文件存至第三方云存储。

## Z

#### <a name="online"></a>[**<u>在线 (online)</u>**](../../cn/Agora%20Platform/online.md)

在线是指用户成功登录 Agora RTM 系统之后的状态。

#### <a name="host"></a>[**<u>主播 (host/broadcaster)</u>**](../../cn/Agora%20Platform/term_host.md)

主播指频道内有发流权限的用户。

#### <a name="caller"></a>**主叫 (caller)</u>**

主叫是指发送<a href="#call-invitation">呼叫邀请</a>的用户。

#### <a name="transcoding"></a>[**<u>转码 (transcoding)</u>**](../../cn/Agora%20Platform/transcoding.md)

转码指将音视频解码并重新编码，从而实现格式、属性等的转换的过程。

#### <a name="custom-source"></a>[**<u>自采集 (custom source)</u>**](../../cn/Agora%20Platform/custom_source.md)
自采集，又称自定义采集，是指 app 自行采集音视频数据的过程。

#### <a name="custom-rendering "></a>[**<u>自渲染 (custom rendering)</u>**](../../cn/Agora%20Platform/custom_rendering.md)

自渲染，又称自定义渲染，是指开发者从 SDK 获取原始音视频数据后自行渲染的过程。
