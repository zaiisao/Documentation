
---
title: Custom Audio Source and Renderer
description: 
platform: Android
updatedAt: Mon Jul 06 2020 10:35:05 GMT+0800 (CST)
---
# Custom Audio Source and Renderer
## Introduction

Before reading this article, you need to undersand two concepts: audio source and audio sink. Audio source is where audio frames come from, such as a microphone and a recorder. Audio sink is where audio frames go, such as a speaker and a earpiece.

By default, the Agora SDK uses default audio and video modules for capturing and rendering in real-time communications. 

However, the default modules might not meet your development requirements, such as in the following scenarios:

- Your app has its own audio or video module.
- You want to use a non-camera source, such as recorded screen data.
- You need to process the captured video with a pre-processing library for functions such as image enhancement.
- You need flexible device resource allocation to avoid conflicts with other services.

This article tells you how to use the Agora Native SDK to customize the audio source and sink.

## Implementation

Before customizing the audio source or sink, ensure that you implement the basic real-time communication functions in your project. For details, see [Start a Call](../../en/Audio%20Broadcast/start_call_android.md) or [Start Live Interactive Streaming](../../en/Audio%20Broadcast/start_live_android.md).

### Custom audio source

Refer to the following steps to customize the audio source in your project:

1. Call the `setExternalAudioSource` method to enable the external audio source before joining a channel.
2. Record and process the audio data on your own.
3. Send the audio data back to the SDK using the `pushExternalAudioFrame` method.

**API call sequence**

Refer to the following diagram to customize the audio source in your project.

![](https://web-cdn.agora.io/docs-files/1568968141511)

**Sample code**

Refer to the following code to customize the audio source in your project.

```java
// Enable the external audio source mode.
rtcEngine.setExternalAudioSource(
	true,      // Enable the external audio source.
	44100,     // Sampling rate. Set it as 8k, 16k, 32k, 44.1k, or 48kHz.
	1          // The number of external audio channels. The maximum value is 2.
);

// To continually push the enternal audio frame.
rtcEngine.pushExternalAudioFrame(
	data,             // The audio data in the format of byte[].
	timestamp         // The timestamp of the audio frame.
);
```

**API Reference**
- [`setExternalAudioSource`](https://docs.agora.io/en/Audio%20Broadcast/API%20Reference/java/classio_1_1agora_1_1rtc_1_1_rtc_engine.html#a5e5630afd7104ee7be8b246ae004efb3) 
-   [`pushExternalAudioFrame`](https://docs.agora.io/en/Audio%20Broadcast/API%20Reference/java/classio_1_1agora_1_1rtc_1_1_rtc_engine.html#a9e219a679d066cfc2544b5e8f9d4d69f)

### Custom audio sink

Refer to the following steps to customize the audio sink in your project:

1. Call the `setExternalAudioSink` method to enable the external audio sink before joining a channel.
2. After you join the channel, call the `pullPlaybackAudioFrame` method to get the remote audio data.
3. Play the remote audio data on your own.

**API call sequence**

Refer to the following diagram to customize the audio sink in your project.

![](https://web-cdn.agora.io/docs-files/1569378513078)

**Sample code**

Refer to the following code to customize the audio sink in your project.

```java
// Notify the SDK that you want to use the external audio sink.
rtcEngine.setExternalAudioSink(
    true,      // Enable the external audio sink.
    44100,     // Set the audio sample rate as 8k, 16k, 32k, 44.1k or 48kHz.
    1          // Number of channels. The maximum number is 2.
);
 
// Pull the remote audio data for playback.
rtcEngine.pullPlaybackAudioFrame(
    data,             // The audio data in the format of byte[].
    lengthInByte      // Byte length in byte of the audio data.
);
```

**API reference**

- [`setExternalAudioSink`](https://docs.agora.io/en/Audio%20Broadcast/API%20Reference/java/classio_1_1agora_1_1rtc_1_1_rtc_engine.html#a270c0607d443790e92cdbd0d45ba1732)
- [`pullPlaybackAudioFrame`](https://docs.agora.io/en/Audio%20Broadcast/API%20Reference/java/classio_1_1agora_1_1rtc_1_1_rtc_engine.html#ae15064944870692e9a0a59fdc87654c4)

## Sample code

Agora provides an open-source [Customer recoder](https://github.com/AgoraIO/Advanced-Audio/tree/dev/3.1.0_android/Advanced-Audio-Android/sample-custom-recorder/) demo project on GitHub. You can download it for source code reference.

## Consideration

Customizing the audio source and sink requires you to manage audio data recording and playback on your own.

- When customizing the audio source, you need to record and process the audio data on your own.
- When customizing the audio sink, you need to process and play back the audio data on your own.
