
---
title: 通话前检测网络质量
description: 通话前的网络质量检测。
platform: Android
updatedAt: Mon Jan 14 2019 09:34:16 GMT+0800 (CST)
---
# 通话前检测网络质量
## 功能描述

通话前的网络质量检测用于在加入频道前检查用户当前网络状况是否满足音频码率或者当前选定的 Video Profile 的目标码率。检测结果通过每两秒钟一个回调通知用户, 结果是一个通过丢包率和网络 jitter 计算出来的网络质量打分，主要反映用户的上行网络状况。

> 纯语音产品使用 48 kbps 的固定探测码率；视频产品会根据当前选定的 Video Profile 调整探测码率。

## 实现方法

注：所有示例代码均假设rtcEngine已经初始化完毕。

```Java
// 在合适的时机启动 lastmile 测试，启用网络测试的情景请参考方法文档
rtcEngine.enableLastmileTest();

// 位于全局 IRtcEngineEventHandler 中会发生回调
public void onLastmileQuality(int quality) {
// 若启动了 lastmile 测试，在还未获得此回调之前不要调用其它方法
// quality 即为当前检测到的 quality 类型，可以根据此参数执行相关逻辑。
// ⑴ 可以选择在回调内部结束测试
rtcEngine.disableLastmileTest();
}

// ⑵ 也可以选择其它时候结束测试, 结束测试之前 onLastmileQuality() 回调可能会被调用多次
rtcEngine.disableLastmileTest();
```

### API参考

- [`RtcEngine.enableLastmileTest()`](https://docs.agora.io/cn/Video/API%20Reference/java/classio_1_1agora_1_1rtc_1_1_rtc_engine.html#a35d045b585649ca89377ed82e9cf0662)
- [`RtcEngine.disableLastmileTest()`](https://docs.agora.io/cn/Video/API%20Reference/java/classio_1_1agora_1_1rtc_1_1_rtc_engine.html#a35d045b585649ca89377ed82e9cf0662)
- [`IRtcEngineEventHandler.onLastmileQuality()`](https://docs.agora.io/cn/Video/API%20Reference/java/classio_1_1agora_1_1rtc_1_1_i_rtc_engine_event_handler.html#a2887941e3c105c21309bd2643372e7f5)

## 开发注意事项

- last-mile 测试必须在加入通话频道之前。
- 第一次回调的结果有一定概率是 unknown。如果回调第一次返回时结果是 unknown, 可通过之后的几次回调获得结果。
