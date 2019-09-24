
---
title: 音视频设备测试与切换
description: 
platform: macOS
updatedAt: Wed Jul 17 2019 07:57:25 GMT+0800 (CST)
---
# 音视频设备测试与切换
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

* [`startRecordingDeviceTest`](https://docs.agora.io/cn/Video/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/startRecordingDeviceTest:)
* [`stopRecordingDeviceTest`](https://docs.agora.io/cn/Video/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/stopRecordingDeviceTest.)
* [`startPlaybackDeviceTest`](https://docs.agora.io/cn/Video/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/startPlaybackDeviceTest:)
* [`stopPlaybackDeviceTest`](https://docs.agora.io/cn/Video/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/stopPlaybackDeviceTest)
* [`startCaptureDeviceTest`](https://docs.agora.io/cn/Video/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/startCaptureDeviceTest:)
* [`stopCaptureDeviceTest`](https://docs.agora.io/cn/Video/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/stopCaptureDeviceTest)

## 注意事项

在初始化输入设备时可能失败，请查询对应的 [错误信息](https://docs.agora.io/cn/Video/API%20Reference/oc/Constants/AgoraErrorCode.html)。
