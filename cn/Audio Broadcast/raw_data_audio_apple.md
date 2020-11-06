
---
title: 原始音频数据
description: 
platform: iOS,macOS
updatedAt: Tue Mar 10 2020 00:10:10 GMT+0800 (CST)
---
# 原始音频数据
## 功能描述

音视频传输过程中，我们可以对采集到的音视频数据进行前处理和后处理，获取想要的播放效果。

对于有自行处理音视频数据需求的场景，Agora 提供原始数据功能，你可以在将数据发送给编码器前进行前处理，对捕捉到的语音信号或视频帧进行修改；也可以在将数据发送给解码器后进行后处理，对接收到的语音信号或视频帧进行修改。

Native SDK 通过提供 `IAudioFrameObserver` 类，实现采集、修改原始音频数据功能。

## 实现方法
在使用原始数据功能前，请确保你已在项目中完成基本的实时音视频功能，详见如下文档：
- iOS：[一对一通话](../../cn/Audio%20Broadcast/start_call_ios.md)或[互动直播](../../cn/Audio%20Broadcast/start_live_ios.md)
- macOS：[一对一通话](../../cn/Audio%20Broadcast/start_call_mac.md)或[互动直播](../../cn/Audio%20Broadcast/start_live_mac.md)

参考如下步骤，在你的项目中实现原始音频数据功能：

1. 加入频道前调用 `registerAudioFrameObserver` 方法注册语音观测器，并在该方法中实现一个 `IAudioFrameObserver` 类。
2. 成功注册后，SDK 通过 `onRecordAudioFrame`、`onPlaybackAudioFrame`、`onPlaybackAudioFrameBeforeMixing` 或 `onMixedAudioFrame` 回调发送采集到的原始音频数据。
3. 用户拿到音频数据后，根据场景需要自行进行处理。完成音频数据处理后，你可以直接进行自播放，或根据场景需求再通过 `onRecordAudioFrame`、`onPlaybackAudioFrame`、`onPlaybackAudioFrameBeforeMixing` 或 `onMixedAudioFrame` 回调发送给 SDK。

### API 时序图

下图展示使用原始音频数据的 API 调用时序：

![](https://web-cdn.agora.io/docs-files/1569209374977)

### 示例代码

你可以对照 API 时序图，参考下面的示例代码片段，在项目中实现音频原始数据功能：

```C++
class AgoraAudioFrameObserver : public agora::media::IAudioFrameObserver
{
    public:
        // 获取录制的音频帧
        virtual bool onRecordAudioFrame(AudioFrame& audioFrame) override
        {
            return true;
        }
        // 获取播放的音频帧
        virtual bool onPlaybackAudioFrame(AudioFrame& audioFrame) override
        {
            return true;
         }
        // 获取远端某个用户发送的音频帧
        virtual bool onPlaybackAudioFrameBeforeMixing(unsigned int uid, AudioFrame& audioFrame) override
         {
            return true;
         }
        // 获取本地录制和播放混音后的音频帧
        virtual bool onMixedAudioFrame(AudioFrame& audioFrame) override
         {
         return true;
         }
 
};

class IAudioFrameObserver
{
    public:
            enum AUDIO_FRAME_TYPE {
            FRAME_TYPE_PCM16 = 0, // 音频数据格式为 PCM 16
            };
    struct AudioFrame {
            AUDIO_FRAME_TYPE type;
            int samples;  // 该音频帧的帧数
            int bytesPerSample; // 每帧的字节数：2
            int channels; // 声道数；双声道则音频数据重叠
            int samplesPerSec; // 采样率
            void* buffer; // 音频数据 Buffer
            int64_t renderTimeMs; // 当前音频帧的时间戳
         };
    public:
            virtual bool onRecordAudioFrame(AudioFrame& audioFrame) = 0;
            virtual bool onPlaybackAudioFrame(AudioFrame& audioFrame) = 0;
            virtual bool onPlaybackAudioFrameBeforeMixing(unsigned int uid, AudioFrame& audioFrame) = 0;
            virtual bool onMixedAudioFrame(AudioFrame& audioFrame) = 0;
};
```

同时，我们在 GitHub 提供一个开源的 [AgoraAudioIO-Objective-C](https://github.com/AgoraIO/Advanced-Audio/tree/dev/backup/Custom-Audio-Device/AgoraAudioIO-Objective-C) 示例项目。你可以前往下载，或参考该项目的源代码。

### API 参考

- [registerAudioFrameObserver](https://docs.agora.io/cn/Audio%20Broadcast/API%20Reference/cpp/classagora_1_1media_1_1_i_media_engine.html#ae46ca0d20789787aaab2fb268a524100)
- [setRecordingAudioFrameParameters](https://docs.agora.io/cn/Audio%20Broadcast/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine.html#a2c4717760b5fbf1bb8c1a3c16ca67fe5)
- [setPlaybackAudioFrameParameters](https://docs.agora.io/cn/Audio%20Broadcast/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine.html#aa5f2f6eb3db5acaaf8c40818d90694f1)
- [setMixedAudioFrameParameters](https://docs.agora.io/cn/Audio%20Broadcast/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine.html#a520ebcda51b5eb488339f3a12dfb8013)
- [onRecordAudioFrame](https://docs.agora.io/cn/Audio%20Broadcast/API%20Reference/cpp/classagora_1_1media_1_1_i_audio_frame_observer.html#ac6ab0c792420daf929fed78f9d39f728)
- [onPlaybackAudioFrame](https://docs.agora.io/cn/Audio%20Broadcast/API%20Reference/cpp/classagora_1_1media_1_1_i_audio_frame_observer.html#aefc7f9cb0d1fcbc787775588bc849bac)
- [onPlaybackAudioFrameBeforeMixing](https://docs.agora.io/cn/Audio%20Broadcast/API%20Reference/cpp/classagora_1_1media_1_1_i_audio_frame_observer.html#ae04d85a65eefec5e7c1e0477bcaa067c)
- [onMixedAudioFrame](https://docs.agora.io/cn/Audio%20Broadcast/API%20Reference/cpp/classagora_1_1media_1_1_i_audio_frame_observer.html#a78d095cbd0b8ee04f657430bb6de8100)






## 开发注意事项

本文中使用的原始数据接口为 C++ 接口。如果你在 iOS 或 macOS 平台开发，请参考如下步骤注册音频数据观测器。

```objective-c
static AgoraAudioFrameObserver* s_audioFrameObserver;
- (void)addRegiset:(AgoraRtcEngineKit *)agoraKit {
 
    // Agora Engine of C++
    agora::rtc::IRtcEngine* rtc_engine = (agora::rtc::IRtcEngine*)agoraKit.getNativeHandle;
    agora::util::AutoPtr<agora::media::IMediaEngine> mediaEngine;
    mediaEngine.queryInterface(rtc_engine, agora::AGORA_IID_MEDIA_ENGINE);
 
    if (mediaEngine) {
        s_audioFrameObserver = new AgoraAudioFrameObserver();
        mediaEngine->registerAudioFrameObserver(s_audioFrameObserver);
    }
}
 
- (void)cancelRegiset {
    agora::rtc::IRtcEngine* rtc_engine = (agora::rtc::IRtcEngine*)self.agoraKit.getNativeHandle;
    agora::util::AutoPtr<agora::media::IMediaEngine> mediaEngine;
    mediaEngine.queryInterface(rtc_engine, agora::AGORA_IID_MEDIA_ENGINE);
    mediaEngine->registerAudioFrameObserver(NULL);
}
```

