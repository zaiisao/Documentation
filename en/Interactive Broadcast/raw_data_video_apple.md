
---
title: Raw Video Data
description: 
platform: iOS,macOS
updatedAt: Mon Jul 06 2020 04:56:19 GMT+0800 (CST)
---
# Raw Video Data
## Introduction

During real-time communications, you can pre- and post-process the audio and video data and modify them for desired playback effects

The Native SDK uses the `IAudioFrameObserver` and `IVideoFrameObserver` class to provide raw data functions. You can pre-process the data before sending it to the encoder and modify the captured video frames and voice signals. You can also post-process the data after sending it to the decoder and modify the received video frames and voice signals.

This article tells how to use raw video data with the `IVideoFrameObserver` class.

## Implementation

Before using the raw data functions, ensure that you have implemented the basic real-time communication functions in your project. For details, see the following documents:
- iOS: [Start a Video Call](../../en/Interactive%20Broadcast/start_call_ios.md) or [Start Interactive Video Streaming](../../en/Interactive%20Broadcast/start_live_ios.md)
- macOS: [Start a Video Call](../../en/Interactive%20Broadcast/start_call_mac.md) or [Start Interactive Video Streaming](../../en/Interactive%20Broadcast/start_live_mac.md)

Follow these steps to implement the raw data functions in your project:

1. Call the `registerVideoFrameObserver` method to register a video observer object before joining the channel. You need to implement an `IVideoFrameObserver` class in this method.
2. After you successfully register the observer object, the SDK triggers the `onCaptureVideoFrame`, `onPreEncodeVideoFrame` or `onRenderVideoFrame` callback to send the raw video data each time when it receives a video frame.
3. Process the captured raw data according to your needs. Send the processed data back to the SDK through the `onCaptureVideoFrame` and `onRenderVideoFrame` callback.

### API call sequence

The following diagram shows how to implement the raw data functions in your project:

![](https://web-cdn.agora.io/docs-files/1578466906009)

### Sample code

Refer to the following sample code to implement the raw data functions in your project:

```C++
class AgoraVideoFrameObserver : public agora::media::IVideoFrameObserver
{
public:
    // Get the video frame captured by the local camera.
    virtual bool onCaptureVideoFrame(VideoFrame& videoFrame) override
    {
        int width = videoFrame.width;
        int height = videoFrame.height;
 
        memset(videoFrame.uBuffer, 128, videoFrame.uStride * height / 2);
        memset(videoFrame.vBuffer, 128, videoFrame.vStride * height / 2);
 
        return true;
    }
     
    // Get the video frame sent by the remote user.
    virtual bool onRenderVideoFrame(unsigned int uid, VideoFrame& videoFrame) override
    {
        return true;
    }
		
	// Get the video frame before encoding.
	virtual bool onPreEncodeVideoFrame(VideoFrame& videoFrame) override
	{
		return true;
	}
};

class IVideoFrameObserver
{
     public:
         enum VIDEO_FRAME_TYPE {
         FRAME_TYPE_YUV420 = 0,  // The video frame format is YUV 420.
         };
     struct VideoFrame {
         VIDEO_FRAME_TYPE type;
         int width;  // Width of the video frame.
         int height;  // Height of the video frame.
         int yStride;  // Line stride of the Y buffer in the YUV data.
         int uStride;  // Line stride of the U buffer in the YUV data.
         int vStride;  // Line strided of the V buffer in the YUV data.
         void* yBuffer;  // Pointer to the Y buffer in the YUV data.
         void* uBuffer;  // Pointer to the U buffer in the YUV data.
         void* vBuffer;  // Pointer to the V buffer in the YUV data.
         int rotation; // Rotation information of the video frame. You can set it as 0, 90, 180, or 270.
         int64_t renderTimeMs; // Timestamp of the video frame.
         };
     public:
         virtual bool onCaptureVideoFrame(VideoFrame& videoFrame) = 0;
         virtual bool onRenderVideoFrame(unsigned int uid, VideoFrame& videoFrame) = 0;
		 virtual bool onPreEncodeVideoFrame(VideoFrame& videoFrame) { return true; }
};
```

We also provide an open-source [Agora-Plugin-Raw-Data-API-Objective-C](https://github.com/AgoraIO/Advanced-Video/tree/master/iOS%26macOS/Agora-Plugin-Raw-Data-API-Objective-C) demo project on GitHub. You can try the demo, or view the source code in the [VideoChatViewController.m](https://github.com/AgoraIO/Advanced-Video/blob/master/iOS%26macOS/Agora-Plugin-Raw-Data-API-Objective-C/Agora-Plugin-Raw-Data-API-iOS-Objective-C/VideoChatViewController.m) file.

### API reference

 - [`registerVideoFrameObserver`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/cpp/classagora_1_1media_1_1_i_media_engine.html#a5eee4dfd1fd46e4a865feba163f3c5de)
 - [`onCaptureVideoFrame`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/cpp/classagora_1_1media_1_1_i_video_frame_observer.html#a915c673aec879dcc2b08246bb2fcf49a)
 - [`onRenderVideoFrame`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/cpp/classagora_1_1media_1_1_i_video_frame_observer.html#a966ed2459b6887c52112af638bc27c14)
 - [`onPreEncodeVideoFrame`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/cpp/classagora_1_1media_1_1_i_video_frame_observer.html#a2be41cdde19fcc0f365d4eb14a963e1c)

## Considerations

The methods that we use in this article are in C++. For iOS and macOS, refer to the following steps to register the video observer object.

```objective-c
static AgoraVideoFrameObserver* s_videoFrameObserver;
- (void)addRegiset:(AgoraRtcEngineKit *)agoraKit {
 
    // Agora Engine of C++
    agora::rtc::IRtcEngine* rtc_engine = (agora::rtc::IRtcEngine*)agoraKit.getNativeHandle;
    agora::util::AutoPtr<agora::media::IMediaEngine> mediaEngine;
    mediaEngine.queryInterface(rtc_engine, agora::AGORA_IID_MEDIA_ENGINE);
 
    if (mediaEngine) {
        s_videoFrameObserver = new AgoraVideoFrameObserver();
        mediaEngine->registerVideoFrameObserver(s_videoFrameObserver);
    }
}
 
- (void)cancelRegiset {
    agora::rtc::IRtcEngine* rtc_engine = (agora::rtc::IRtcEngine*)self.agoraKit.getNativeHandle;
    agora::util::AutoPtr<agora::media::IMediaEngine> mediaEngine;
    mediaEngine.queryInterface(rtc_engine, agora::AGORA_IID_MEDIA_ENGINE);
    mediaEngine->registerVideoFrameObserver(NULL);
}
```

## Reference

Refer to [Raw Audio Data](../../en/Interactive%20Broadcast/raw_data_audio_apple.md) if you want to implement the raw audio data function in your project.
 
 

