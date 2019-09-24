
---
title: 音频设备测试与切换
description: 音频设备测试与切换
platform: macOS
updatedAt: Wed Jul 17 2019 07:56:08 GMT+0800 (CST)
---
# 音频设备测试与切换
## 功能描述

为保证通话或直播质量，我们推荐在进入频道前进行音视频设备测试，检测麦克风、摄像头等音视频设备能否正常工作。该功能对于有高质量要求的场景，如在线教育等，尤为适用。

## 实现方法

### 录音设备测试

- 用途：测试本地音频录制设备，如麦克风，是否正常工作。
- 测试方法和原理：调用 `startRecordingDeviceTest`；用户说话，SDK 在 `reportAudioVolumeIndication` 回调中报告音量信息。UID 为 0 表示本地音量。完成测试后，需调用 `stopRecordingDeviceTest` 停止录制设备测试。

```swift	
// swift
// 开始录制设备测试
agoraKit.startRecordingDeviceTest(1000)
	
// 停止录制设备测试
agoraKit.stopRecordingDeviceTest()
```

```oc
// objective-c
// 开始录制设备测试
[agoraKit startRecordingDeviceTest: 1000];

// 停止录制设备测试
[agoraKit stopRecordingDeviceTest];
```



### 播放设备测试

- 用途：测试本地音频播放设备，如外放设备，是否正常工作。
- 测试方法和原理：调用 `startPlaybackDeviceTest`；用户指定播放的音频文件，能听到声音，则说明播放设备正常工作。完成测试后，需调用 `stopPlaybackDeviceTest` 停止播放设备测试。

```swift
// swift
// 开始播放设备测试
agoraKit.startPlaybackDeviceTest("audio file path")
	
// 停止播放设备测试
agoraKit.stopPlaybackDeviceTest()
```

```oc
// objective-c
// 开始播放设备测试
[agoraKit startPlaybackDeviceTest: @"audio file path"];

// 停止播放设备测试
[agoraKit stopPlaybackDeviceTest];
```

### API 参考

* [`startRecordingDeviceTest`](https://docs.agora.io/cn/Voice/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/startRecordingDeviceTest:)
* [`stopRecordingDeviceTest`](https://docs.agora.io/cn/Voice/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/stopRecordingDeviceTest.)
* [`startPlaybackDeviceTest`](https://docs.agora.io/cn/Voice/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/startPlaybackDeviceTest:)
* [`stopPlaybackDeviceTest`](https://docs.agora.io/cn/Voice/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/stopPlaybackDeviceTest)

## 开发注意事项

在初始化输入设备时可能失败，请查询对应的 [错误信息](https://docs.agora.io/cn/Voice/API%20Reference/oc/Constants/AgoraErrorCode.html)。


