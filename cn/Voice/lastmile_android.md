
---
title: 通话前检测网络质量
description: 通话前的网络质量检测。
platform: Android
updatedAt: Mon Jul 15 2019 10:34:32 GMT+0800 (CST)
---
# 通话前检测网络质量
## 功能描述

通话前的网络质量检测用于在加入频道前检查用户当前网络状况是否满足音频码率或者当前选定的 Video Profile 的目标码率。检测结果通过每两秒钟一个回调通知用户, 结果是一个通过丢包率和网络 jitter 计算出来的网络质量打分，主要反映用户的上行网络状况。

> 纯语音产品使用 48 kbps 的固定探测码率；视频产品会根据当前选定的 Video Profile 调整探测码率。

## 实现方法

注：所有示例代码均假设rtcEngine已经初始化完毕。

```java
// 配置一个 LastmileProbeConfig 实例。参数可参考 API 文档
LastmileProbeConfig config = new LastmileProbeConfig(
  // 确认探测上行网络质量
	probeUplink(true),
  // 确认探测下行网络质量
  probeDownlink(true),
  // 期望的最高发送码率，单位为 Kbps，范围为 [100, 5000]
	expectedUplinkBitrate.1000,
  // 期望的最高接收码率，单位为 Kbps，范围为 [100, 5000]
  expectedDownlinkBitrate.1000,
);

// 加入频道前开始 Last-mile 网络探测
rtcEngine.startLastmileProbeConfig(config);

// 位于全局 IRtcEngineEventHandler 中
// 开始 Last-mile 网络探测后，约 2 秒后会发生该回调
public void onLastmileQuality(int quality)

// 位于全局 IRtcEngineEventHandler 中
// 开始 Last-mile 网络探测后，约 30 秒后会发生该回调
public void onLastmileProbeResult(LastmileProbeResult) {
	// (1)可以选择在回调内部结束测试。在测试结束前，Agora 建议不要调用其他 API 方法
	rtcEngine.stopLastmileProbeTest();
}

// (2)也可以选择其他时候结束测试。在测试结束前，Agora 建议不要调用其他 API 方法
rtcEngine.stopLastmileProbeTest();
```

### API参考

- [`startLastmileProbeTest`](https://docs.agora.io/cn/Voice/API%20Reference/java/classio_1_1agora_1_1rtc_1_1_rtc_engine.html#a81c6541685b1c4437d9779a095a0f871)
- [`stopLastmileProbeTest`](https://docs.agora.io/cn/Voice/API%20Reference/java/classio_1_1agora_1_1rtc_1_1_rtc_engine.html#ae21243b8da8bda9ee5f3a00621cbf959)
- [`onLastmileQuality`](https://docs.agora.io/cn/Voice/API%20Reference/java/classio_1_1agora_1_1rtc_1_1_i_rtc_engine_event_handler.html#a2887941e3c105c21309bd2643372e7f5)
- [`onLastmileProbeResult`](https://docs.agora.io/cn/Voice/API%20Reference/java/classio_1_1agora_1_1rtc_1_1_i_rtc_engine_event_handler.html#ad74a9120325bfeccdec4af4611110281)

## 开发注意事项

- last-mile 测试必须在加入通话频道之前。
- 第一次回调的结果有一定概率是 unknown。如果回调第一次返回时结果是 unknown, 可通过之后的几次回调获得结果。
