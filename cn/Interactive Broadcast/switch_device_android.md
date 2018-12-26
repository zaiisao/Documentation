
---
title: 音视频设备测试与切换
description: 
platform: Android
updatedAt: Wed Dec 26 2018 09:48:58 GMT+0000 (UTC)
---
# 音视频设备测试与切换
## 功能描述

很多开发者在 App 上线后会收到用户反馈听不到对方说话，或看不到对方的视频画面。这些问题部分是因为客户的本地麦克风或者喇叭不可用，部分是客户的摄像头损坏。

声网提供的音视频测试与切换功能可以帮助开发者进行一些设备测试，检测摄像头是否能正常工作，检测音频设备是否可以正常录音及播放。音频测试检查系统的音频设备（耳麦、扬声器等）和网络连接是否正常。在测试过程中，用户先说一段话，在 10 秒后，声音会回放出来。如果 10 秒后用户能正常听到自己刚才说的话，就表示系统音频设备和网络连接都是正常的。

你可以在以下情况使用该功能：
    1、直播场景下，在开播前请主播自测。
    2、线上用户进行自我排查纠错。

## 音视频设备测试

```Java
	// 开启回声测试
	rtcEngine.startEchoTest();

	// 等待并检查是否可以听到自己的声音回放

	// 停止测试
	rtcEngine.stopEchoTest();
```

### API 参考

- [`startEchoTest()`](https://docs.agora.io/cn/Video/API%20Reference/java/classio_1_1agora_1_1rtc_1_1_rtc_engine.html#ac93b84c9ebbb32f5ee304732804ec1b9)
- [`stopEchoTest()`](https://docs.agora.io/cn/Video/API%20Reference/java/classio_1_1agora_1_1rtc_1_1_rtc_engine.html#a01b8067275003c011f6d81bb41ee0fe1)

## 注意事项

- 调用 `startEchoTest` 后必须调用 `stopEchoTest` 以结束测试，否则不能进行下一次回声测试，或者调用 `joinChannel` 进行通话。
- 直播模式下，该方法仅能由用户角色为主播的用户调用。如果用户由通信模式切换到直播模式，请务必调用 `setClientRole` 方法将用户的橘色设置为。
