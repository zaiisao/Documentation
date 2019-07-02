
---
title: Modify Raw Data
description: 
platform: iOS,macOS
updatedAt: Tue Jul 02 2019 08:29:07 GMT+0800 (CST)
---
# Modify Raw Data
The Agora Raw Data interface is an advanced feature provided in the SDK library for users to obtain the raw voice or video data of the SDK engine. Developers can modify the voice or video data and create special effects to meet their needs.

You can implement a preprocessing stage before sending the data to the encoder and modifying the captured video frames or voice signals. You can also implement a postprocessing stage after sending the data to the decoder and modifying the received video frames or voice signals.

The Agora Raw Data interface is a C++ interface.

## Modify the Raw Voice Data

1. Define `AgoraAudioFrameObserver` by inheriting `IAudioFrameObserver` (the `IAudioFrameObserver` class is defined in `IAgoraMediaEngine.h`). You need the following virtual interfaces:

   For example,

   ```c++
   class AgoraAudioFrameObserver : public agora::media::IAudioFrameObserver
   {
     public:
	   // Occurs when the recorded audio frame is received.
       virtual bool onRecordAudioFrame(AudioFrame& audioFrame) override
       {
         return true;
       }
	   // Occurs when the audio playback frame is received.
       virtual bool onPlaybackAudioFrame(AudioFrame& audioFrame) override
       {
         return true;
        }
	   // Occurs when the audio playback frame of a specified user is received.
       virtual bool onPlaybackAudioFrameBeforeMixing(unsigned int uid, AudioFrame& audioFrame) override
        {
         return true;
        }
	   // Occurs when the mixed recorded and playback audio frame is received.
       virtual bool onMixedAudioFrame(AudioFrame& audioFrame) override
        {
        return true;
        }
   
   };
   ```

   This example returns true for voice preprocessing or postprocessing interfaces. Users can modify the data, if necessary:

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

> `engine` can be obtained using the following method. `kit` means `AgoraRtcEngineKit`.
>
> ```c++
> agora::IRtcEngine* rtc_engine = (agora::IRtcEngine*)kit.getNativeHandle;
> agora::util::AutoPtr<agora::media::IMediaEngine> mediaEngine;
> mediaEngine.queryInterface(rtc_engine, agora::AGORA_IID_MEDIA_ENGINE);
> ```

### API Reference

- [onRecordAudioFrame](https://docs.agora.io/en/Voice/API%20Reference/cpp/classagora_1_1media_1_1_i_audio_frame_observer.html#ac6ab0c792420daf929fed78f9d39f728)
- [onPlaybackAudioFrame](https://docs.agora.io/en/Voice/API%20Reference/cpp/classagora_1_1media_1_1_i_audio_frame_observer.html#aefc7f9cb0d1fcbc787775588bc849bac)
- [onPlaybackAudioFrameBeforeMixing](https://docs.agora.io/en/Voice/API%20Reference/cpp/classagora_1_1media_1_1_i_audio_frame_observer.html#ae04d85a65eefec5e7c1e0477bcaa067c)
- [onMixedAudioFrame](https://docs.agora.io/en/Voice/API%20Reference/cpp/classagora_1_1media_1_1_i_audio_frame_observer.html#a78d095cbd0b8ee04f657430bb6de8100)

Call the following methods to modify the audio sample rate in the above callbacks:

- [setRecordingAudioFrameParameters](https://docs.agora.io/en/Voice/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine.html#a2c4717760b5fbf1bb8c1a3c16ca67fe5)
- [setPlaybackAudioFrameParameters](https://docs.agora.io/en/Voice/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine.html#aa5f2f6eb3db5acaaf8c40818d90694f1)
- [setMixedAudioFrameParameters](https://docs.agora.io/en/Voice/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine.html#a520ebcda51b5eb488339f3a12dfb8013)

