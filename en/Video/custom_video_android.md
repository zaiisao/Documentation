
---
title: Custom Video Source and Renderer
description: 
platform: Android
updatedAt: Mon Sep 23 2019 01:34:23 GMT+0800 (CST)
---
# Custom Video Source and Renderer
## Introduction

By default, the Agora SDK uses default audio and video modules for capturing and rendering in real-time communications. 

However, the default modules might not meet your development requirements, such as in the following scenarios:

- Your app has its own audio or video module.
- You want to use a non-camera source, such as recorded screen data.
- You need to process the captured video with a pre-processing library for functions such as image enhancement.
- You need flexible device resource allocation to avoid conflicts with other services.

This article tells you how to use the Agora SDK to customize the video source and renderer.

## Implementation

Before customizing the video source or renderer, ensure that you hava implemented the basic real-time communication functions in your project. For details, see [Start a Call](../../en/Video/start_call_android.md) or [Start a Live Broadcast](../../en/Video/start_live_android.md).

### Customize the Audio Source

Use the push method to customize the audio source, where by default the SDK conducts no data processing to the audio frame, such as noise reduction. Implement noise reduction on your own if you have such requirements.

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
*  [`pushExternalAudioFrame`](https://docs.agora.io/en/Video/API%20Reference/java/classio_1_1agora_1_1rtc_1_1_rtc_engine.html#a9e219a679d066cfc2544b5e8f9d4d69f)

### Customize the Video Source

The Agora SDK provides two methods to customize the video source:

- The IVideoSource interface of MediaI. You can use this interface with the IVideoSink interface for more varied scenarios.
- Push method. This method is easy to use and works best for clients with frame optimization capacity.

#### MediaIO Method

Use the IVideoSource interface in MediaIO to customize the video source. This method sends the external video frame to the SDK, and you need to implement local rendering if the local preview is enabled.

```java
IVideoFrameConsumer mConsumer;
boolean mHasStarted;

// Create a VideoSource instance.
VideoSource source = new VideoSource() {
	@Override
	public int getBufferType() {
		// Get the current frame type. 
		// The SDK uses different methods to process different frame types.
		// If you want to switch to another VideoSource type, create another instance.
		// There are three video frame types: BYTE_BUFFER(1); BYTE_ARRAY(2); TEXTURE(3)
		return BufferType.BYTE_ARRAY;
	}

	@Override
 	public boolean onInitialize(IVideoFrameConsumer consumer) {
		// Consumer was created by the SDK.
		// Save it in the lifecycle of the VideoSource.
		mConsumer = consumer;
	}

	@Override
 	public boolean onStart() {
		mHasStarted = true;
	}

	@Override
  	public void onStop() {
		mHasStarted = false;
	}

	@Override
 	public void onDispose() {
		// Release the consumer
		mConsumer = null;
	}
};

// Change the inputting video stream to the VideoSource instance.
rtcEngine.setVideoSource(source);

// After receiving the video frame data, use the consumer class to send the data.
// Choose differnet methods according to the frame type.
// Suppose the current frame type is byte array, i.e. NV21.
if (mHasStarted && mConsumer != null) {
	mConsumer.consumeByteArrayFrame(data, AgoraVideoFrame.NV21, width, height, rotation, timestamp);
}
```

##### API Reference

* [`setlVideoSource`](https://docs.agora.io/en/Video/API%20Reference/java/classio_1_1agora_1_1rtc_1_1_rtc_engine.html#aa240e991d12b5240fc5fd362cbc0d521)
* [`IVideoSource`](https://docs.agora.io/en/Video/API%20Reference/java/interfaceio_1_1agora_1_1rtc_1_1mediaio_1_1_i_video_source.html)

#### Push Method

Compared to the MediaIO method, the push method uses less code but lacks any optimization of the captured video frame. This method requires you to do the processing.

```java
// Notify the SDK that the external video source is used.
rtcEngine.setExternalVideoSource(
    true，      // Whether to use an external video source.
    false,       // Whether to use texture as the output format.
    true         // Whether to use the push mode. True means yes. False means to use the pull mode, which is not supported.
    );

// Use the push method to send the video frame data once it is received.
rtcEngine.pushExternalVideoFrame(new AgoraVideoFrame(
    // Pass parameters of the frame (such as format and width and height) in the AgoraVideoFrame construct.
));
```

##### API Reference
* [`pushExternalVideoFrame`](https://docs.agora.io/en/Video/API%20Reference/java/classio_1_1agora_1_1rtc_1_1_rtc_engine.html#a6e7327f4449800a2c2ddc200eb2c0386)

The video source and renderer can be customized by switching on/off the video frame input based on the media engine callback. This method can be used if an app has its own video module and only needs the Agora SDK for real-time communications. See [Customize the Video Source with the Agora Component](../../en/Video/custom_advanced_android.md).

### Customize the Video Renderer

Use the IVideoSink Interface of MediaIO to customize the video renderer.

```java
IVideoSink sink = new IVideoSink() {
	@Override
	public boolean onInitialize () {
		return true;
	}

	@Override
	public boolean onStart() {
		return true;
	}
 
	@Override
	public void onStop() {
	}
 
	@Override
	public void onDispose() {
	}
 
	@Override
	public long getEGLContextHandle() {
	}
 
	@Override
	public int getBufferType() {
		return BufferType.BYTE_ARRAY;
	}
 
	@Override
	public int getPixelFormat() {
		return PixelFormat.NV21;
	}
	
	@Override
	public void consumeByteArrayFrame(byte[] data, int format, int width, int height, int rotation, long timestamp) {
	}
}

rtcEngine.setLocalVideoRenderer(sink);
```

#### API Reference
* [`setLocalVideoRenderer`](https://docs.agora.io/en/Video/API%20Reference/java/classio_1_1agora_1_1rtc_1_1_rtc_engine.html#ab10fd6d8dd89a5bca09b115ecd9e3416)
* [`setRemoteVideoRenderer`](https://docs.agora.io/en/Video/API%20Reference/java/classio_1_1agora_1_1rtc_1_1_rtc_engine.html#a0da32c040cb9d987df2950b83459ba56)
* [`IVideoSink`](https://docs.agora.io/en/Video/API%20Reference/java/interfaceio_1_1agora_1_1rtc_1_1mediaio_1_1_i_video_sink.html)


Agora provides a helper class and sample code for developers to customize the video renderer. See [Customize the Video Renderer with Agora Components](../../en/Video/custom_advanced_android.md).

Agora provides a sample app for customizing the video source and video sink. See [Agora Custom Media Device](https://github.com/AgoraIO/Advanced-Video/tree/master/Custom-Media-Device/Agora-Custom-Media-Device-Android).

## Consideration

Customizing the audio/video source and renderer is an advanced feature provided by Agora SDK. Ensure that you are experienced in audio and video application development.
