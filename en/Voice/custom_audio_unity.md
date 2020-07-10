
---
title: Custom Audio Source and Renderer
description: 
platform: Unity
updatedAt: Fri Jul 10 2020 08:49:24 GMT+0800 (CST)
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

This article tells you how to use the Agora Unity SDK to customize the audio source and sink.

## Implementation

Before customizing the audio source or sink, ensure that you implement the basic real-time communication functions in your project. For details, see [Start a Call](../../en/Voice/start_call_audio_unity.md) or [Start Live Interactive Streaming](../../en/Voice/start_live_audio_unity.md).

### Custom audio source

Refer to the following steps to customize the audio source in your project:

1. Call the `SetExternalAudioSource` method to enable the external audio source before joining a channel.
2. Record and process the audio data on your own.
3. Send the audio data back to the SDK using the `PushAudioFrame` method.

**Sample code**

```c#
// Notifies the SDK that you want to use the external audio sink.
public int SetExternalAudioSource(bool enabled, int sampleRate, int channels)
{
    mRtcEngine.SetExternalAudioSource(enabled, sampleRate, channels);
}

// Pushes the remote audio data for playback.
public int PushAudioFrame(AudioFrame audioFrame)
{
    mRtcEngine.PushAudioFrame(audioFrame);
}

public struct AudioFrame 
{
    // The type of the audio frame. See #AUDIO_FRAME_TYPE.
    public AUDIO_FRAME_TYPE type;
    // The number of samples per channel in the audio frame.
    public int samples;  
    // The number of bytes per audio sample, which is usually 16-bit (2-byte).
    public int bytesPerSample;  
    // The number of audio channels.
    // - 1: Mono
    // - 2: Stereo (the data is interleaved)
    public int channels;  
    // The sample rate.
    public int samplesPerSec;  
    // The data buffer of the audio frame. When the audio frame uses a stereo channel, the data buffer is interleaved. 
    // The size of the data buffer is as follows: buffer = samples × channels × bytesPerSample.
    public byte[] buffer;  
    // The timestamp of the external audio frame. You can use this parameter for the following purposes:
    // - Restore the order of the captured audio frame.
    // - Synchronize audio and video frames in video-related scenarios, including where external video sources are used.
    public long renderTimeMs;
    // Reserved for future use.
    public int avsync_type;
};
```

**API reference**

- [`SetExternalAudioSource`](https://docs.agora.io/en/Voice/API%20Reference/unity/classagora__gaming__rtc_1_1_i_rtc_engine.html#a91a7599be9ca163f0b43c83a4b3a902e)
- [`PushAudioFrame`](https://docs.agora.io/en/Voice/API%20Reference/unity/classagora__gaming__rtc_1_1_i_rtc_engine.html#ac7340e14573a6fdf089924b228555ba7)

### Custom audio sink

Refer to the following steps to customize the audio sink in your project:

1. Call the `SetExternalAudioSink` method to enable the external audio sink before joining a channel.
2. After you join the channel, call the `PullAudioFrame` method to get the remote audio data.
3. Play the remote audio data on your own.

**Sample code**

```c#
// Notifies the SDK that you want to use the external audio sink.
public int SetExternalAudioSource(bool enabled, int sampleRate, int channels)
{
    mRtcEngine.SetExternalAudioSource(enabled, sampleRate, channels);
}

// Pulls the remote audio data for playback.
public int PullAudioFrame(IntPtr audioBuffer, int type, int samples, int bytesPerSample, int channels, int samplesPerSec, long renderTimeMs, int avsync_type)
{
    mRtcEngine.PullAudioFrame(audioBuffer, type, samples, bytesPerSample, channels, samplesPerSec, renderTimeMs, avsync_type);
}
```

**API reference**

- [`SetExternalAudioSink`](https://docs.agora.io/en/Voice/API%20Reference/unity/classagora__gaming__rtc_1_1_i_rtc_engine.html#ac767651baab2797313e4c13db7f66260)
- [`PullAudioFrame`](https://docs.agora.io/en/Voice/API%20Reference/unity/classagora__gaming__rtc_1_1_audio_raw_data_manager.html#a6aa04f6b4cf488d46bc64b39a11d891e)

## Considerations

* Ensure the accuracy and efficiency of the audio data processing in the callback methods to avoid any possible crash.
* Set the audio data to `RAW_AUDIO_FRAME_OP_MODE_READ_WRITE` if you want to read, write, and manipulate the data.
* Customizing the audio source and sink requires you to manage audio data recording and playback on your own.

	- When customizing the audio source, you need to record and process the audio data on your own.
	- When customizing the audio sink, you need to process and play back the audio data on your own.
