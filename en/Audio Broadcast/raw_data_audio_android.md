
---
title: Raw Audio Data
description: 
platform: Android
updatedAt: Mon Jul 06 2020 10:35:11 GMT+0800 (CST)
---
# Raw Audio Data
## Introduction

During real-time communications, you can pre- and post-process the audio and video data and modify them for desired playback effects

The Native SDK uses the `IAudioFrameObserver` and `IVideoFrameObserver` class to provide raw data functions. You can pre-process the data before sending it to the encoder and modify the captured video frames and voice signals. You can also post-process the data after sending it to the decoder and modify the received video frames and voice signals.

This article tells how to use raw audio data with the `IAudioFrameObserver` class.

## Implementation

Before using the raw data functions, ensure that you have implemented the basic real-time communication functions in your project. For details, see [Start a Call](../../en/Audio%20Broadcast/start_call_android.md) or [Start Live Interactive Streaming](../../en/Audio%20Broadcast/start_live_android.md).

Follow these steps to implement the raw data functions in your project:

1. Call the `registerAudioFrameObserver` method to register a audio observer object before joining the channel. You need to implement an `IAudioFrameObserver` class in this method.
2. After you successfully register the observer object, the SDK triggers the  `onRecordAudioFrame`, `onPlaybackAudioFrame`, `onPlaybackAudioFrameBeforeMixing`, or `onMixedAudioFrame`  callback to send the raw audio data at the set time interval.
3. Process the captured raw data according to your needs. Send the processed data back to the SDK through the `onRecordAudioFrame`, `onPlaybackAudioFrame`, `onPlaybackAudioFrameBeforeMixing`, or `onMixedAudioFrame` callback.

### API call sequence

The following diagram shows how to implement the raw data functions in your project:

![](https://web-cdn.agora.io/docs-files/1569207426105)

### Sample code

Refer to the following sample code to implement the raw data functions in your project:

```C++
#include <jni.h>
#include <android/log.h>
#include <cstring>
  
#include "agora/IAgoraRtcEngine.h"
#include "agora/IAgoraMediaEngine.h"
  
#include "video_preprocessing_plugin_jni.h"
 
 
class AgoraAudioFrameObserver : public agora::media::IAudioFrameObserver
{
    public:
        // Occurs when the recorded audio frame is received.
        virtual bool onRecordAudioFrame(AudioFrame& audioFrame) override
        {
            return true;
        }
        // Occurs when the audio playback frame is received
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

class IAudioFrameObserver
{
    public:
            enum AUDIO_FRAME_TYPE {
            FRAME_TYPE_PCM16 = 0, // The audio frame type: PCM16
            };
    struct AudioFrame {
            AUDIO_FRAME_TYPE type;
            int samples;  // The number of samples in this frame
            int bytesPerSample; // The number of bytes per sample: 2 for PCM 16
            int channels; // The number of channels (data are interleaved if stereo)
            int samplesPerSec; // The aampling rate
            void* buffer; // The audio data buffer
            int64_t renderTimeMs; // Timestamp of the current audio frame
         };
    public:
            virtual bool onRecordAudioFrame(AudioFrame& audioFrame) = 0;
            virtual bool onPlaybackAudioFrame(AudioFrame& audioFrame) = 0;
            virtual bool onPlaybackAudioFrameBeforeMixing(unsigned int uid, AudioFrame& audioFrame) = 0;
            virtual bool onMixedAudioFrame(AudioFrame& audioFrame) = 0;
};
```

We also provide an open-source [AgoraAudioIO-Android](https://github.com/AgoraIO/Advanced-Audio/tree/dev/backup/Custom-Audio-Device/AgoraAudioIO-Android) demo project on GitHub. You can try the demo or view the source code.

### API reference

- [registerAudioFrameObserver](https://docs.agora.io/en/Audio%20Broadcast/API%20Reference/cpp/classagora_1_1media_1_1_i_media_engine.html#ae46ca0d20789787aaab2fb268a524100)
- [setRecordingAudioFrameParameters](https://docs.agora.io/en/Audio%20Broadcast/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine.html#a2c4717760b5fbf1bb8c1a3c16ca67fe5)
- [setPlaybackAudioFrameParameters](https://docs.agora.io/en/Audio%20Broadcast/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine.html#aa5f2f6eb3db5acaaf8c40818d90694f1)
- [setMixedAudioFrameParameters](https://docs.agora.io/en/Audio%20Broadcast/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine.html#a520ebcda51b5eb488339f3a12dfb8013)
- [onRecordAudioFrame](https://docs.agora.io/en/Audio%20Broadcast/API%20Reference/cpp/classagora_1_1media_1_1_i_audio_frame_observer.html#ac6ab0c792420daf929fed78f9d39f728)
- [onPlaybackAudioFrame](https://docs.agora.io/en/Audio%20Broadcast/API%20Reference/cpp/classagora_1_1media_1_1_i_audio_frame_observer.html#aefc7f9cb0d1fcbc787775588bc849bac)
- [onPlaybackAudioFrameBeforeMixing](https://docs.agora.io/en/Audio%20Broadcast/API%20Reference/cpp/classagora_1_1media_1_1_i_audio_frame_observer.html#ae04d85a65eefec5e7c1e0477bcaa067c)
- [onMixedAudioFrame](https://docs.agora.io/en/Audio%20Broadcast/API%20Reference/cpp/classagora_1_1media_1_1_i_audio_frame_observer.html#a78d095cbd0b8ee04f657430bb6de8100)


## Considerations

The methods that we use in this article are in C++. For Android, refer to the following steps to register the audio observer object.

1. Before joining a channel, create a shared library project with `libapm-` as the prefix and `so` as the suffix. For example, `libapm-encryption.so`.
2. After you successfully create the file, the RtcEngine object automatically loads the file as a plug-in.
3. After you destroy the RtcEngine object, call the `unloadAgoraRtcEnginePlugin` method to remove the plug-in.

```java
static AgoraAudioFrameObserver s_audioFrameObserver;
static agora::rtc::IRtcEngine* rtcEngine = NULL;
 
 
#ifdef __cplusplus
extern "C" {
#endif
 
int __attribute__((visibility("default"))) loadAgoraRtcEnginePlugin(agora::rtc::IRtcEngine* engine)
{
    __android_log_print(ANDROID_LOG_ERROR, "plugin", "plugin loadAgoraRtcEnginePlugin");
    rtcEngine = engine;
    return 0;
}

void __attribute__((visibility("default"))) unloadAgoraRtcEnginePlugin(agora::rtc::IRtcEngine* engine)
{
    __android_log_print(ANDROID_LOG_ERROR, "plugin", "plugin unloadAgoraRtcEnginePlugin");
    rtcEngine = NULL;
}
 
JNIEXPORT void JNICALL Java_io_agora_propeller_preprocessing_AudioPreProcessing_enablePreProcessing
  (JNIEnv *env, jobject obj, jboolean enable)
{
    if (!rtcEngine)
        return;
    agora::util::AutoPtr<agora::media::IMediaEngine> mediaEngine;
    mediaEngine.queryInterface(rtcEngine, agora::AGORA_IID_MEDIA_ENGINE);
    if (mediaEngine) {
        if (enable) {
            mediaEngine->registerAudioFrameObserver(&s_audioFrameObserver);
        } else {
            mediaEngine->registerAudioFrameObserver(NULL);
        }
    }
}
 
#ifdef __cplusplus
}
#endif
```
