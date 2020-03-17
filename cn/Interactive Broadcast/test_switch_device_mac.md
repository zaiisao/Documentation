
---
title: 音视频设备测试
description: 
platform: macOS
updatedAt: Fri Mar 06 2020 08:35:22 GMT+0800 (CST)
---
# 音视频设备测试
## 功能描述

为保证通话或直播质量，我们推荐在进入频道前进行音视频设备测试，检测麦克风、摄像头等音视频设备能否正常工作。该功能对于有高质量要求的场景，如在线教育等，尤为适用。

## 实现方法

请确保你已经了解如何[实现音视频通话](../../cn/Interactive%20Broadcast/start_call_mac.md)或[互动直播](../../cn/Interactive%20Broadcast/start_live_mac.md)。

参考以下步骤测试音视频设备：

- 选择以下一种方式测试音频设备：
	- 调用 `startEchoTestWithInterval` 测试系统的音频设备（耳麦、扬声器等）和网络连接。
	- 调用 `startRecordingDeviceTest` 测试录音设备，调用 `startPlaybackDeviceTest` 测试音频播放设备。
	- 调用 `startAudioDeviceLoopbackTest` 测试音频设备回路（包括录音设备和音频播放设备）。
- 调用 `startCaptureDeviceTest` 方法测试视频采集设备。

<div class="alert note">所有测试设备的方法都必须在加入频道之前调用。</div>

### 语音通话测试

- 用途：测试系统的音频设备（耳麦、扬声器等）和网络连接，是否正常工作。
- 测试方法和原理：调用 `startEchoTestWithInterval`，并使用该方法的 `interval` 参数设置返回测试结果的时间间隔；用户说话；SDK 在设定的时间间隔后，如果能正常播放该用户说的话，则说明音频设备及网络连接正常。

```swift
// Swift
// 开启回声测试
agoraKit.startEchoTestWithInterval(10)

// 等待并检查是否可以听到自己的声音回放

// 停止测试
agoraKit.stopEchoTest
```

```objective-c
// Objective-C
// 开启回声测试
[agoraKit startEchoTestWithInterval: 10];

// 等待并检查是否可以听到自己的声音回放

// 停止测试
[agoraKit stopEchoTest];
```

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


### 音频播放设备测试

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

### 音频设备回路测试

- 用途：测试本地音频设备回路是否正常工作。
- 测试方法和原理：调用 `startAudioDeviceLoopbackTest`；用户说话，麦克风会采集本地讲话声音，然后用扬声器播放，同时 SDK 会在 `reportAudioVolumeIndication` 回调中报告音量信息。UID 为 0 表示本地音量。完成测试后，需调用 `stopAudioDeviceLoopbackTest` 停止录制设备测试。

### 视频采集设备测试

- 用途：测试本地视频采集设备，如摄像头，是否正常功能。
- 测试方法和原理：调用 `enableVideo` 开启视频模块后，调用 `startCaptureDeviceTest`；用户指定显示图像的窗口，能看到本地采集的图像，则说明视频设备正常工作。完成测试后，需调用 `stopCaptureDeviceTest` 停止视频设备测试。

```swift
// swift
// 开始视频采集设备测试
agoraKit.startCaptureDeviceTest("view window")

// 停止视频采集设备测试
agoraKit.stopCaptureDeviceTest
```

```objective-c
// objective-c
// 开始视频采集设备测试
[agoraKit startCaptureDeviceTest: "view window"];

// 停止视频采集设备测试
[agoraKit stopCaptureDeviceTest];
```


### API 参考

* [`startEchoTestWithInterval`](https://docs.agora.io/cn/Interactive%20Broadcast/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/startEchoTestWithInterval:successBlock:)
* [`stopEchoTest`](https://docs.agora.io/cn/Interactive%20Broadcast/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/stopEchoTest)
* [`startRecordingDeviceTest`](https://docs.agora.io/cn/Interactive%20Broadcast/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/startRecordingDeviceTest:)
* [`stopRecordingDeviceTest`](https://docs.agora.io/cn/Interactive%20Broadcast/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/stopRecordingDeviceTest.)
* [`startPlaybackDeviceTest`](https://docs.agora.io/cn/Interactive%20Broadcast/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/startPlaybackDeviceTest:)
* [`stopPlaybackDeviceTest`](https://docs.agora.io/cn/Interactive%20Broadcast/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/stopPlaybackDeviceTest)
* [`startCaptureDeviceTest`](https://docs.agora.io/cn/Interactive%20Broadcast/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/startCaptureDeviceTest:)
* [`stopCaptureDeviceTest`](https://docs.agora.io/cn/Interactive%20Broadcast/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/stopCaptureDeviceTest)
* [`startAudioDeviceLoopbackTest`](https://docs.agora.io/cn/Interactive%20Broadcast/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/startAudioDeviceLoopbackTest:)
* [`stopAudioDeviceLoopbackTest`](https://docs.agora.io/cn/Interactive%20Broadcast/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/stopAudioDeviceLoopbackTest)

## 开发注意事项

- 测试结束后，请务必调用相应的 stop 方法停止测试，然后再调用 `joinChannelByToken` 加入频道。
- 
在初始化输入设备时可能失败，请查询对应的 [错误信息](https://docs.agora.io/cn/Interactive%20Broadcast/API%20Reference/oc/Constants/AgoraErrorCode.html)。
