
---
title: 频道类常见问题
description: 
platform: 频道类常见问题
updatedAt: Thu Nov 01 2018 08:10:55 GMT+0000 (UTC)
---
# 频道类常见问题
# 频道类常见问题

### 为什么提示我被踢出了设备？

如果一个已经登录的用户，在另一个设备登录，则当前用户会被踢。

### 网络环境差时，SDK 会强行让用户自动退出频道么？

SDK 不会让用户自动退出频道，除非用户自己主动退出，例如，应用程序调用 `leaveChannel`。

### 在每个房间，每个频道内，通话中是否有管理员？

没有管理员的概念，管理员是属于信令层的范围。可以由信令层实现，由信令服务器主动下发命令，调用SDK的接口来实现通话管理。

### 客户端是否需要维护频道？

频道是自动创建和删除的，客户端无需处理和维护。当所有客户端都离开一个频道时，频道自动被删除。

### 什么是 App ID 和 Token? 我如何使用？

更多详细介绍， 以及关于如何获取和使用 App ID 和 Token 的步骤，请参考 [密钥说明](../../cn/Agora%20Platform/token.md)。

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

