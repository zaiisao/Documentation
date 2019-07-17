
---
title: 客户端自定义采集
description: 
platform: Android
updatedAt: Wed Jul 17 2019 09:30:00 GMT+0800 (CST)
---
# 客户端自定义采集
## 功能介绍

实时通信过程中，Agora SDK 通常会启动默认的音频模块进行采集和渲染。如果想要在客户端实现自定义音频采集和渲染，则可以使用自定义的音频源或渲染器，来进行实现。

**自定义采集**主要适用于以下场景：

* 当 SDK 内置的音频源不能满足开发者需求时，比如需要使用自定义的前处理库。
* 开发者的 App 中已有自己的音频模块，为了复用代码，也可以自定义音视频源。

## 实现方法

在开始自定义采集和渲染前，请确保你已完成环境准备、安装包获取等步骤，详见 [集成客户端](../../cn/Audio%20Broadcast/android_audio.md)。

### 自定义音源

你可以使用 Push 方法自定义音频源。该方法下，SDK 默认不会对采集传入的音频数据做消噪等处理。如有音频消噪需求，需要开发者自行实现。

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

#### API 参考
* [`pushExternalAudioFrame`](https://docs.agora.io/cn/Audio%20Broadcast/API%20Reference/java/classio_1_1agora_1_1rtc_1_1_rtc_engine.html#a9e219a679d066cfc2544b5e8f9d4d69f)

## 开发注意事项

客户端自定义采集和渲染属于较复杂的功能，开发者自身需要具备音频相关知识，能够自己独立开发完成采集与渲染。

