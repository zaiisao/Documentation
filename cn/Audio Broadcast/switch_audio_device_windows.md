
---
title: 音频设备测试与切换
description: 
platform: Windows
updatedAt: Wed Jul 17 2019 04:30:25 GMT+0800 (CST)
---
# 音频设备测试与切换
## 功能描述

为保证通话或直播质量，我们推荐在进入频道前进行音视频设备测试，检测麦克风、摄像头等音视频设备能否正常工作。该功能对于有高质量要求的场景，如在线教育等，尤为适用。

## 实现方法

### 录制设备测试

- 用途：测试本地音频录制设备，如麦克风，是否正常工作。
- 测试方法和原理：调用 `startRecordingDeviceTest`；用户说话，SDK 在 `onAudioVolumeIndication` 回调中报告音量信息。UID 为 0 表示本地音量。完成测试后，需调用 `stopRecordingDeviceTest` 停止录制设备测试。

```C++
// 初始化参数对象
RtcEngineParameters rep(*lpRtcEngine);

// 枚举所有音频设备
AAudioDeviceManager* lpDeviceManager = new AAudioDeviceManager(lpRtcEngine);
IAudioDeviceCollection *lpRecordingDeviceCollection = (*lpDeviceManager)->enumerateRecordingDevices();

UINT lCount = (UINT) lpRecordingDeviceCollection->getCount();

CString strDeviceName;
CString strDeviceID;

for (UINT nIndex = 0; nIndex < lCount; nIndex++){
    int nRet = lpRecordingDeviceCollection->getDevice(nIndex, strDeviceName, strDeviceID);
	...
}

// 选择一个音频采集设备
lpDeviceManager->setRecordingDevice(strDeviceID); // device ID chosen

// 实现音频音量回调接口
virtual void onAudioVolumeIndication(const AudioVolumeInfo* speakers, unsigned int speakerNumber, int totalVolume) {
        (void)speakers;
        (void)speakerNumber;
        (void)totalVolume;
    }

// 启用音频音量回调功能
rep.enableAudioVolumeIndication(1000, // 回调间隔，以毫秒为单位
	10 // 顺滑度
	);

// 开始音频采集设备测试
(*lpDeviceManager)->startRecordingDeviceTest(1000);

// 停止音频采集设备测试
(*lpDeviceManager)->stopRecordingDeviceTest();
```



### 播放设备测试

- 用途：测试本地音频播放设备，如外放设备，是否正常工作。
- 测试方法和原理：调用 `startPlaybackDeviceTest`；用户指定播放的音频文件，能听到声音，则说明播放设备正常工作。完成测试后，需调用 `stopPlaybackDeviceTest` 停止播放设备测试。

```C++
// 初始化参数对象
RtcEngineParameters rep(*lpRtcEngine);

AAudioDeviceManager* lpDeviceManager = new AAudioDeviceManager(lpRtcEngine);
IAudioDeviceCollection *lpPlaybackDeviceCollection = (*lpDeviceManager)->enumeratePlaybackDevices();

UINT lCount = (UINT) lpPlaybackDeviceCollection->getCount();

CString strDeviceName;
CString strDeviceID;

for (UINT nIndex = 0; nIndex < lCount; nIndex++){
    int nRet = lpPlaybackDeviceCollection->getDevice(nIndex, strDeviceName, strDeviceID);
	...
}

// 选择一个音频播放设备
lpDeviceManager->setPlaybackDevice(strDeviceID); // device ID chosen

#ifdef UNICODE
	CHAR wdFilePath[MAX_PATH];
	::WideCharToMultiByte(CP_UTF8, 0, filePath, -1, wdFilePath, MAX_PATH, NULL, NULL);
	(*lpDeviceManager)->startPlaybackDeviceTest(wdFilePath);
#else
	(*lpDeviceManager)->startPlaybackDeviceTest(filePath);
#endif


// 停止音频播放设备测试
(*lpDeviceManager)->stopPlaybackDeviceTest();
```

### API 参考

* [`enableAudioVolumeIndication`](https://docs.agora.io/cn/Audio%20Broadcast/API%20Reference/cpp/classagora_1_1rtc_1_1_rtc_engine_parameters.html#a59ae67333fbc61a7002a46c809e2ec4f)
* [`enumerateRecordingDevices`](https://docs.agora.io/cn/Audio%20Broadcast/API%20Reference/cpp/classagora_1_1rtc_1_1_i_audio_device_manager.html#a1ea4f53d60dc91ea83960885f9ab77ee)
* [`setRecordingDevice`](https://docs.agora.io/cn/Audio%20Broadcast/API%20Reference/cpp/classagora_1_1rtc_1_1_i_audio_device_manager.html#a723941355030636cd7d183d53cc7ace7)
* [`enumeratePlaybackDevices`](https://docs.agora.io/cn/Audio%20Broadcast/API%20Reference/cpp/classagora_1_1rtc_1_1_i_audio_device_manager.html#aa13c99d575d89e7ceeeb139be723b18a)
* [`setPlaybackDevice`](https://docs.agora.io/cn/Audio%20Broadcast/API%20Reference/cpp/classagora_1_1rtc_1_1_i_audio_device_manager.html#a1ee23eae83165a27bcbd88d80158b4f1)
* [`startRecordingDeviceTest`](https://docs.agora.io/cn/Audio%20Broadcast/API%20Reference/cpp/classagora_1_1rtc_1_1_i_audio_device_manager.html#a9e732d31f179a90d388998f5b86ebf06)
* [`stopRecordingDeviceTest`](https://docs.agora.io/cn/Audio%20Broadcast/API%20Reference/cpp/classagora_1_1rtc_1_1_i_audio_device_manager.html#a796e7b8a58eb303f18f04e1e9d12a94b)

## 开发注意事项

- 请确保进行测试时，相应的音视频设备没有被其他应用程序占用。
- 测试结束后，请务必调用相应的 stop 方法停止测试，然后再调用 `joinChannel` 加入频道。
