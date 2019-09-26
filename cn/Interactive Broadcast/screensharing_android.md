
---
title: 屏幕共享
description: 
platform: Android
updatedAt: Thu Sep 26 2019 04:16:04 GMT+0800 (CST)
---
# 屏幕共享
## 功能简介

在视频通话或互动直播中进行屏幕共享，可以将说话人或主播的屏幕内容，以视频的方式分享给其他说话人或观众观看，以提高沟通效率。

屏幕共享在如下场景中应用广泛：

- 视频会议场景中，屏幕共享可以将讲话者本地的文件、数据、网页、PPT 等画面分享给其他与会人；
- 在线课堂场景中，屏幕共享可以将老师的课件、笔记、讲课内容等画面展示给学生观看。

## 实现方法

在开始屏幕共享前，请确保你已完成环境准备、安装包获取等步骤，详见 [集成客户端](../../cn/Interactive%20Broadcast/android_video.md)。

Agora SDK 不提供在 Android 平台实现屏幕共享的 API，但你可以结合 Android 的系统 API 实现该功能。
* 利用 MediaProjection/VirtualDisplay 拿到屏幕数据
* 维护一套 OpenGL 环境，并创建一个 SurfaceView 传给 VirtualDisplay 作为桌面图像数据的接受方
* 将从 SurfaceView 绘制回调中得到的图像数据作为外部视频源，调用 SDK 的 `pushExternalVideoFrame` 传给远端

```java
// java
MediaProjectionManager projectManager = (MediaProjectionManager) mContext.getSystemService(
Context.MEDIA_PROJECTION_SERVICE);

// 代表桌面获取的intent，并使用 startActivityForResult()调用分享功能
Intent intent = projectManager.createScreenCaptureIntent();
startActivityForResult(intent);

MediaProjection projection;
VirtualDisplay display;

// startActivityForResult()的Activity复写这个接口
@Override
onActivityResult(int requestCode, int resultCode, Intent resuleData) {
	projection = projectManager.getMediaProjection(resultCode, resultData);
	display = projection.createVirtualDisplay(name, width, height, dpi, flags, surface, callback, handler);
}

// 其中在Surface中得到的texture由SDK的方法发送出去
rtcEngine.pushExternalVideoFrame(new AgoraVideoFrame(...));

// 停止桌面共享
projection.stop();
```

你也可以参考完整的 [Agora Screen Sharing](https://github.com/AgoraIO/Advanced-Video/tree/master/Screensharing/Agora-Screen-Sharing-Android#agora-screen-sharing-android)  示例代码实现屏幕共享。

## 开发注意事项
* MediaProjection 等 API 需要 Android API level 21+，相关的使用方法请参考 [Google MediaProjection API 文档](https://developer.android.com/reference/android/media/projection/MediaProjection)。
* 此文档里的代码为缩减版，详细的设计和实现细节请参考完整的 [示例代码](https://github.com/AgoraIO/Advanced-Video/tree/master/Screensharing/Agora-Screen-Sharing-Android#agora-screen-sharing-android)。
