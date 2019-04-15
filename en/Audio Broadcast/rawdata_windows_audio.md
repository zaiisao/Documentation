
---
title: Modify Raw Data
description: 
platform: Windows
updatedAt: Tue Feb 19 2019 10:01:16 GMT+0800 (CST)
---
# Modify Raw Data
The Agora Raw Data interface is an advanced feature provided in the SDK library for users to obtain the raw voice or video data of the SDK engine. Developers can modify the voice or video data and create special effects to meet their needs.

You can implement a preprocessing stage before sending the data to the encoder and modifying the captured video frames or voice signals. You can also implement a postprocessing stage after sending the data to the decoder and modifying the received video frames or voice signals.

The Agora Raw Data interface is a C++ interface. 

## Modify the Raw Voice Data

1.  Define <code>AgoraAudioFrameObserver</code> by inheriting <code>IAudioFrameObserver</code> (the <code>IAudioFrameObserver</code> Class is defined in <code>IAgoraMediaEngine.h</code>). You need the following virtual interfaces:


For example,

```
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

This example returns true for voice preprocessing or postprocessing interfaces. Users can modify the data, if necessary:

```
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

2.  Register the voice frame observer to the SDK engine. After creating the <code>IRtcEngine</code> object, and before joining a channel, you can register the voice frame observer object.


```
AgoraAudioFrameObserver s_audioFrameObserver;

agora::util::AutoPtr<agora::media::IMediaEngine> mediaEngine;
mediaEngine.queryInterface(*engine, agora::AGORA_IID_MEDIA_ENGINE);
if (mediaEngine)
{
  mediaEngine->registerAudioFrameObserver(&s_audioFrameObserver);
}
```

> <code>engine</code> can be passed in by <code>loadAgoraRtcEnginePlugin</code> in [Create the Agora Native SDK Plugin](#create_plugin).

## Modify the Raw Video Data

1.  Define <code>AgoraVideoFrameObserver</code> by inheriting <code>IVideoFrameObserver</code> (the <code>IVideoFrameObserver</code> class is defined in <code>IAgoraMediaEngine.h</code>). You need the following virtual interfaces:

    For example,

```
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


This example returns true for voice preprocessing or postprocessing interfaces. Users can modify the data, if necessary:

```
class IVideoFrameObserver
{
  public:
    enum VIDEO_FRAME_TYPE {
    FRAME_TYPE_YUV420 = 0,  //YUV 420 format
    };
  struct VideoFrame {
    VIDEO_FRAME_TYPE type;
    int width;  //width of the video frame
    int height;  //height of the video frame
    int yStride;  //stride of the Y data buffer
    int uStride;  //stride of the U data buffer
    int vStride;  // stride of the V data buffer
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

2.  Register the video frame observer to the SDK engine. After creating the <code>IRtcEngine</code> object and enabling the video mode, and before joining a channel, you can register the video frame observer object.


```
AgoraVideoFrameObserver s_videoFrameObserver;

agora::util::AutoPtr<agora::media::IMediaEngine> mediaEngine;
  mediaEngine.queryInterface(*engine,agora::AGORA_IID_MEDIA_ENGINE);
  if (mediaEngine)
  {
     mediaEngine->registerVideoFrameObserver(&s_videoFrameObserver);
  }
```

> <code>engine</code> can be passed in by <code>loadAgoraRtcEnginePlugin</code> in [Create the Agora Native SDK Plugin](#create_plugin).

<a name="create_plugin"></a>
## Create the Agora Native SDK Plugin

The Agora Native SDK supports loading the third-party plugin by using dynamically linked libraries. The usage is as follows:

1.  Create a Windows DLL project. The DLL file must be created with <code>libapm-</code> as the prefix and <code>dll</code> as the suffix. For example, <code>libapm-encryption.dll</code>.

2.  Implement and export the related interface functions.


The following plugin interface function is mandatory and called when the Agora SDK loads the plugin. It returns 0 upon a successful registration, and the registration fails when it does not return 0.

```
extern "C" __declspec(dllexport) __cdecl int loadAgoraRtcEnginePlugin(agora::IRtcEngine* engine)
{
    agora::util::AutoPtr<agora::media::IMediaEngine> mediaEngine;
    mediaEngine.queryInterface(*engine,agora::AGORA_IID_MEDIA_ENGINE);
    if (mediaEngine)
    {
        mediaEngine->registerVideoFrameObserver(&s_videoFrameObserver);
    }
  return 0;
}
```

The following plugin interface function is optional and can be called when the Agora SDK removes the plugin.

```
extern "C" __declspec(dllexport) __cdecl void unloadAgoraRtcEnginePlugin(agora::IRtcEngine* engine)
{
    agora::util::AutoPtr<agora::media::IMediaEngine> mediaEngine;
    mediaEngine.queryInterface(*engine,agora::AGORA_IID_MEDIA_ENGINE);
    if (mediaEngine)
    {
        mediaEngine->registerVideoFrameObserver(NULL);
    }
}
```

The process of the Agora Native SDK loading the plugin is as follows:

1.  The Agora SDK engine searches all matched dynamically linked libraries in the <code>libapm-xxx.dll</code> format in the directory that contains the native libraries during initialization. For example, <code>libapm-encryption.dll</code>.

2.  The Agora SDK loads the plugin using LoadLibrary.

3.  The user must implement the plugin and export the <code>loadAgoraRtcEnginePlugin</code> interface according to the instructions above. The SDK obtains and calls <code>loadAgoraRtcEnginePlugin</code> for each plugin found. If it returns 0 after calling the <code>loadAgoraRtcEnginePlugin</code> function, then the execution succeeds. Otherwise, the SDK will remove the plugin (FreeLibrary). The input parameter of the interface is <code>agora::</code>. The IRtcEngine interface object can be used by the plugin in the <code>loadAgoraRtcEnginePlug</code> function to register <code>IPacketObserver</code>, <code>IAudioFrameObserver</code>, and <code>IVideoFrameObserver</code>.

4.  When the SDK engine is destroyed, call the optional <code>unloadAgoraRtcEnginePlugin</code> function of the plugin.



