
---
title: 音视频设备测试与切换
description: 
platform: Windows
updatedAt: Tue Dec 25 2018 08:37:23 GMT+0000 (UTC)
---
# 音视频设备测试与切换
## 功能描述

很多开发者在 App 上线后会收到用户反馈听不到对方说话，或看不到对方的视频画面。这些问题部分是因为客户的本地麦克风或者喇叭不可用，部分是客户的摄像头损坏。

声网提供的音视频测试与切换功能可以帮助开发者进行一些设备测试，检测摄像头是否能正常工作，检测音频设备是否可以正常录音及播放。音频测试检查系统的音频设备（耳麦、扬声器等）和网络连接是否正常。在测试过程中，用户先说一段话，在 10 秒后，声音会回放出来。如果 10 秒后用户能正常听到自己刚才说的话，就表示系统音频设备和网络连接都是正常的。

你可以在以下情况使用该功能：
    1、直播场景下，在开播前请主播自测。
    2、线上用户进行自我排查纠错。

## 实现方法

### 麦克风测试

```C++
// 1. find all audio recording devices

AAudioDeviceManager* lpDeviceManager = new AAudioDeviceManager(lpRtcEngine);
IAudioDeviceCollection *lpRecordingDeviceCollection = (*lpDeviceManager)->enumerateRecordingDevices();

UINT lCount = (UINT) lpRecordingDeviceCollection->getCount();

CString strDeviceName;
CString strDeviceID;

for (UINT nIndex = 0; nIndex < lCount; nIndex++){
    int nRet = lpRecordingDeviceCollection->getDevice(nIndex, strDeviceName, strDeviceID);
	...
}

// 2. choose one device

lpDeviceManager->setRecordingDevice(strDeviceID); // device ID choosed

// 3. implement callbacks for audio volume indication

virtual void onAudioVolumeIndication(const AudioVolumeInfo* speakers, unsigned int speakerNumber, int totalVolume) {
        (void)speakers;
        (void)speakerNumber;
        (void)totalVolume;
    }

// 4. enable audio volume indication callbacks

RtcEngineParameters rep(*lpRtcEngine);
rep.enableAudioVolumeIndication(1000 /* interval */, 10 /* smooth */);

// 5. start recording deivce testing and you will get callbacks

(*lpDeviceManager)->startRecordingDeviceTest(1000);

// 6. stop recording deivce testing

(*lpDeviceManager)->stopRecordingDeviceTest();
```




### 摄像头测试

```C++

// 1. Find all video capture devices.

AVideoDeviceManager* lpDeviceManager = new AVideoDeviceManager(lpRtcEngine);
IVideoDeviceCollection *lpVideoDeviceCollection = (*lpDeviceManager)->enumerateVideoDevices();

UINT lCount = (UINT) lpVideoDeviceCollection->getCount();

CString strDeviceName;
CString strDeviceID;

for (UINT nIndex = 0; nIndex < lCount; nIndex++){
    int nRet = lpVideoDeviceCollection->getDevice(nIndex, strDeviceName, strDeviceID);
	...
}

// 2. Choose one device.

lpDeviceManager->setDevice(strDeviceID); // The chosen device ID.

// 3. Start video capture device testing and you will see the preview.

(*lpDeviceManager)->startDeviceTest(view); // Pass a window handler on to it.

// 4. Stop video capture device testing.

(*lpDeviceManager)->stopDeviceTest();
```

### 外放测试

```C++
// 1. find all audio playback devices

AAudioDeviceManager* lpDeviceManager = new AAudioDeviceManager(lpRtcEngine);
IAudioDeviceCollection *lpPlaybackDeviceCollection = (*lpDeviceManager)->enumeratePlaybackDevices();

UINT lCount = (UINT) lpPlaybackDeviceCollection->getCount();

CString strDeviceName;
CString strDeviceID;

for (UINT nIndex = 0; nIndex < lCount; nIndex++){
    int nRet = lpPlaybackDeviceCollection->getDevice(nIndex, strDeviceName, strDeviceID);
	...
}

// 2. choose one device

lpDeviceManager->setPlaybackDevice(strDeviceID); // device ID choosed

// 3. start recording deivce testing and you will hear voice from device you choosed
// do not need to call `enableAudioVolumeIndication`, because you can directly hear the sound

(*lpDeviceManager)->startRecordingDeviceTest(1000);

// 4. stop recording deivce testing

(*lpDeviceManager)->stopRecordingDeviceTest();
```

## API 参考

* [enableAudioVolumeIndication](https://docs.agora.io/cn/Interactive%20Broadcast/API%20Reference/cpp/classagora_1_1rtc_1_1_rtc_engine_parameters.html#a59ae67333fbc61a7002a46c809e2ec4f)
* [enumerateRecordingDevices](https://docs.agora.io/cn/Interactive%20Broadcast/API%20Reference/cpp/classagora_1_1rtc_1_1_i_audio_device_manager.html#a1ea4f53d60dc91ea83960885f9ab77ee)
* [setRecordingDevice](https://docs.agora.io/cn/Interactive%20Broadcast/API%20Reference/cpp/classagora_1_1rtc_1_1_i_audio_device_manager.html#a723941355030636cd7d183d53cc7ace7)
* [enumeratePlaybackDevices](https://docs.agora.io/cn/Interactive%20Broadcast/API%20Reference/cpp/classagora_1_1rtc_1_1_i_audio_device_manager.html#aa13c99d575d89e7ceeeb139be723b18a)
* [setPlaybackDevice](https://docs.agora.io/cn/Interactive%20Broadcast/API%20Reference/cpp/classagora_1_1rtc_1_1_i_audio_device_manager.html#a1ee23eae83165a27bcbd88d80158b4f1)
* [startRecordingDeviceTest](https://docs.agora.io/cn/Interactive%20Broadcast/API%20Reference/cpp/classagora_1_1rtc_1_1_i_audio_device_manager.html#a9e732d31f179a90d388998f5b86ebf06)
* [stopRecordingDeviceTest](https://docs.agora.io/cn/Interactive%20Broadcast/API%20Reference/cpp/classagora_1_1rtc_1_1_i_audio_device_manager.html#a796e7b8a58eb303f18f04e1e9d12a94b)
* [enumerateVideoDevices](https://docs.agora.io/cn/Interactive%20Broadcast/API%20Reference/cpp/classagora_1_1rtc_1_1_i_video_device_manager.html#aef51744162ec544abf2aaf0488ca062d)
* [startDeviceTest](https://docs.agora.io/cn/Interactive%20Broadcast/API%20Reference/cpp/classagora_1_1rtc_1_1_i_video_device_manager.html#ac148cafcb191841fd4aa7f5b6166b16d)
* [stopDeviceTest](https://docs.agora.io/cn/Interactive%20Broadcast/API%20Reference/cpp/classagora_1_1rtc_1_1_i_video_device_manager.html#ae3fe9f7ad1ddf4d5cda5e30d14b9d321)

## 注意事项

音视频采集设备在很多设备和系统上都是独占的，需要确保做测试的时候没有被其他程序占用。另外这些测试也不要和加入频道一起工作
