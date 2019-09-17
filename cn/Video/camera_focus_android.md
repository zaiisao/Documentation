
---
title: 摄像头对焦
description: 
platform: Android
updatedAt: Tue Sep 17 2019 10:25:44 GMT+0800 (CST)
---
# 摄像头对焦
## 功能简介

视频场景中，经常会使用到摄像头曝光和对焦的功能，帮助被拍摄物成像清晰、亮度适宜。
Agora SDK 在 Android 平台提供整套的摄像头管理方法，方便用户切换前后摄像头，并对摄像头的缩放、对焦和曝光进行设置。

* 曝光：支持手动选择曝光区域，摄像头自动曝光
* 对焦：支持人脸自动对焦、手动对焦

## 实现方法

在设置摄像头曝光或对焦前，请确保你已完成环境准备、安装包获取等步骤，详见[集成客户端](../../cn/Video/android_video.md)。


```java
// java
// 检测当前设备是否支持自动曝光并设置
boolean shouldSetExposure = rtcEngine.isCameraExposurePositionSupported();
if (shouldSetExposure) {
    // 假设在屏幕(50，100)的位置曝光
    float positionX = 50.0f;
    float positionY = 100.0f;
    rtcEngine.setCameraExposurePosition(positionX, positionY);
}

// 检测当前设备是否支持人脸自动对焦并设置
boolean shouldSetFaceMode = rtcEngine.isCameraAutoFocusFaceModeSupported();
rtcEngine.setCameraAutoFocusFaceModeEnabled(shouldSetFaceMode);

// 检测当前设备是否支持手动对焦并设置
boolean shouldManualFocus = rtcEngine.isCameraFocusSupported();
if (shouldManualFocus) {
    // 假设在屏幕(50，100)的位置对焦
    float positionX = 50.0f;
    float positionY = 100.0f;
    rtcEgnine.setCameraFocusPositionInPreview(positionX, positionY);
}
	
```

如果想获取摄像头的曝光或对焦的位置，也可以在 `onCameraFocusAreaChanged` 和 `onCameraExposurePositionChanged` 回调中实现。
```java
// java
public void onCameraFocusAreaChanged(rect) {
// 对焦区域更新
// 你可以在这里监听到焦点更新事件，并进行自己的逻辑处理
}

public void onCameraExposureAreaChanged(rect) {
// 曝光区域更新
// 你可以在这里监听到曝光更新事件，并进行自己的逻辑处理
}
```


### API 参考

- [`isCameraExposurePostionSupported`](https://docs.agora.io/cn/Video/API%20Reference/java/classio_1_1agora_1_1rtc_1_1_rtc_engine.html#a6818c2a98bebeb72e4802b1c585da99b)
- [`setCameraExposurePosition`](https://docs.agora.io/cn/Video/API%20Reference/java/classio_1_1agora_1_1rtc_1_1_rtc_engine.html#a0ac20919f60df42635850c53c9cbdefd)
- [`isCameraFocusSupported`](https://docs.agora.io/cn/Video/API%20Reference/java/classio_1_1agora_1_1rtc_1_1_rtc_engine.html#a0e20f04ccecfc41aa23bf63116c9a8cd)
- [`isCameraAutoFocusFaceModeSupported`](https://docs.agora.io/cn/Video/API%20Reference/java/classio_1_1agora_1_1rtc_1_1_rtc_engine.html#a09f61f738cf7d8a1902761e03a7fa600)
- [`setCameraFocusPositionInPreview`](https://docs.agora.io/cn/Video/API%20Reference/java/classio_1_1agora_1_1rtc_1_1_rtc_engine.html#aba273e4337a760d883b6c7c1344183c0)
- [`setCameraAutoFocusModeEnabled`](https://docs.agora.io/cn/Video/API%20Reference/java/classio_1_1agora_1_1rtc_1_1_rtc_engine.html#a7e67afe7ad0045448fe0bd97203afcee)
- [`onCameraExposureAreaChanged`](https://docs.agora.io/cn/Video/API%20Reference/java/classio_1_1agora_1_1rtc_1_1_i_rtc_engine_event_handler.html#ab6bc82a55191e596d5bf5a7c56bdf95e)
- [`onCameraFocusAreaChanged`](https://docs.agora.io/cn/Video/API%20Reference/java/classio_1_1agora_1_1rtc_1_1_i_rtc_engine_event_handler.html#a7af6c96c4c35587a13d1e367255e3ec0)

