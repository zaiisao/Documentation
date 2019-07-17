
---
title: 音视频设备测试与切换
description: 
platform: macOS
updatedAt: Wed Jul 17 2019 06:25:56 GMT+0800 (CST)
---
# 音视频设备测试与切换
## 功能描述

声网提供的音视频测试与切换功能可以帮助开发者进行一些设备测试，检测摄像头是否能正常工作，检测音频设备是否可以正常录音及播放。音频测试检查系统的音频设备（耳麦、扬声器等）和网络连接是否正常。

你可以在以下情况使用该功能：
    1、直播场景下，在开播前请主播自测。
    2、线上用户进行自我排查纠错。

## 实现方法

### 麦克风测试

```swift	
// starts the microphone test
agoraKit.startRecordingDeviceTest(1000)
	
// stops the microphone test
agoraKit.stopRecordingDeviceTest()
```

```oc
[agoraKit startRecordingDeviceTest: 1000];

[agoraKit stopRecordingDeviceTest];
```


### 外放测试

```swift
// swift
// starts a playback device test
agoraKit.startPlaybackDeviceTest("audio file path")
	
// stops a playback device test
agoraKit.stopPlaybackDeviceTest()
```

```oc
// objective-c
// starts a playback device test
[agoraKit startPlaybackDeviceTest: @"audio file path"];

// stops a playback device test
[agoraKit stopPlaybackDeviceTest];
```

### API 参考

* [`startRecordingDeviceTest`](https://docs.agora.io/cn/Interactive%20Broadcast/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/startRecordingDeviceTest:)
* [`stopRecordingDeviceTest`](https://docs.agora.io/cn/Interactive%20Broadcast/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/stopRecordingDeviceTest.)
* [`startPlaybackDeviceTest`](https://docs.agora.io/cn/Interactive%20Broadcast/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/startPlaybackDeviceTest:)
* [`stopPlaybackDeviceTest`](https://docs.agora.io/cn/Interactive%20Broadcast/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/stopPlaybackDeviceTest)

## 注意事项

在初始化输入设备时可能失败，请查询对应的 [错误信息](https://docs.agora.io/cn/Interactive%20Broadcast/API%20Reference/oc/Constants/AgoraErrorCode.html)。
