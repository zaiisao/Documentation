
---
title: Raw Video Data
description: 
platform: Unity
updatedAt: Mon Jul 06 2020 04:57:34 GMT+0800 (CST)
---
# Raw Video Data
## Introduction

During real-time communications, you can pre- and post-process received audio and video data to achieve the desired playback effect.

Agora provides the raw data function for those who want to process the audio or video data according to the actual needs. This function enables you to pre-process the captured video frame or audio signal before sending it to the encoder, or to post-process the decoded video frame or audio signal.

Agora Unity SDK provides the `VideoRawDataManager` class for capturing and processing the raw video data.

## Implementation

Before using the raw data function, ensure that you have implemented the basic real-time communication functions in your project. See [Start a Video Call](https://docs.agora.io/en/Video/start_call_unity?platform=Unity) or [Start Interactive Video Streaming](https://docs.agora.io/en/Interactive%20Broadcast/start_live_unity?platform=Unity) for details.

Follow these steps to implement the raw video data functions in your project:

1. Choose either of the following methods to register a video observer:
   - Before joining a channel, call `EnableVideoObserver` to register a video observer. This method applies to scenarios where the SDK captures and renders the video.
   - Call `RegisterVideoRawDataObserver` to register a video observer. This method applies to scenarios where the SDK does not handle the video capturing and rendering, and you can handle it according to your needs.

2. After you successfully register the observer, call the following methods to listen for callbacks according to your needs:
   - Call `SetOnCaptureVideoFrameCallback` to listen for the `OnCaptureVideoFrameHandler` callback, which returns a video frame captured by the local camera.
   - Call `SetOnRenderVideoFrameCallback` to listen for the `OnRenderVideoFrameHandler` callback, which returns a video frame sent by the remote user.
3. Process the captured raw video data according to your needs. 
4. Choose either of the following methods to unregister the video observer:
   - After leaving the channel, call `DisableVideoObserver` to unregister the video observer.
   - Call `UnRegisterVideoRawDataObserver` to unregister a video observer.
   <div class="alert note">Ensure that you pair EnableVideoObserver only with DisableVideoObserver, and RegisterVideoRawDataObserver only with UnRegisterVideoRawDataObserver. Do not mix them up. Otherwise, undefined behaviors may occur.</div>

### API call sequence

The following diagram shows how to implement the raw data functions in your project:

![](https://web-cdn.agora.io/docs-files/1576228297748)

### Sample code

See the following sample code to implement the raw video data functions in your project:

```C#
void Start()
{
    // Initializes the IRtcEngine object.
    mRtcEngine = IRtcEngine.GetEngine(mVendorKey);
    // Gets the VideoRawDataManager object.
    videoRawDataManager = VideoRawDataManager.GetInstance(mRtcEngine);
    // Enables the video module.
    mRtcEngine.EnableVideo();
    // Enables the video observer.
    mRtcEngine.EnableVideoObserver();
    // Listens for the OnCaptureVideoFrameHandler delegate.
    videoRawDataManager.SetOnCaptureVideoFrameCallback(OnCaptureVideoFrameHandler);
    // Listens for the OnRenderVideoFrameHandler delegate.
    videoRawDataManager.SetOnRenderVideoFrameCallback(OnRenderVideoFrameHandler);
}

// Gets a video frame sent by the remote user.
void OnRenderVideoFrameHandler(uint uid, VideoFrame videoFrame)
{
    Debug.Log("OnRenderVideoFrameHandler");
}

// Gets a video frame captured by the local camera.
void OnCaptureVideoFrameHandler(VideoFrame videoFrame)
{
    Debug.Log("OnCaptureVideoFrameHandler");
}

public enum VIDEO_FRAME_TYPE {
    /** 0: YUV420. */
    FRAME_TYPE_YUV420 = 0, 
    /** 1: RGBA. */
    FRAME_TYPE_RGBA = 1,
};

public struct VideoFrame {
    // Supports FRAME_TYPE_RGBA only.
    public VIDEO_FRAME_TYPE type;
    // The width of the video frame.
    public int width; 
    // The height of the video frame.
    public int height; 
    // The line span of the Y buffer within the video data.
    public int yStride; 
    // The buffer of the RGBA data.
    public byte[] buffer; 
    // Sets the rotation of the video frame before rendering the video. Supports 0, 90, 180, 270 degrees clockwise.
    public int rotation;
    // The timestamp of the external audio frame.
    public long renderTimeMs;
    // Reserved for future use.
    public int avsync_type;
};
```


### API reference

- [EnableVideoObserver](https://docs.agora.io/en/Video/API%20Reference/unity/classagora__gaming__rtc_1_1_i_rtc_engine.html#ace979cd59611a0cc39e13f8ea33c0f7c)
- [DisableVideoObserver](https://docs.agora.io/en/Video/API%20Reference/unity/classagora__gaming__rtc_1_1_i_rtc_engine.html#ace613c4deed4548ee30a80a18a7007df)
- [RegisterVideoRawDataObserver](https://docs.agora.io/en/Video/API%20Reference/unity/classagora__gaming__rtc_1_1_video_raw_data_manager.html#ad2fddfb037739fdcb5cdd245caeb12f0)
- [UnRegisterVideoRawDataObserver](https://docs.agora.io/en/Video/API%20Reference/unity/classagora__gaming__rtc_1_1_video_raw_data_manager.html#ad485000862fc71f39889f826f1353ba3)
- [SetOnCaptureVideoFrameCallback](https://docs.agora.io/en/Video/API%20Reference/unity/classagora__gaming__rtc_1_1_video_raw_data_manager.html#a86b6c82c97dbe94f7a11839506a09109)
- [SetOnRenderVideoFrameCallback](https://docs.agora.io/en/Video/API%20Reference/unity/classagora__gaming__rtc_1_1_video_raw_data_manager.html#ad7516aa3de9f25b208fe2aa9baf56097)
- [OnCaptureVideoFrameHandler](https://docs.agora.io/en/Video/API%20Reference/unity/classagora__gaming__rtc_1_1_video_raw_data_manager.html#a7173eb3a85e986f50696732076c811b9)
- [OnRenderVideoFrameHandler](https://docs.agora.io/en/Video/API%20Reference/unity/classagora__gaming__rtc_1_1_video_raw_data_manager.html#a2ad89cb34bf7ca354ee71a35985bb5c7)

## Considerations

- After you call `EnableVideoObserver` or `DisableVideoObserver`, SDK calls `RegisterVideoRawDataObserver` or `UnRegisterVideoRawDataObserver` automatically, so you don't need to call them again.
