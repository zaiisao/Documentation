
---
title: 教师端实现
description: 
platform: Web
updatedAt: Mon Apr 13 2020 13:40:10 GMT+0800 (CST)
---
# 教师端实现
本文展示如何在 Web 平台实现教师端相关功能。

## 基础流程图

参考下图，在你的项目中实现如下功能：

- 教师端登录登出

![](https://web-cdn.agora.io/docs-files/1579596188973)

- 教师端回放

![](https://web-cdn.agora.io/docs-files/1579576003209)

## 集成指引

根据下表链接，下载对应的 SDK，参考集成文档的步骤将 SDK 集成到你的项目中。

<%

if (platform == "android") 
{ %>
| 产品 | SDK 下载 | 集成文档 |
| ---------------- | ---------------- | ---------------- |
| [RTC (Real-time Communication) SDK](https://docs.agora.io/cn/Interactive%20Broadcast/product_live?platform=All%20Platforms)      | [Android 视频互动直播 SDK](https://download.agora.io/sdk/release/Agora_Native_SDK_for_Android_v2_9_0_103_FULL_20200325_1695.zip)      | [实现互动直播](https://docs.agora.io/cn/Interactive%20Broadcast/start_live_android?platform=Android) |
| [RTM (Real-time Messaging) SDK](https://docs.agora.io/cn/Real-time-Messaging/product_rtm?platform=All%20Platforms) | [Android 实时消息 SDK](https://docs.agora.io/cn/Real-time-Messaging/downloads) | [收发点对点消息和频道消息](https://docs.agora.io/cn/Real-time-Messaging/messaging_android?platform=Android) |
| [白板](https://developer.netless.link/docs/android/overview/android-introduction/) | [SDK 集成](https://developer.netless.link/docs/android/quick-start/android-prepare/) | [白板快速开始](https://developer.netless.link/docs/android/quick-start/android-init-sdk/) |

<%
if (product == "1v1") 
{ %>
| 产品 | SDK 下载 | 集成文档 |
| ---------------- | ---------------- | ---------------- |
| [RTC (Real-time Communication) SDK](https://docs.agora.io/en/Video/product_video?platform=All%20Platforms)      | [Android 视频通话 SDK](https://download.agora.io/sdk/release/Agora_Native_SDK_for_Android_v2_9_0_103_FULL_20200325_1695.zip)      | [实现视频通话](https://docs.agora.io/cn/Video/start_call_android?platform=Android) |
| [RTM (Real-time Messaging) SDK](https://docs.agora.io/cn/Real-time-Messaging/product_rtm?platform=All%20Platforms) | [Android 实时消息 SDK](https://docs.agora.io/cn/Real-time-Messaging/downloads) | [收发点对点消息和频道消息](https://docs.agora.io/cn/Real-time-Messaging/messaging_android?platform=Android) |
| [白板](https://developer.netless.link/docs/android/overview/android-introduction/) | [SDK 集成](https://developer.netless.link/docs/android/quick-start/android-prepare/) | [白板快速开始](https://developer.netless.link/docs/android/quick-start/android-init-sdk/) |
<% }
<% }

if (platform == "ios")
{ %>
| 产品 | SDK 下载 | 集成文档 |
| ---------------- | ---------------- | ---------------- |
| [RTC (Real-time Communication) SDK](https://docs.agora.io/cn/Interactive%20Broadcast/product_live?platform=All%20Platforms)      | [iOS 视频互动直播 SDK](https://download.agora.io/sdk/release/Agora_Native_SDK_for_iOS_v2_9_0_103_FULL_20200325_2479.zip)     | [实现互动直播](https://docs.agora.io/cn/Interactive%20Broadcast/start_live_ios?platform=iOS) |
| [RTM (Real-time Messaging) SDK](https://docs.agora.io/cn/Real-time-Messaging/product_rtm?platform=All%20Platforms) | [iOS 实时消息 SDK](https://docs.agora.io/cn/Real-time-Messaging/downloads) | [收发点对点消息和频道消息](https://docs.agora.io/cn/Real-time-Messaging/messaging_ios?platform=iOS) |
| [白板](https://developer.netless.link/docs/ios/overview/ios-introduction/) | [SDK 集成](https://developer.netless.link/docs/ios/quick-start/ios-prepare/) | [白板快速开始](https://developer.netless.link/docs/ios/quick-start/ios-init-sdk/) | 

<%
if (product == "1v1")
{ %>
| 产品 | SDK 下载 | 集成文档 |
| ---------------- | ---------------- | ---------------- |
| [RTC (Real-time Communication) SDK](https://docs.agora.io/en/Video/product_video?platform=All%20Platforms)      | [iOS 视频通话 SDK](https://download.agora.io/sdk/release/Agora_Native_SDK_for_iOS_v2_9_0_103_FULL_20200325_2479.zip)     | [实现视频通话](https://docs.agora.io/cn/Video/start_call_ios?platform=iOS) |
| [RTM (Real-time Messaging) SDK](https://docs.agora.io/cn/Real-time-Messaging/product_rtm?platform=All%20Platforms) | [iOS 实时消息 SDK](https://docs.agora.io/cn/Real-time-Messaging/downloads) | [收发点对点消息和频道消息](https://docs.agora.io/cn/Real-time-Messaging/messaging_ios?platform=iOS) |
| [白板](https://developer.netless.link/docs/ios/overview/ios-introduction/) | [SDK 集成](https://developer.netless.link/docs/ios/quick-start/ios-prepare/) | [白板快速开始](https://developer.netless.link/docs/ios/quick-start/ios-init-sdk/) | 
<% }
<% }

if (platform == "webstudent") { %>
| 产品 | SDK 下载 | 集成文档 |
| ---------------- | ---------------- | ---------------- | 
| [RTC (Real-time Communication) SDK](https://docs.agora.io/cn/Interactive%20Broadcast/product_live?platform=All%20Platforms)      | [ Web 视频互动直播 SDK](https://docs.agora.io/cn/Interactive%20Broadcast/downloads)      | [实现互动直播](https://docs.agora.io/cn/Interactive%20Broadcast/start_live_web?platform=Web) |
| [RTM (Real-time Messaging) SDK](https://docs.agora.io/cn/Real-time-Messaging/product_rtm?platform=All%20Platforms) | [Web 实时消息 SDK](https://docs.agora.io/cn/Real-time-Messaging/downloads) | [收发点对点消息和频道消息](https://docs.agora.io/cn/Real-time-Messaging/messaging_web?platform=Web) |
| [白板](https://developer.netless.link/docs/javascript/overview/js-outline/) | [SDK 集成](https://developer.netless.link/docs/javascript/guide/js-sdk/) | [白板快速开始](https://developer.netless.link/docs/javascript/quick-start/js-precondition/) |

<%
if (product == "1v1") { %>
| 产品 | SDK 下载 | 集成文档 |
| ---------------- | ---------------- | ---------------- | 
| [RTC (Real-time Communication) SDK](https://docs.agora.io/en/Video/product_video?platform=All%20Platforms)      | [ Web 视频通话 SDK](https://docs.agora.io/cn/Interactive%20Broadcast/downloads)      | [实现视频通话](https://docs.agora.io/cn/Video/start_call_web?platform=Web) |
| [RTM (Real-time Messaging) SDK](https://docs.agora.io/cn/Real-time-Messaging/product_rtm?platform=All%20Platforms) | [Web 实时消息 SDK](https://docs.agora.io/cn/Real-time-Messaging/downloads) | [收发点对点消息和频道消息](https://docs.agora.io/cn/Real-time-Messaging/messaging_web?platform=Web) |
| [白板](https://developer.netless.link/docs/javascript/overview/js-outline/) | [SDK 集成](https://developer.netless.link/docs/javascript/guide/js-sdk/) | [白板快速开始](https://developer.netless.link/docs/javascript/quick-start/js-precondition/) |
<% }
<% }

if (platform == "webteacher") { %>
| 产品 | SDK 下载 | 集成文档 |
| ---------------- | ---------------- | ---------------- | 
| [RTC (Real-time Communication) SDK](https://docs.agora.io/cn/Interactive%20Broadcast/product_live?platform=All%20Platforms)      | [ Web 视频互动直播 SDK](https://docs.agora.io/cn/Interactive%20Broadcast/downloads)      | [实现互动直播](https://docs.agora.io/cn/Interactive%20Broadcast/start_live_web?platform=Web) |
| [RTM (Real-time Messaging) SDK](https://docs.agora.io/cn/Real-time-Messaging/product_rtm?platform=All%20Platforms) | [Web 实时消息 SDK](https://docs.agora.io/cn/Real-time-Messaging/downloads) | [收发点对点消息和频道消息](https://docs.agora.io/cn/Real-time-Messaging/messaging_web?platform=Web) |
| [云端录制](https://docs.agora.io/cn/cloud-recording/product_cloud_recording?platform=All%20Platforms) | / | [使用 RESTful API 录制](https://docs.agora.io/cn/cloud-recording/cloud_recording_rest?platform=All%20Platforms) |
| [白板](https://developer.netless.link/docs/javascript/overview/js-outline/) | [SDK 集成](https://developer.netless.link/docs/javascript/guide/js-sdk/) | [白板快速开始](https://developer.netless.link/docs/javascript/quick-start/js-precondition/) |

<%
if (product == "1v1") { %>
| 产品 | SDK 下载 | 集成文档 |
| ---------------- | ---------------- | ---------------- | 
| [RTC (Real-time Communication) SDK](https://docs.agora.io/en/Video/product_video?platform=All%20Platforms)      | [ Web 视频通话 SDK](https://docs.agora.io/cn/Interactive%20Broadcast/downloads)      | [实现视频通话](https://docs.agora.io/cn/Video/start_call_web?platform=Web) |
| [RTM (Real-time Messaging) SDK](https://docs.agora.io/cn/Real-time-Messaging/product_rtm?platform=All%20Platforms) | [Web 实时消息 SDK](https://docs.agora.io/cn/Real-time-Messaging/downloads) | [收发点对点消息和频道消息](https://docs.agora.io/cn/Real-time-Messaging/messaging_web?platform=Web) |
| [云端录制](https://docs.agora.io/cn/cloud-recording/product_cloud_recording?platform=All%20Platforms) | / | [使用 RESTful API 录制](https://docs.agora.io/cn/cloud-recording/cloud_recording_rest?platform=All%20Platforms) |
| [白板](https://developer.netless.link/docs/javascript/overview/js-outline/) | [SDK 集成](https://developer.netless.link/docs/javascript/guide/js-sdk/) | [白板快速开始](https://developer.netless.link/docs/javascript/quick-start/js-precondition/) |
<% }

%>

## 核心 API 时序图

参考下图时序，将 [RTC SDK](https://docs.agora.io/cn/Agora%20Platform/terms?platform=All%20Platforms#agora-rtc-sdk) 和 [RTM SDK](https://docs.agora.io/cn/Agora%20Platform/terms?platform=All%20Platforms#agora-rtm-sdk) 集成到你的项目中。

![](https://web-cdn.agora.io/docs-files/1581388838880)


## 核心 API 参考

- RTM SDK

| API | 实现功能 | 
| ---------------- | ---------------- | 
| [createInstance](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_web/modules/agorartm.html#createinstance)      | 创建并返回一个 RtmClient 实例。      |
| [login](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_web/classes/rtmclient.html#login) | 登录 Agora RTM 系统。 |
| [createChannel](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_web/classes/rtmclient.html#createchannel) | 创建 Agora RTM 频道。一个 RtcClient 可以创建多个频道。 |
| [join](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_web/classes/rtmchannel.html#join) | 加入 Agora RTM 频道。|
| [getChannelAttributes](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_web/classes/rtmclient.html#getchannelattributes) | 获取指定频道的频道信息。|
| [queryPeersOnlineStatus](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_web/classes/rtmclient.html#querypeersonlinestatus) | 查询指定用户的在线状态。|
| [sendMessage](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_web/classes/rtmchannel.html#sendmessage) | 发送频道消息。成功发送后，频道内所有用户都能收到。|
| [leave](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_web/classes/rtmchannel.html#leave) | 离开 RTM 频道。|

- RTC SDK

| API | 实现功能 |
| ---------------- | ---------------- |
| [createClient](https://docs.agora.io/cn/Interactive%20Broadcast/API%20Reference/web/globals.html#createclient)      | 创建客户端。      |
| [Client.init](https://docs.agora.io/cn/Interactive%20Broadcast/API%20Reference/web/interfaces/agorartc.client.html#init) | 初始化客户端对象。 |
| [Client.setClientRole](https://docs.agora.io/cn/Interactive%20Broadcast/API%20Reference/web/interfaces/agorartc.client.html#setclientrole) | 设置直播场景下的用户角色。本场景中，我们将老师的用户角色设为主播，可以与同为主播的学生进行互动。|
| [createStream](https://docs.agora.io/cn/Interactive%20Broadcast/API%20Reference/web/globals.html#createstream) | 创建并返回音视频流对象。 |
| [Stream.init](https://docs.agora.io/cn/Interactive%20Broadcast/API%20Reference/web/interfaces/agorartc.stream.html#init) | 初始化音视频对象。 |
| [Client.join](https://docs.agora.io/cn/Interactive%20Broadcast/API%20Reference/web/interfaces/agorartc.client.html#join) | 加入 Agora RTC 频道。 |
| [Client.publish](https://docs.agora.io/cn/Interactive%20Broadcast/API%20Reference/web/interfaces/agorartc.client.html#publish) | 发布本地音视频流至 SD-RTN。 |
| [Client.on](https://docs.agora.io/cn/Interactive%20Broadcast/API%20Reference/web/interfaces/agorartc.client.html#on)("stream-added") | 远端音视频已添加。 |
| [Client.subscribe](https://docs.agora.io/cn/Interactive%20Broadcast/API%20Reference/web/interfaces/agorartc.client.html#subscribe) | 订阅远端音视频流。|
| [Stream.play](https://docs.agora.io/cn/Interactive%20Broadcast/API%20Reference/web/interfaces/agorartc.stream.html#play) | 播放音、视频流。|
| [Client.leave](https://docs.agora.io/cn/Interactive%20Broadcast/API%20Reference/web/interfaces/agorartc.client.html#leave) | 离开 RTC 频道。 |



## 附加功能

你还可以参考下列文档或示例代码，在你的项目中实现更多教育场景的附加功能。


<details>
<summary>网络质量监测</summary>
你可以通过使用 RTC SDK 的 <code>on("network-quality")</code> 回调，实时监控通话中每个用户的网络上下行 last mile 网络质量。
更多质量透明相关方法，可参考如下文档：
<li><a href="https://docs.agora.io/cn/Interactive%20Broadcast/lastmile_quality_web?platform=Web">通话前网络质量探测</a></li>
<li><a href="https://docs.agora.io/cn/Interactive%20Broadcast/in-call_quality_web?platform=Web">通话中质量监测</a></li>
</details>
<details>
<summary>关闭本地音视频</summary>
你可以通过调用 RTC SDK 的如下方法，实现相关功能：
	<li>调用 <code>muteAudio</code> 或 <code>unmuteAudio</code>关闭或重新开启本地音频。</li>
	<li>调用 <code>muteVideo</code> 或 <code>unmuteVideo</code> 关闭或重新开启本地视频。</li>
</details>
 
<details>
<summary>关闭远端音视频</summary>
你需要联合调用 RTM SDK 和 RTC SDK 的方法，实现该功能：
<ol>
	<li>教师端调用 <code>sendMessageToPeer</code> 方法，给学生发送点对点消息，通知学生关闭音视频。</li>
	<li>学生端调用对应的 <code>mute</code> 方法关闭本地的音视频。</li>
</ol>
</details>
<details>
<summary>屏幕共享</summary>
根据你的浏览器，参考如下文档实现屏幕共享功能：
<li><a href="https://docs.agora.io/cn/Interactive%20Broadcast/screensharing_web?platform=Web#a-name--chromeachrome-%E5%B1%8F%E5%B9%95%E5%85%B1%E4%BA%AB">Chrome 屏幕共享</a></li>
<li><a href="https://docs.agora.io/cn/Interactive%20Broadcast/screensharing_web?platform=Web#a-nameffafirefox-%E5%B1%8F%E5%B9%95%E5%85%B1%E4%BA%AB">Firefox 屏幕共享</a></li>
</details>

<details>
<summary>白板</summary>
参考下列常用功能文档，在你的项目中实现白板相关功能。
	<li><a href="https://developer.netless.link/docs/javascript/features/js-document/">文档转换</a></li>
	<li><a href="https://developer.netless.link/docs/javascript/features/js-state/">状态监听</a></li>
	<li><a href="https://developer-en.netless.link/docs/javascript/features/js-tools/">使用教具</a></li>
	<li><a href="https://developer-en.netless.link/docs/javascript/features/js-view/">视角操作</a></li>
	<li><a href="https://developer.netless.link/docs/javascript/features/js-operation/">白板操作</a></li>
	<li><a href="https://developer.netless.link/docs/javascript/features/js-scenes/">页面（场景）管理</a></li>
</details>


## 开源示例项目

我们也在 GitHub 上提供了互动直播大课的[开源示例项目](https://github.com/AgoraIO-Usecase/eEducation/tree/master/education_web)，你也可以前往下载，或查看其中的源代码。
