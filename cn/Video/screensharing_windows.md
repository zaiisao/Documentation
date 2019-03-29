
---
title: 进行屏幕共享
description: 
platform: Windows
updatedAt: Fri Mar 29 2019 03:16:46 GMT+0000 (UTC)
---
# 进行屏幕共享
## 功能简介
在视频通话或互动直播中进行屏幕共享，可以将说话人或主播的屏幕内容，以视频的方式分享给其他说话人或观众观看，以提高沟通效率。

屏幕共享在如下场景中应用广泛：

- 视频会议场景中，屏幕共享可以将讲话者本地的文件、数据、网页、PPT 等画面分享给其他与会人；
- 在线课堂场景中，屏幕共享可以将老师的课件、笔记、讲课内容等画面展示给学生观看。

## 实现方法
在开始屏幕共享前，请确保你已完成环境准备、安装包获取等步骤，详见 [集成客户端](../../cn/Video/windows_video.md)。

```cpp
// cpp
// 1. 开始屏幕共享

if (lpRect != NULL) { // 共享屏幕窗口的某个区域
	rect.left = lpRect->left;
	rect.right = lpRect->right;
	rect.top = lpRect->top;
	rect.bottom = lpRect->bottom;

	lpAgoraEngine->startScreenCapture(hWnd, nFPS, &rect, nBitrate);
} else { // 通过 window ID 共享屏幕窗口。如果window ID 为  0，则代表全屏共享
	lpAgoraEngine->startScreenCapture(hWnd, nCapFPS, NULL, nBitrate);
}

// 2. 停止屏幕共享

lpAgoraEngine->stopScreenCapture();
```

### API 参考
* [`startScreenCapture`](https://docs.agora.io/cn/Video/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine.html#af71935ad435402f776bcfc2be3cf687f)
* [`stopScreenCapture`](https://docs.agora.io/cn/Video/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine.html#a77412ab7c8653289a28212e60bd00673)
* [`updateScreenCaptureRegion`](https://docs.agora.io/cn/Video/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine.html#a99ce13ce3b9b2c65e5ec35b9861b56e3)

## 开发注意事项
在 Windows 平台上进行屏幕共享时，如果共享的是 QQ 聊天窗口会导致黑屏。
