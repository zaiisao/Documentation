
---
title: Customize the Audio/Video Source and Renderer
description: 
platform: Android
updatedAt: Tue Nov 27 2018 05:39:26 GMT+0000 (UTC)
---
# Customize the Audio/Video Source and Renderer
## Introduction

By default, An App uses the internal audio and video modules for capturing and rendering during real-time communication. If developers hope to use external audio or video source and renderer, this page shows how to use the APIs provided by Agora SDK to customize the audio and video source and renderer.

**Customizing audio and video source and renderer** mainly applies to the following scenarios:

- When the audio or video source captured by the internal modules can not meet the needs of the developers. For example, for the purpose of image enhancement, developers need to process the captured video frame with a preprocessing library.
- When an App contains its own audio or video module and wants a cusmized source for  code reuse.
- When developers want to use non-camera source, such as recorded screen data.
- When developers need flexible device resource allocation to avoid conflicts with other services.

## Implementaions

Before proceeding, ensure that you have finished preparing the development environment. See [Integrate the SDK](../../en/Interactive%20Broadcast/android_video.md) for details.

### Customize the Audio Source

Use the Push method to customize the audio source, where the SDK conducts no data processing to the audio frame, such as noise reduction.

```java
    //java
    //Enable the external audio source mode
    rtcEngine.setExternalAudioSource(
        true,      // enable the external audio source
        44100,     // sampling rate. Set it as 8k, 16k, 32k, 44.1k or 48kHz
        1          // the number of external audio channels. The maximum value is 2
    );

    // To continually push the enternal audio frame
    rtcEngine.pushExternalAudioFrame(
        data,             // the audio data in the formart of byte[]
        timestamp         // the timestamp of the audio frame
    );
```

**Relevant APIs and descriptions**
*  [`pushExternalAudioFrame`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/java/classio_1_1agora_1_1rtc_1_1_rtc_engine.html#a9e219a679d066cfc2544b5e8f9d4d69f)

### Customize the Video Source

Agora SDK provides two methods to customze the video source:

- The MediaIO method (Recommended).
- The Push method. This method skips processing the video frame, and works best for clients with frame optimization capacity.

#### The MediaIO Method

Use the IVideoSource interface in MediaIO to customize the video source. This method sends the external video frame to the server, and developers need to implement local rendering if local preview is to be enabled.



```java
    //java
    IVideoFrameConsumer mConsumer;
    boolean mHasStarted;

    // Create a VideoSource instance
    VideoSource source = new VideoSource() {
        @Override
        public int getBufferType() {
            // Get the current frame type. 
            // The SDK uses different methods to process different frame types.
            // If you want to switch to another VideoSource type, create another instance
            // There are three types of video frame type
            return BufferType.BYTE_ARRAY;
            // return BufferType.TEXTURE
            // return BufferType.BYTE_BUFFER;
        }

        @Override
         public boolean onInitialize(IVideoFrameConsumer consumer) {
            // consumer was created by the SDK.
            // Ensure to save it in the life cycle of the VideoSource。
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

    // Change the inputting video stream to the VideoSource instance
    rtcEngine.setVideoSource(source);

    // After receving the video frame data, use the consumer class to send the data
    // Choose differnet methods according to the frame type.
    // Suppose the current frame type is byte array, i.e. NV21
    if (mHasStarted && mConsumer != null) {
        mConsumer.consumeByteArrayFrame(data, AgoraVideoFrame.NV21, width, height, rotation, timestamp);
    }
```

**Relevant APIs and descriptions**

* [`setlVideoSource`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/java/classio_1_1agora_1_1rtc_1_1_rtc_engine.html#aa240e991d12b5240fc5fd362cbc0d521)
* [`IVideoSource`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/java/interfaceio_1_1agora_1_1rtc_1_1mediaio_1_1_i_video_source.html)

#### The Push Method

Compared to the MediaIO method, the Push method uses less codes, but lacks any optimization to the captured video frame. This method requires developers to do the processing.

```java
    //java
    // Notifies the SDK that the external video source is used
    rtcEngine.setExternalVideoSource(
        true，      // whether to use external video source
        false,      // whether to use texture as the output format
        true        // whether to use the push mode. True means yes. False means to use the pull mode, which is not supported
    );

    // Use the push method to send out the video frame data once it is received.
    rtcEngine.pushExternalVideoFrame(new AgoraVideoFrame(
        // Pass parameters of the frame such as format, width and height of the in the AgoraVideoFrame construct
    ));
```

**Relevant APIs and descriptions**
* [`pushExternalVideoFrame`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/java/classio_1_1agora_1_1rtc_1_1_rtc_engine.html#a6e7327f4449800a2c2ddc200eb2c0386)

The video source and renderer can be customized by switching on/off the video frame input based on the media engine callback. This simpler method can be used if an app has its own video module and only needs the Agora SDK for real-time communications. See [Customize the Video Source with the Agora Component](../../en/Interactive%20Broadcast/custom_advanced_android.md) for details.

### Customize the Video Renderer

Use the IVideoSink Interface of MediaIO to customize the video renderer.

```java
    //java
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
            // create your egl context
            // a return value of 0 means there no egl context is created in the renderer
            return 0;
        }

        @Override
        public int getBufferType() {
            return BufferType.BYTE_ARRAY;
        }

        @Override
        public int getPixelFormat() {
            return PixelFormat.NV21;
        }
    }

    rtcEngine.setLocalVideoRenderer(sink);
```

**Relevant APIs and descriptions**
* [`setLocalVideoRenderer`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/java/classio_1_1agora_1_1rtc_1_1_rtc_engine.html#ab10fd6d8dd89a5bca09b115ecd9e3416)
* [`setRemoteVideoRenderer`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/java/classio_1_1agora_1_1rtc_1_1_rtc_engine.html#a0da32c040cb9d987df2950b83459ba56)
* [`IVideoSink`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/java/interfaceio_1_1agora_1_1rtc_1_1mediaio_1_1_i_video_sink.html)


Agora provides a helper class and sample code for developers to customize the video renderer. See [Customize the Video Renderer with Agora Components](../../en/Interactive%20Broadcast/custom_advanced_android.md) for details.

Agora also provides a sample app for customizing the video source and video sink. For details, see [Agora Custom Media Device](https://github.com/AgoraIO/Advanced-Video/tree/master/Custom-Media-Device/Agora-Custom-Media-Device-Android).

## Consideration

Customizing audio/video source and renderer is an advanced feature provided by Agora SDK. To develop this function, we believe it necessary that you have adequate knowledge and experience in audio and video application development.
