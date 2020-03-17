
---
title: 音频设备测试与切换
description: 
platform: Android
updatedAt: Fri Mar 06 2020 08:33:23 GMT+0800 (CST)
---
# 音频设备测试与切换
## 功能描述

为保证通话或直播质量，我们推荐在进入频道前进行音视频设备测试，检测麦克风、摄像头等音视频设备能否正常工作。该功能对于有高质量要求的场景，如在线教育等，尤为适用。

Agora 通过一个 `startEchoTest` 方法，支持用户在加入频道前，启动语音通话测试。该测试目的是测试系统的音频设备（耳麦、扬声器等）和网络连接是否正常。本文介绍如何使用该方法在项目中进行音频设备检测。

## 实现方法

请确保你已经了解如何[实现音视频通话](../../cn/Voice/start_call_android.md)或[实现互动直播](../../cn/Voice/start_live_android.md)。

1. 在加入频道前，调用 `startEchoTest` 方法。调用该方法时，你需要设置一个 `intervalInSeconds` 参数，表示获取本次测试结果的间隔时间。该参数单位为秒，取值范围为 [2, 10]，默认值为 10。

2. 成功调用 `startEchoTest` 方法后，引导用户先说一段话，如果声音在设置的时间间隔后回放出来，且用户能听到自己刚才说的话，则表示系统音频设备和网络连接都是正常的。

3. 获取音频设备测试结果后，调用  `stopEchoTest` 方法停止语音通话检测，然后你可以调用 `joinChannel` 加入频道。

### 示例代码

```java
// 开启回声测试
rtcEngine.startEchoTest(10);

// 等待并检查是否可以听到自己的声音回放

// 停止测试
rtcEngine.stopEchoTest();
```

### API 参考

- [`startEchoTest`](https://docs.agora.io/cn/Voice/API%20Reference/java/classio_1_1agora_1_1rtc_1_1_rtc_engine.html#ac93b84c9ebbb32f5ee304732804ec1b9)
- [`stopEchoTest`](https://docs.agora.io/cn/Voice/API%20Reference/java/classio_1_1agora_1_1rtc_1_1_rtc_engine.html#a01b8067275003c011f6d81bb41ee0fe1)

## 开发注意事项

- 调用 `startEchoTest` 后必须调用 `stopEchoTest` 以结束测试，否则不能进行下一次回声测试，或者调用 `joinChannel` 进行通话。
- 直播场景下，`startEchoTest` 仅能由用户角色为主播的用户调用。
