
---
title: 自定义音频采集和渲染
description: 
platform: Android
updatedAt: Tue Mar 10 2020 00:01:30 GMT+0800 (CST)
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

开始自定义采集和渲染前，请确保你已在项目中实现基本的通话或者直播功能，详见[一对一通话](../../cn/Video/start_call_android.md)或[互动直播](../../cn/Video/start_live_android.md)。

### 自定义音频采集

参考如下步骤，在你的项目中实现自定义音频采集功能：

1. `joinChannel` 前，通过调用 `setExternalAudioSource` 指定外部音频采集设备。
2. 指定外部采集设备后，开发者自行管理音频数据采集和处理。
3. 完成音频数据处理后，再通过 `pushExternalAudioFrame` 发送给 SDK 进行后续操作。

**API 时序图**

参考下图时序在你的项目中实现自定义音频采集。

![](https://web-cdn.agora.io/docs-files/1568966776336)

**示例代码**

参考下文代码在你的项目中实现自定义音频采集。

```java
// 首先开启外部音频源模式
rtcEngine.setExternalAudioSource(
	true,      // 开启外部音频源
	44100,     // 采样率，可以有8k，16k，32k，44.1k和48kHz等模式
	1          // 外部音源的通道数，最多2个
);

// 持续的输出音频数据
rtcEngine.pushExternalAudioFrame(
	data,             // byte[] 类型的音频数据
	timestamp         // 时间戳
);
```

**API 参考**
* [`setExternalAudioSource`](https://docs.agora.io/cn/Video/API%20Reference/java/classio_1_1agora_1_1rtc_1_1_rtc_engine.html#a5e5630afd7104ee7be8b246ae004efb3)
* [`pushExternalAudioFrame`](https://docs.agora.io/cn/Video/API%20Reference/java/classio_1_1agora_1_1rtc_1_1_rtc_engine.html#a9e219a679d066cfc2544b5e8f9d4d69f)

### 自定义音频渲染

参考如下步骤，在你的项目中实现自定义音频渲染功能：

1. `joinChannel` 前，通过调用 `setExternalAudioSink` 开启并设置外部音频渲染。
2. 成功加入频道后，调用 `pullPlaybackAudioFrame` 拉取远端发送的音频数据。
3. 开发者自行渲染并播放拉取到的音频数据。

**API 时序图**

参考下图时序在你的项目中实现自定义音频渲染。

![](https://web-cdn.agora.io/docs-files/1568966971796)

**示例代码**

参考下文代码在你的项目中实现自定义音频渲染。

```java
// 首先开启外部音频渲染
rtcEngine.setExternalAudioSink(
    true,      // 开启外部音频渲染
    44100,     // 采样率，可以有8k，16k，32k，44.1k 和 48kHz 等模式
    1          // 外部音源的通道数，最多 2 个
);
 
// 主动拉取远端音频帧进行播放
rtcEngine.pullPlaybackAudioFrame(
    data,             // byte[] 类型的音频数据
    lengthInByte      // 拉取的音频数据的字节数，单位为 byte
);
```

**API 参考**

- [`setExternalAudioSink`](https://docs.agora.io/cn/Video/API%20Reference/java/classio_1_1agora_1_1rtc_1_1_rtc_engine.html#a270c0607d443790e92cdbd0d45ba1732)
- [`pullPlaybackAudioFrame`](https://docs.agora.io/cn/Video/API%20Reference/java/classio_1_1agora_1_1rtc_1_1_rtc_engine.html#ae15064944870692e9a0a59fdc87654c4)

## 示例代码

我们在 GitHub 上提供一个开源的 [Custom recorder](https://github.com/AgoraIO/Advanced-Audio/tree/dev/3.1.0_android/Advanced-Audio-Android/sample-custom-recorder/) 示例项目，你可以前往下载，或查看其中的源代码。


## 开发注意事项

自定义音频采集和渲染场景中，需要开发者具有采集或渲染音频数据的能力：

- 自定义音频采集场景中，你需要自行管理音频数据的采集和处理。
- 自定义音频渲染场景中，你需要自行管理音频数据的处理和播放。

