
---
title: Customize the Audio Source
description: 
platform: Android
updatedAt: Fri Sep 20 2019 08:34:35 GMT+0800 (CST)
---
# Customize the Audio Source
## Introduction

By default, an app uses the internal audio modules for capturing and rendering during real-time communication. You can use an external audio source and renderer. This page shows how to use the methods provided by Agora SDK to customize the audio source and renderer.

**Customizing the audio source** mainly applies to the following scenarios:

* When the audio source captured by the internal modules do not meet your needs. 
* When an app has its own audio module and uses a customized source for code reuse.
* When you need flexible device resource allocation to avoid conflicts with other services.

## Implementation

Ensure that you prepared the development environment. See [Integrate the SDK](../../en/Audio%20Broadcast/android_audio.md).

### Customize the Audio Source

Use the push method to customize the audio source, where by default the SDK conducts no data processing to the audio frame, such as noise reduction.  Implement noise reduction on your own if you have such requirements.

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


#### API Reference
*  [`pushExternalAudioFrame`](https://docs.agora.io/en/Audio%20Broadcast/API%20Reference/java/classio_1_1agora_1_1rtc_1_1_rtc_engine.html#a9e219a679d066cfc2544b5e8f9d4d69f)


## Consideration

Customizing the audio source and renderer is an advanced feature provided by Agora SDK. Ensure that you are experienced in audio application development.
