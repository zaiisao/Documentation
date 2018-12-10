
---
title: 摄像头对焦
description: 
platform: Android
updatedAt: Mon Dec 10 2018 02:46:34 GMT+0000 (UTC)
---
# 摄像头对焦
## 功能简介

视频场景中，经常会使用到摄像头对焦的功能，帮助被拍摄物成像清晰。
Agora SDK 在 Android 平台提供整套的摄像头管理方法，方便用户切换前后摄像头，以及对摄像头的缩放、对焦进行设置。

## 实现方法

在设置摄像头对焦前，请确保你已完成环境准备、安装包获取等步骤，详见 [集成客户端](../../cn/Video/android_video.md)。


```java
// java
// 检测当前设备是否支持人脸自动对焦并设置
boolean shouldSetFaceMode = rtcEngine.isCameraAutoFocusFaceModeSupported();
rtcEngine.setCameraAutoFocusFaceModeEnabled(shouldSetFaceMode);
	
// 检测设备是否支持手动对焦功能并设置
boolean shouldManualFocus = rtcEngine.isCameraFocusSupported();
if (shouldManualFocus) {
	// 假设在屏幕(50, 100)的位置对焦
	float positionX = 50.0f;
	float positionY = 100.0f;
	rtcEngine.setCameraFocusPositionInPreview(positionX, positionY);
}
```

### API 参考

- [`isCameraFocusSupported`](https://docs.agora.io/cn/Video/API%20Reference/java/classio_1_1agora_1_1rtc_1_1_rtc_engine.html#a0e20f04ccecfc41aa23bf63116c9a8cd)
- [`isCameraAutoFocusFaceModeSupported`](https://docs.agora.io/cn/Video/API%20Reference/java/classio_1_1agora_1_1rtc_1_1_rtc_engine.html#a09f61f738cf7d8a1902761e03a7fa600)
- [`setCameraFocusPositionInPreview`](https://docs.agora.io/cn/Video/API%20Reference/java/classio_1_1agora_1_1rtc_1_1_rtc_engine.html#aba273e4337a760d883b6c7c1344183c0)
- [`setCameraAutoFocusFaceModeEnabled`](https://docs.agora.io/cn/Video/API%20Reference/java/classio_1_1agora_1_1rtc_1_1_rtc_engine.html#a7e67afe7ad0045448fe0bd97203afcee)
