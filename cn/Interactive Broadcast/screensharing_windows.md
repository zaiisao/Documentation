
---
title: 进行屏幕共享
description: 
platform: Windows
updatedAt: Wed Nov 21 2018 01:54:34 GMT+0000 (UTC)
---
# 进行屏幕共享
## 功能简介
在视频通话或互动直播中进行屏幕共享，可以将说话人或主播的屏幕内容，以视频的方式分享给其他说话人或观众观看，以提高沟通效率。

屏幕共享在如下场景中应用广泛：

- 视频会议场景中，屏幕共享可以将讲话者本地的文件、数据、网页、PPT 等画面分享给其他与会人；
- 在线课堂场景中，屏幕共享可以将老师的课件、笔记、讲课内容等画面展示给学生观看。

## 实现方法
在开始屏幕共享前，请确保你已完成环境准备、安装包获取等步骤，详见 [集成客户端](../../cn/Interactive%20Broadcast/windows_video.md)。

```cpp
//cpp
// 1. start screensharing

if (lpRect != NULL) { // share some area on window
	rect.left = lpRect->left;
	rect.right = lpRect->right;
	rect.top = lpRect->top;
	rect.bottom = lpRect->bottom;

	lpAgoraEngine->startScreenCapture(hWnd, nFPS, &rect, nBitrate);
} else { // share the window by window id(hWnd), 0 means the full screen
	lpAgoraEngine->startScreenCapture(hWnd, nCapFPS, NULL, nBitrate);
}

// 2. stop screensharing

lpAgoraEngine->stopScreenCapture();
```

**相关 API 及链接**
* [`startScreenCapture`](https://docs.agora.io/cn/Interactive%20Broadcast/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine.html#af71935ad435402f776bcfc2be3cf687f)：开始屏幕共享
* [`stopScreenCapture`](https://docs.agora.io/cn/Interactive%20Broadcast/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine.html#a77412ab7c8653289a28212e60bd00673)：停止屏幕共享
* [`updateScreenCaptureRegion`](https://docs.agora.io/cn/Interactive%20Broadcast/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine.html#a99ce13ce3b9b2c65e5ec35b9861b56e3)：更新屏幕共享区域

