
---
title: 修改音视频原始数据
description: 
platform: Android
updatedAt: Tue Jul 02 2019 08:21:59 GMT+0800 (CST)
---
# 修改音视频原始数据
Agora 原始数据接口是 SDK 库提供的高级功能，便于你（开发者）获取媒体引擎的原始语音或视频数据。开发者可以修改语音或视频数据，创建特效来更好地满足自己应用程序的特殊需求。

你可以在将数据发送给编码器前插入一个前处理阶段，对捕捉到的视频帧或语音信号进行修改。也可以在将数据发送给解码器后插入一个后处理阶段，对接收到的视频帧和语音信号进行修改。

Agora 原始数据接口是一个 C++ 接口。你需要在 Android 上使用 SDK 库的 JNI 和插件管理器。

## 修改语音数据

1.  定义 `AgoraAudioFrameObserver` 继承 `IAudioFrameObserver` \(接口类 `IAudioFrameObserver` 在 `IAgoraMediaEngine.h` 定义\) 。需要实现以下虚拟接口:

	```
	class AgoraAudioFrameObserver : public agora::media::IAudioFrameObserver
	{
		public:
		    // 10 毫秒自动回调：获取录制的声音
			virtual bool onRecordAudioFrame(AudioFrame& audioFrame) override
			{
				return true;
			}
			// 10 毫秒自动回调：获取播放的声音
			virtual bool onPlaybackAudioFrame(AudioFrame& audioFrame) override
			{
				return true;
			 }
		    // 10 毫秒自动回调：获取混音前的指定用户的声音
			virtual bool onPlaybackAudioFrameBeforeMixing(unsigned int uid, AudioFrame& audioFrame) override
			 {
				return true;
			 }
			// 10 毫秒自动回调：获取录制和播放语音混音后的数据
			virtual bool onMixedAudioFrame(AudioFrame& audioFrame) override
			 {
			 return true;
			 }

	};
	```

	上述例子为语音前处理或后处理接口返回 true。如有需要，用户可以修改数据:

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
				int64_t renderTimeMs; //timestamp of the audio frame
			 };
		public:
				virtual bool onRecordAudioFrame(AudioFrame& audioFrame) = 0;
				virtual bool onPlaybackAudioFrame(AudioFrame& audioFrame) = 0;
				virtual bool onPlaybackAudioFrameBeforeMixing(unsigned int uid, AudioFrame& audioFrame) = 0;
				virtual bool onMixedAudioFrame(AudioFrame& audioFrame) = 0;
	};
	```

2.  向媒体引擎注册语音帧观测器。在创建了 `IRtcEngine` 对象后、加入频道前，你可以注册语音观测器对象。

	```
	AgoraAudioFrameObserver s_audioFrameObserver;

	agora::util::AutoPtr<agora::media::IMediaEngine> mediaEngine;
	mediaEngine.queryInterface(engine, agora::AGORA_IID_MEDIA_ENGINE);
	if (mediaEngine)
	{
		mediaEngine->registerAudioFrameObserver(&s_audioFrameObserver);
	}
	```

> 这里的 `engine` 可以通过 [创建插件](#create_plugin) 里的 `loadAgoraRtcEnginePlugin` 传入。

### API 参考

- [onRecordAudioFrame](https://docs.agora.io/cn/Audio%20Broadcast/API%20Reference/cpp/classagora_1_1media_1_1_i_audio_frame_observer.html#ac6ab0c792420daf929fed78f9d39f728)
- [onPlaybackAudioFrame](https://docs.agora.io/cn/Audio%20Broadcast/API%20Reference/cpp/classagora_1_1media_1_1_i_audio_frame_observer.html#aefc7f9cb0d1fcbc787775588bc849bac)
- [onPlaybackAudioFrameBeforeMixing](https://docs.agora.io/cn/Audio%20Broadcast/API%20Reference/cpp/classagora_1_1media_1_1_i_audio_frame_observer.html#ae04d85a65eefec5e7c1e0477bcaa067c)
- [onMixedAudioFrame](https://docs.agora.io/cn/Audio%20Broadcast/API%20Reference/cpp/classagora_1_1media_1_1_i_audio_frame_observer.html#a78d095cbd0b8ee04f657430bb6de8100)

Call the following methods to modify the audio sample rate in the above callbacks:

- [setRecordingAudioFrameParameters](https://docs.agora.io/cn/Audio%20Broadcast/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine.html#a2c4717760b5fbf1bb8c1a3c16ca67fe5)
- [setPlaybackAudioFrameParameters](https://docs.agora.io/cn/Audio%20Broadcast/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine.html#aa5f2f6eb3db5acaaf8c40818d90694f1)
- [setMixedAudioFrameParameters](https://docs.agora.io/cn/Audio%20Broadcast/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine.html#a520ebcda51b5eb488339f3a12dfb8013)


## 创建插件

### 具体用法

Agora Native SDK 支持以动态链接库的方式加载第三方插件。具体用法如下:

1.  创建一个共享库工程，SO 的命名必须以 `libapm-` 为前缀，`so` 后缀，比如 `libapm-encryption.so` 。

2.  实现和导出相关接口函数。

	-   以下为必须实现的插件接口函数，在 SDK 加载插件时会被调用，注册成功时返回 0，失败时返回非 0。

	```
	extern "C" __attribute__((visibility("default"))) int loadAgoraRtcEnginePlugin(agora::IRtcEngine* engine);
	```

	代码示例如下:

	```
	extern "C" __attribute__((visibility("default"))) int loadAgoraRtcEnginePlugin(agora::IRtcEngine* engine)
	{
			agora::util::AutoPtr<agora::media::IMediaEngine> mediaEngine;
			mediaEngine.queryInterface(engine,agora::AGORA_IID_MEDIA_ENGINE);
			if (mediaEngine)
			{
					mediaEngine->registerVideoFrameObserver(&s_videoFrameObserver);
			}
		return 0;
	}
	```

	-   以下为可选的插件接口函数，在 SDK 卸载插件时会被调用。

	```
	extern "C" __attribute__((visibility("default"))) void unloadAgoraRtcEnginePlugin(agora::IRtcEngine* engine);
	```

	示例代码如下:

	```
	extern "C" __attribute__((visibility("default"))) void unloadAgoraRtcEnginePlugin(agora::IRtcEngine* engine)
	{
			agora::util::AutoPtr<agora::media::IMediaEngine> mediaEngine;
			mediaEngine.queryInterface(engine,agora::AGORA_IID_MEDIA_ENGINE);
			if (mediaEngine)
			{
					mediaEngine->registerVideoFrameObserver(NULL);
			}
	}
	```

### 加载插件过程

Agora Native SDK 加载插件的过程如下:

1.  SDK 引擎在初始化时搜索 native 库所在目录下的所有匹配 `libapm-xxx.so` 格式的动态链接库，如 `libapm-encryption.so` 就是一个符合格式的插件名字；

2.  SDK 用 `dlopen` 加载插件；

3.  插件必须按上述要求实现并导出 `loadAgoraRtcEnginePlugin` 接口，以及可选实现 `unloadAgoraRtcEnginePlugin` 接口。针对每个找到的插件，SDK 获取并调用 `loadAgoraRtcEnginePlugin` 函数，`loadAgoraRtcEnginePlugin` 返回 0 表示成功，如果返回非 0，SDK 会卸载该插件 \(FreeLibrary\)；该接口的输入参数为 `agora::IRtcEngine` 接口对象，插件可以在 `loadAgoraRtcEnginePlugin` 函数里使用该对象注册 `IPacketObserver`、`IAudioFrameObserver` 或 `IVideoFrameObserver` 等。

4.  SDK 引擎销毁时，调用插件的 `unloadAgoraRtcEnginePlugin` 入口函数，该函数为可选实现。



