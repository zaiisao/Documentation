
---
title: 屏幕共享
description: 
platform: Android
updatedAt: Thu Jun 11 2020 09:38:54 GMT+0800 (CST)
---
# 屏幕共享
## 功能简介

在视频通话或互动直播中进行屏幕共享，可以将说话人或主播的屏幕内容，以视频的方式分享给其他说话人或观众观看，以提高沟通效率。

屏幕共享在如下场景中应用广泛：

- 视频会议场景中，屏幕共享可以将讲话者本地的文件、数据、网页、PPT 等画面分享给其他与会人；
- 在线课堂场景中，屏幕共享可以将老师的课件、笔记、讲课内容等画面展示给学生观看。

## 实现方法

在开始屏幕共享前，请确保已在你的项目中实现基本的实时音视频功能。详见开始[音视频通话](../../cn/Interactive%20Broadcast/start_call_android.md)或[开始互动直播](../../cn/Interactive%20Broadcast/start_live_android.md)。

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

同时，我们在 GitHub 提供已实现屏幕共享功能的开源示例项目。你可以前往 [Switch External Video](https://github.com/AgoraIO/Advanced-Video/blob/master/Android/sample-switch-external-video) 下载体验并参考 [`switchVideoInputActivity.java`](https://github.com/AgoraIO/Advanced-Video/blob/master/Android/sample-switch-external-video/src/main/java/io/agora/advancedvideo/switchvideoinput/SwitchVideoInputActivity.java) 文件中 `startScreenShare` 方法的代码。

## 同时共享屏幕和开启视频

我们在 GitHub 提供一个实现同时发布屏幕共享流和用户视频流功能的开源示例项目。请前往 [Agora-Screen-Sharing-Android](https://github.com/AgoraIO/Advanced-Video/tree/dev/win-screenshare/Screensharing/Agora-Screen-Sharing-Android) 下载体验。

## 开发注意事项
* MediaProjection 等 API 需要 Android API level 21+，相关的使用方法请参考 [Google MediaProjection API 文档](https://developer.android.com/reference/android/media/projection/MediaProjection)。
* 此文档里的代码为缩减版，详细的设计和实现细节请参考完整的[示例代码](https://github.com/AgoraIO/Advanced-Video/blob/master/Android/sample-switch-external-video)。
