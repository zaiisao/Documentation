
---
title: Modify Raw Data
description: 
platform: Android
updatedAt: Tue Feb 19 2019 09:54:47 GMT+0000 (UTC)
---
# Modify Raw Data
The Agora Raw Data interface is an advanced feature provided in the SDK library for users to obtain the raw voice or video data of the SDK engine. Developers can modify the voice or video data and create special effects to meet their needs.

You can implement a preprocessing stage before sending the data to the encoder and modifying the captured video frames or voice signals. You can also implement a postprocessing stage after sending the data to the decoder and modifying the received video frames or voice signals.

The Agora Raw Data interface is a C++ interface. If you use the Agora Native SDK for Android, you need to use the JNI and plug-in manager included in the SDK library.

## Modify the Raw Voice Data

1.  Define `AgoraAudioFrameObserver` by inheriting `IAudioFrameObserver` (the `IAudioFrameObserver` class is defined in `IAgoraMediaEngine.h`). You need the following virtual interface:

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
	class IvoiceFrameObserver
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

2.  Register the voice frame observer to the SDK engine. After creating the IRtcEngine object, and before joining a channel, you can register the voice frame observer object.

	```
	AgoraAudioFrameObserver s_audioFrameObserver;

	agora::util::AutoPtr<agora::media::IMediaEngine> mediaEngine;
	mediaEngine.queryInterface(engine, agora::AGORA_IID_MEDIA_ENGINE);
	if (mediaEngine)
	{
		mediaEngine->registerAudioFrameObserver(&s_audioFrameObserver);
	}
	```

> `engine` can be passed in by `loadAgoraRtcEnginePlugin` in [Creating the Agora Native SDK Plugin](#create_plugin).


## Creating the Agora Native SDK Plugin

The Agora Native SDK supports loading the third-party plugin by using dynamically linked libraries. The usage is as follows:

1.  Create a shared library project, and the SO file must be created with `libapm-` as the prefix and `so` as the suffix. For example, `libapm-encryption.so`.

2.  Implement and export the related interface functions.

   -   The following plugin interface function is mandatory and called when the Agora SDK loads the plugin. It returns 0 upon a successful registration, and the registration fails when it does not return 0.

    ```
    extern "C" __attribute__((visibility("default"))) int loadAgoraRtcEnginePlugin(agora::IRtcEngine* engine);
    ```

    **Sample Code:**

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

   -   The following plugin interface function is optional and can be called when the Agora SDK removes the plugin.

    ```
    extern "C" __attribute__((visibility("default"))) void unloadAgoraRtcEnginePlugin(agora::IRtcEngine* engine);
    ```

    **Sample Code:**

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

## Loading the Agora Native SDK Plugin

The process of the Agora Native SDK loading the plugin is as follows:

1.  The Agora SDK engine searches all matched dynamically linked libraries in the `libapm-xxx.so` format in the directory that contains the native libraries during initialization. For example, `libapm-encryption.so`.

2.  The Agora SDK loads the plugin using `dlopen`.

3.  The user must implement the plugin and export the `loadAgoraRtcEnginePlugin` interface according to the instructions above. The SDK obtains and calls `loadAgoraRtcEnginePlugin` for each plugin found. If it returns 0 after calling the `loadAgoraRtcEnginePlugin` function, then the execution succeeds. Otherwise, the SDK will remove the plugin (FreeLibrary). The input parameter of the interface is `agora::`. The `IRtcEngine` interface object can be used by the plugin in the `loadAgoraRtcEnginePlug` function to register `IPacketObserver`, `IAudioFrameObserver`, and `IVideoFrameObserver`.

4.  When the SDK engine is destroyed, call the optional `unloadAgoraRtcEnginePlugin` function of the plugin.



