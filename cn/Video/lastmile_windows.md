
---
title: 通话前检测网络质量
description: 通话前的网络质量检测
platform: Windows
updatedAt: Mon Jan 14 2019 07:21:16 GMT+0000 (UTC)
---
# 通话前检测网络质量
## 功能描述

通话前的网络质量检测用于在加入频道前检查用户当前网络状况是否满足音频码率或者当前选定的 Video Profile 的目标码率。检测结果通过每两秒钟一个回调通知用户, 结果是一个通过丢包率和网络 jitter 计算出来的网络质量打分，主要反映用户的上行网络状况。

> 纯语音产品使用 48 kbps 的固定探测码率；视频产品会根据当前选定的 Video Profile 调整探测码率。

## 实现方法

```C++
// 实现 lastmile 回调接口
void onLastmileQuality(int quality) {
        // quality 即为当前检测到的 quality 类型，可以
        // 根据此参数执行相关逻辑。
        // ⑴ 可以选择在回调内部结束测试
}

// 在合适的时机启动 last-mile 测试，启用网络测试的情景请参考方法文档
int ret = lpAgoraEngine->enableLastmileTest();

// ⑵ 也可以选择其它时候结束测试, 结束测试之前调可能会被调用多次
int ret = lpAgoraEngine->disableLastmileTest();

```

### API 参考
* [`enableLastmileTest`](https://docs.agora.io/cn/Interactive%20Broadcast/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine.html#a2803623f129eeb92503a7a4e5a09a46d)
* [`disableLastmileTest`](https://docs.agora.io/cn/Interactive%20Broadcast/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine.html#a544fb9fda664578b80bbd7dbfffafd53)
* [`onLastmileQuality`](https://docs.agora.io/cn/Interactive%20Broadcast/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine_event_handler.html#ac7e14d1a26eb35ef236a0662d28d2b33)

## 开发注意事项

- last-mile 测试必须在加入通话频道之前。
- 第一次回调的结果有一定概率是 unknown。如果回调第一次返回时结果是 unknown, 可通过之后的几次回调获得结果。
