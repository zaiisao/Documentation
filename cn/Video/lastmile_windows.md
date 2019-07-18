
---
title: 通话前检测网络质量
description: 通话前的网络质量检测
platform: Windows
updatedAt: Thu Jul 18 2019 03:28:24 GMT+0800 (CST)
---
# 通话前检测网络质量
## 功能描述

在加入频道或切换角色为主播前，进行网络质量探测，可以判断或预测用户当前的网络状况是否良好，可以满足音频码率或者当前选定的视频属性的目标码率。

在对网络质量要求高的场景下，Agora 建议在加入频道前进行探测，保证通信顺畅。

> 纯语音产品使用 48 Kbps 的固定探测码率；视频产品会根据当前选定的视频属性调整探测码率。

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

- Last-mile 测试必须在加入通话频道之前。在结束测试之前，Agora 不建议调用其他 API 方法。
- `onLastmileQuality` 回调第一次报告的结果有一定概率是 unknown, 可通过之后的几次回调获得结果。
