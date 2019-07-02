
---
title: Modify Raw Data
description: 
platform: Android
updatedAt: Tue Jul 02 2019 08:30:11 GMT+0800 (CST)
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

### API Reference

- [onRecordAudioFrame](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/cpp/classagora_1_1media_1_1_i_audio_frame_observer.html#ac6ab0c792420daf929fed78f9d39f728)
- [onPlaybackAudioFrame](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/cpp/classagora_1_1media_1_1_i_audio_frame_observer.html#aefc7f9cb0d1fcbc787775588bc849bac)
- [onPlaybackAudioFrameBeforeMixing](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/cpp/classagora_1_1media_1_1_i_audio_frame_observer.html#ae04d85a65eefec5e7c1e0477bcaa067c)
- [onMixedAudioFrame](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/cpp/classagora_1_1media_1_1_i_audio_frame_observer.html#a78d095cbd0b8ee04f657430bb6de8100)

Call the following methods to modify the audio sample rate in the above callbacks:

- [setRecordingAudioFrameParameters](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine.html#a2c4717760b5fbf1bb8c1a3c16ca67fe5)
- [setPlaybackAudioFrameParameters](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine.html#aa5f2f6eb3db5acaaf8c40818d90694f1)
- [setMixedAudioFrameParameters](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine.html#a520ebcda51b5eb488339f3a12dfb8013)

## Modify the Raw Video Data

1.  Define `AgoraVideoFrameObserver` by inheriting `IVideoFrameObserver` (the `IVideoFrameObserver` class is defined in `IAgoraMediaEngine.h`). You need the following virtual interfaces:

    ```
    class AgoraVideoFrameObserver : public agora::media::IVideoFrameObserver
    {
      public:
		// Occurs when the camera captured image is received.
        virtual bool onCaptureVideoFrame(VideoFrame& videoFrame) override
        {
          return true;
        }
		// Processes the received image of the specified user.
        virtual bool onRenderVideoFrame(unsigned int uid, VideoFrame& videoFrame) override
        {
          return true;
        }
    };
    ```

    This example returns true for voice preprocessing or postprocessing interfaces. Users can modify the data, if necessary：

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

2.  Register the video frame observer to the SDK engine. After creating the `IRtcEngine` object and enabling the video mode, you can register the video frame observer object before joining a channel, .

	```
	AgoraVideoFrameObserver s_videoFrameObserver;

	agora::util::AutoPtr<agora::media::IMediaEngine> mediaEngine;
		mediaEngine.queryInterface(engine,agora::AGORA_IID_MEDIA_ENGINE);
		if (mediaEngine)
		{
			 mediaEngine->registerVideoFrameObserver(&s_videoFrameObserver);
		}
	```

> `engine` can be passed in by `loadAgoraRtcEnginePlugin` in [Creating the Agora Native SDK Plugin](#create_plugin).

<a name = "create_plugin"></a>
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



