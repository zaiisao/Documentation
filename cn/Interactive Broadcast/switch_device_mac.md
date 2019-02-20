
---
title: 音视频设备测试与切换
description: 
platform: macOS
updatedAt: Wed Feb 20 2019 08:14:21 GMT+0000 (UTC)
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
// starts a playback device test
agoraKit.startPlaybackDeviceTest("audio file path")
	
// stops a playback device test
agoraKit.stopPlaybackDeviceTest()
```

```oc
[agoraKit startPlaybackDeviceTest: @"audio file path"];

[agoraKit stopPlaybackDeviceTest];
```

### API 参考

* [`startRecordingDeviceTest`](https://docs.agora.io/cn/Interactive%20Broadcast/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/stopRecordingDeviceTest)
* [`stopPlaybackDeviceTest`](https://docs.agora.io/cn/Interactive%20Broadcast/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/stopRecordingDeviceTest)
* [`startPlaybackDeviceTest`](https://docs.agora.io/cn/Interactive%20Broadcast/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/startPlaybackDeviceTest:)
* [`stopPlaybackDeviceTest`](https://docs.agora.io/cn/Interactive%20Broadcast/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/stopPlaybackDeviceTest)

## 注意事项

在初始化输入设备时可能失败，对应的错误信息请在[开发者中心](https://docs.agora.io/cn/Interactive%20Broadcast/API%20Reference/web/interfaces/agorartc.stream.html#init)查询
