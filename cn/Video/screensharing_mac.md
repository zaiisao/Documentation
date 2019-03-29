
---
title: 进行屏幕共享
description: 
platform: macOS
updatedAt: Fri Mar 29 2019 02:44:05 GMT+0000 (UTC)
---
# 进行屏幕共享
## 功能简介
在视频通话或互动直播中进行屏幕共享，可以将说话人或主播的屏幕内容，以视频的方式分享给其他说话人或观众观看，以提高沟通效率。

屏幕共享在如下场景中应用广泛：

- 视频会议场景中，屏幕共享可以将讲话者本地的文件、数据、网页、PPT 等画面分享给其他与会人；
- 在线课堂场景中，屏幕共享可以将老师的课件、笔记、讲课内容等画面展示给学生观看。

## 实现方法

在开始屏幕共享前，请确保你已完成环境准备、安装包获取等步骤，详见 [集成客户端](../../cn/Video/mac_video.md)。

Agora 在 macOS 平台上的屏幕共享主要通过如下步骤完成：
1. 通过获取视窗对应的 `windowId`；如果 `windowId` 为 0，表示共享全屏
2. 将视频源从摄像头切成该视窗进行视频传输，使远端用户可以看见被分享的窗口

```swift
// swift
// 开始窗口分享
let windowId = 0
let captureFreq = 15
let bitRate = 400
let rect = CGRect.zero
agoraKit.startScreenCapture(windowId, withCaptureFreq: captureFreq, bitRate: bitRate, andRect: rect)


// 更新该窗口分享的区域
let rect = CGRect(x: 0, y: 0, width: 100, height: 100)
agoraKit.updateScreenCaptureRegion(rect)

// 停止窗口分享
agoraKit.stopScreenCapture()
```

```objective-c
// objective-c
int windowId = 0;
int captureFreq = 15;
int bitRate = 400;
CGRect rect = CGRectZero;

// 更新该窗口分享的区域
CGRect rect = CGRectMake(0, 0, 100, 100);
[agoraKit startScreenCapture: windowId withCaptureFreq: captureFreq bitRate:(NSInteger)bitRate andRect: rect];  

// 停止窗口分享
[agoraKit stopScreenCapture];
```

### API 参考
* [`startScreenCapture:withCaptureFreq:bitrRate:andRect`](https://docs.agora.io/cn/Video/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/startScreenCapture:withCaptureFreq:bitRate:andRect:)
* [`stopScreenCapture`](https://docs.agora.io/cn/Video/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/stopScreenCapture)
* [`updateScreenCaptureRegion:`](https://docs.agora.io/cn/Video/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/updateScreenCaptureRegion:)

