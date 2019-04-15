
---
title: 音频设备测试与切换
description: 音频设备测试与切换
platform: iOS,macOS
updatedAt: Thu Feb 21 2019 09:02:36 GMT+0800 (CST)
---
# 音频设备测试与切换
## 功能描述

很多开发者在 App 上线后会收到用户反馈听不到对方说话。这些问题部分是因为客户的本地麦克风或者喇叭不可用。声网提供的音频测试功能可以帮助开发者进行一些设备测试，检测音频设备是否可以正常录音及播放。在测试过程中，用户先说一段话，在 10 秒后，声音会回放出来。如果 10 秒后用户能正常听到自己刚才说的话，就表示系统音频设备和网络连接都是正常的。

你可以在以下情况使用该功能：
* 直播场景下，在开播前请主播自测。
* 线上用户进行自我排查纠错。

## 实现方法

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


## API 参考
* [`startRecordingDeviceTest`](https://docs.agora.io/cn/Audio%20Broadcast/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/startRecordingDeviceTest:)
* [`stopRecordingDeviceTest`](https://docs.agora.io/cn/Audio%20Broadcast/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/stopRecordingDeviceTest.)
* [`startPlaybackDeviceTest`](https://docs.agora.io/cn/Audio%20Broadcast/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/startPlaybackDeviceTest:)
* [`stopPlaybackDeviceTest`](https://docs.agora.io/cn/Audio%20Broadcast/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/stopPlaybackDeviceTest)

## 开发注意事项

在初始化输入设备时可能失败，请查询对应的 [错误信息](https://docs.agora.io/cn/Audio%20Broadcast/API%20Reference/oc/Constants/AgoraErrorCode.html)。


