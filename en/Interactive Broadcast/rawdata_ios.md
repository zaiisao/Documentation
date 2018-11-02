
---
title: Modify Raw Data
description: 
platform: iOS,macOS
updatedAt: Fri Nov 02 2018 04:09:43 GMT+0000 (UTC)
---
# Modify Raw Data
The Agora raw data interface is an advanced feature provided in the SDK library for users to obtain the voice or video raw data of the SDK engine. Developers can modify the voice or video data and create special effects to meet their needs.

You can implement a preprocessing stage before sending the data to the encoder and modifying the captured video frames or voice signals. You can also implement a postprocessing stage after sending the data to the decoder and modifying the received video frames or voice signals.

The Agora raw data interface is a C++ interface.

## Modify Voice Raw Data

1. Define `AgoraAudioFrameObserver` by inheriting `IAudioFrameObserver` (the `IAudioFrameObserver` Class is defined in `IAgoraMediaEngine.h`). You need the following virtual interfaces:

   For example,

   ```c++
   class AgoraAudioFrameObserver : public agora::media::IAudioFrameObserver
   {
     public:
       virtual bool onRecordAudioFrame(AudioFrame& audioFrame) override
       {
         return true;
       }
       virtual bool onPlaybackAudioFrame(AudioFrame& audioFrame) override
       {
         return true;
        }
       virtual bool onPlaybackAudioFrameBeforeMixing(unsigned int uid, AudioFrame& audioFrame) override
        {
         return true;
        }
       virtual bool onMixedAudioFrame(AudioFrame& audioFrame) override
        {
        return true;
        }
   
   };
   ```

   This example returns true for voice preprocessing or postprocessing interfaces. Users can modify the data if necessary:

   ```c++
   class IAudioFrameObserver
   {
     public:
         enum AUDIO_FRAME_TYPE {
         FRAME_TYPE_PCM16 = 0, //PCM 16bit little endian
         };
     struct AudioFrame {
         AUDIO_FRAME_TYPE type;
         int samples;  //number of samples in this frame
         int bytesPerSample; //number of bytes per sample: 2 for PCM 16
         int channels; // number of channels (data are interleaved if stereo)
         int samplesPerSec; //sampling rate
         void* buffer; //data buffer
         int64_t renderTimeMs;
        };
     public:
         virtual bool onRecordAudioFrame(AudioFrame& audioFrame) = 0;
         virtual bool onPlaybackAudioFrame(AudioFrame& audioFrame) = 0;
         virtual bool onPlaybackAudioFrameBeforeMixing(unsigned int uid, AudioFrame& audioFrame) = 0;
         virtual bool onMixedAudioFrame(AudioFrame& audioFrame) = 0;
   };
   ```

2. Register the voice frame observer to the SDK engine. After creating the `IRtcEngine` object, and before joining a channel, you can register the voice frame observer object.

   ```c++
   AgoraAudioFrameObserver s_audioFrameObserver;
   
   agora::util::AutoPtr<agora::media::IMediaEngine> mediaEngine;
   mediaEngine.queryInterface(*engine, agora::AGORA_IID_MEDIA_ENGINE);
   if (mediaEngine)
   {
     mediaEngine->registerAudioFrameObserver(&s_audioFrameObserver);
   }
   ```

> Here `engine` can be obtained using the following method, and kit means `AgoraRtcEngineKit`.
>
> ```c++
> agora::IRtcEngine* rtc_engine = (agora::IRtcEngine*)kit.getNativeHandle;
> agora::util::AutoPtr<agora::media::IMediaEngine> mediaEngine;
> mediaEngine.queryInterface(rtc_engine, agora::AGORA_IID_MEDIA_ENGINE);
> ```

## Modify Video Raw Data

1. Define `AgoraVideoFrameObserver` by inheriting `IVideoFrameObserver` \(the `IVideoFrameObserver` Class is defined in `IAgoraMediaEngine.h`\). You need the following virtual interfaces:

   For example,

   ```c++
   class AgoraVideoFrameObserver : public agora::media::IVideoFrameObserver
   {
     public:
       virtual bool onCaptureVideoFrame(VideoFrame& videoFrame) override
       {
         return true;
       }
       virtual bool onRenderVideoFrame(unsigned int uid, VideoFrame& videoFrame) override
       {
         return true;
       }
   };
   ```

   This example returns true for voice preprocessing or postprocessing interfaces. Users can modify the data if necessary：

   ```c++
   class IVideoFrameObserver
   {
     public:
       enum VIDEO_FRAME_TYPE {
       FRAME_TYPE_YUV420 = 0,  //YUV 420 format
       };
     struct VideoFrame {
       VIDEO_FRAME_TYPE type;
       int width;  //width of video frame
       int height;  //height of video frame
       int yStride;  //stride of Y data buffer
       int uStride;  //stride of U data buffer
       int vStride;  // stride of V data buffer
       void* yBuffer;  //Y data buffer
       void* uBuffer;  //U data buffer
       void* vBuffer;  //V data buffer
       int rotation; // rotation of this frame (0, 90, 180, 270)
       int64_t renderTimeMs;
       };
     public:
       virtual bool onCaptureVideoFrame(VideoFrame& videoFrame) = 0;
       virtual bool onRenderVideoFrame(unsigned int uid, VideoFrame& videoFrame) = 0;
   };
   ```

2. Register the video frame observer to the SDK engine. After creating the IRtcEngine object and enabling the video mode, and before joining a channel, you can register the video frame observer object.

   ```c++
   AgoraVideoFrameObserver s_videoFrameObserver;
   
   agora::util::AutoPtr<agora::media::IMediaEngine> mediaEngine;
     mediaEngine.queryInterface(*engine,agora::AGORA_IID_MEDIA_ENGINE);
     if (mediaEngine)
     {
        mediaEngine->registerVideoFrameObserver(&s_videoFrameObserver);
     }
   ```

> Here `engine` can be obtained using the following method, and kit means `AgoraRtcEngineKit`.
> ```c++
> agora::IRtcEngine* rtc_engine = (agora::IRtcEngine*)kit.getNativeHandle;
> agora::util::AutoPtr<agora::media::IMediaEngine> mediaEngine;
> mediaEngine.queryInterface(rtc_engine, agora::AGORA_IID_MEDIA_ENGINE);
> ```
