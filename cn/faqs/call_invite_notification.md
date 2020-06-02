
---
title: 如何在通话应用中实现呼叫邀请通知功能？
description: 
platform: All Platforms
updatedAt: Tue Jun 02 2020 16:09:23 GMT+0800 (CST)
---
# 如何在通话应用中实现呼叫邀请通知功能？
## 场景描述

为了在通话应用中实现呼叫邀请通知功能，你需要集成 Agora RTC SDK、Agora RTM SDK 和应用对应平台的原生呼叫 API，例如 Android 平台的 ConnectionService、iOS 平台的 CallKit、Flutter 和 React Native 平台的 CallKeep。由于 RTM SDK 只能在应用正常运行时实现呼叫邀请通知功能，集成原生呼叫 API 可以保证当应用进程处于后台或者被关闭时，用户仍可以收到呼叫邀请通知。

## 实现方法

### 步骤一：集成 RTC SDK 和 RTM SDK

RTC SDK 和 RTM SDK 具体的集成文档请参考：

- [RTC SDK 快速开始](../../cn/Interactive%20Broadcast/start_live_android.md)
- [RTM SDK 快速开始](../../cn/Real-time-Messaging/messaging_android.md)

### 步骤二：使用 RTM SDK 实现呼叫邀请的基本控制逻辑

关于如何使用 RTM SDK 实现呼叫邀请功能，请参考[呼叫邀请](../../cn/Real-time-Messaging/rtm_invite_android.md)。

### 步骤三：集成对应平台的原生呼叫 API 并实现呼叫邀请通知

- 对于 Android 平台，你可以参考 [ConnectionService 的官方文档](https://developer.android.com/reference/android/telecom/ConnectionService)和 Agora 提供的[示例项目](https://github.com/AgoraIO-Usecase/Video-Calling/tree/master/OpenDuo-Android)。
- 对于 iOS 平台，你可以参考 [CallKit 的官方文档](https://developer.apple.com/documentation/callkit)和 Agora 提供的[示例项目](https://github.com/AgoraIO-Usecase/Video-Calling/tree/master/OpenDuo-iOS)。
- 对于 Flutter 和 React Native 平台，你可以参考 [CallKeep 的官方文档](https://github.com/react-native-webrtc/react-native-callkeep)。
