
---
title: 自定义音频采集和渲染
description: 
platform: iOS,macOS
updatedAt: Mon Mar 09 2020 06:59:04 GMT+0800 (CST)
---
# 自定义音频采集和渲染
## 功能介绍

实时音视频传输过程中，Agora SDK 通常会启动默认的音视频模块进行采集和渲染。在以下场景中，你可能会发现默认的音视频模块无法满足开发需求：

- app 中已有自己的音频或视频模块
- 希望使用非 Camera 采集的视频源，如录屏数据
- 需要使用自定义的美颜库或有前处理库
- 某些视频采集设备被系统独占。为避免与其它业务产生冲突，需要灵活的设备管理策略

基于此，Agora Native SDK 支持使用自定义的音视频源或渲染器，实现相关场景。本文介绍如何实现自定义音频采集和渲染。

## 实现方法

开始自定义采集和渲染前，请确保你已在项目中实现基本的通话或者直播功能，详见如下文档：
- iOS：[一对一通话](../../cn/Voice/start_call_ios.md)或[互动直播](../../cn/Voice/start_live_ios.md)
- macOS：[一对一通话](../../cn/Voice/start_call_mac.md)或[互动直播](../../cn/Voice/start_live_mac.md)

### 自定义音频采集

参考如下步骤，在你的项目中实现自定义音频采集功能：

1. `joinChannelByToken` 前，通过调用 `enableExternalAudioSourceWithSampleRate` 指定外部音频采集设备。
2. 指定外部采集设备后，开发者自行管理音频数据采集和处理。
3. 完成音频数据处理后，根据音频数据格式，再通过 `pushExternalAudioFrameRawData` 或 `pushExternalAudioFrameSampleBuffer` 方法发送给 SDK 进行后续操作。

**API 时序图**

参考下图时序在你的项目中实现自定义音频采集。

![](https://web-cdn.agora.io/docs-files/1569378858618)

**示例代码**

参考下文代码在你的项目中实现自定义音频采集。

```swift
// Swift
// swift
// 推入数据类型为 rawData
agoraKit.pushExternalAudioFrameRawData("your rawData", samples: "per push samples", timestamp: 0)

// 推入数据类型为 CMSampleBuffer
agoraKit.pushExternalAudioFrameSampleBuffer("your CMSampleBuffer")
```

```objective-c
// Objective-C
// 推入数据类型为 rawData
[agoraKit pushExternalAudioFrameRawData: "your rawData" samples: "per push samples", timestamp: 0];

// 推入数据类型为 CMSampleBuffer
[agoraKit pushExternalAudioFrameSampleBuffer: "your CMSampleBuffer"];
```

**API 参考**
- [`enableExternalAudioSourceWithSampleRate`](https://docs.agora.io/cn/Voice/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/enableExternalAudioSourceWithSampleRate:channelsPerFrame:)
- [`disableExternalAudioSource`](https://docs.agora.io/cn/Voice/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/disableExternalAudioSource)
- [`pushExternalAudioFrameRawData`](https://docs.agora.io/cn/Voice/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/pushExternalAudioFrameRawData:samples:timestamp:)
- [`pushExternalAudioFrameSampleBuffer`](https://docs.agora.io/cn/Voice/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/pushExternalAudioFrameSampleBuffer:)

### 自定义音频渲染

参考如下步骤，在你的项目中实现自定义音频渲染功能：

1. `joinChannelByToken` 前，通过调用 `enableExternalAudioSink` 开启并设置外部音频渲染。
2. 成功加入频道后，根据音频数据格式，调用 `pullPlaybackAudioFrameRawData` 或 `pullPlaybackAudioFrameSampleBufferByLengthInByte` 方法拉取远端发送的音频数据。
3. 开发者自行渲染并播放拉取到的音频数据。

**API 时序图**

参考下图时序在你的项目中实现自定义音频渲染。

![](https://web-cdn.agora.io/docs-files/1569379341153)

**示例代码**

参考下文代码在你的项目中实现自定义音频渲染。

```swift
// Swift
// 拉取数据类型为 rawData
agoraKit.pullPlaybackAudioFrameRawData("your rawData", lengthInByte: "data length in byte of the external audio data")

// 拉取数据类型为 CMSampleBuffer
agoraKit.pullPlaybackAudioFrameSampleBufferByLengthInByte(lengthInByte: "data length in byte of the external audio data")
```

```objective-c
// Objective-C
// 拉取数据类型为 rawData
[agoraKit pullPlaybackAudioFrameRawData: "your rawData" lengthInByte: "data length in byte of the external audio data"];

// 拉取数据类型为 CMSampleBuffer
[agoraKit pullPlaybackAudioFrameSampleBufferByLengthInByte: lengthInByte: "data length in byte of the external audio data"];
```

**API 参考**

- [`enableExternalAudioSink`](https://docs.agora.io/cn/Voice/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/enableExternalAudioSink:channels:)
- [`disableExternalAudioSink`](https://docs.agora.io/cn/Voice/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/disableExternalAudioSink)
- [`pullPlaybackAudioFrameRawData`](https://docs.agora.io/cn/Voice/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/pullPlaybackAudioFrameRawData:lengthInByte:)
- [`pullPlaybackAudioFrameSampleBufferByLengthInByte`](https://docs.agora.io/cn/Voice/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/pullPlaybackAudioFrameSampleBufferByLengthInByte:)

## 示例项目

我们在 GitHub 提供实现了自定义音频采集和渲染功能的开源示例项目。你可以下载体验，并参考源代码。

- iOS: 参考 Objective-C 示例项目 [AgoraAudioIO-iOS](https://github.com/AgoraIO/Advanced-Audio/tree/master/iOS%26macOS/AgoraAudioIO-Objective-C/AgoraAudioIO-iOS) 中的 [`RoomViewController.m`](https://github.com/AgoraIO/Advanced-Audio/blob/master/iOS%26macOS/AgoraAudioIO-Objective-C/AgoraAudioIO-iOS/RoomViewController.m)。
- macOS: 参考 Objective-C 示例项目 [AgoraAudioIO-macOS](https://github.com/AgoraIO/Advanced-Audio/tree/master/iOS%26macOS/AgoraAudioIO-Objective-C/AgoraAudioIO-macOS) 中的 [`RoomViewController.m`](https://github.com/AgoraIO/Advanced-Audio/blob/master/iOS%26macOS/AgoraAudioIO-Objective-C/AgoraAudioIO-macOS/RoomViewControllerMac.m)。

## 开发注意事项

自定义音频采集和渲染场景中，需要开发者具有采集或渲染音频数据的能力：

- 自定义音频采集场景中，你需要自行管理音频数据的采集和处理。
- 自定义音频渲染场景中，你需要自行管理音频数据的处理和播放。
