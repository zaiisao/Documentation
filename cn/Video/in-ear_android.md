
---
title: 耳返
description: How to enable ear-monitoring and adjust the volume
platform: Android
updatedAt: Wed Sep 25 2019 10:09:14 GMT+0800 (CST)
---
# 耳返
## 功能描述
耳返主要实现监听的功能，在低延时的情况下可以给主播一个比较真实的反馈，在演唱会等专业场景里比较常用。
Agora SDK 支持耳返功能，同时支持调节耳返的音量。

## 实现方法

在实现耳返功能前，请确保已在你的项目中实现基本的实时音视频功能。详见[实现音视频通话](../../cn/Video/start_call_android.md)或[实现互动直播](../../cn/Video/start_live_android.md)。

Agora SDK 提供 `enableInEarMonitoring` 和 `setInEarMonitoringVolume` 方法给开发者根据场景需求灵活配置耳返功能，该方法在 `joinChannel` 前后均可调用。

### 示例代码

```java
 // 设置开启耳返监听功能，默认为 false
 rtcEngine.enableInEarMonitoring(true);

 // 设置耳返的音量，volume的取值范围为0 ~ 100，默认值是 100，代表麦克风录到的原始音量
 int volume = 80;
 rtcEngine.setInEarMonitoringVolume(volume);
```

### API 参考

- [`enableInEarMonitoring`](https://docs.agora.io/cn/Video/API%20Reference/java/classio_1_1agora_1_1rtc_1_1_rtc_engine.html#aeb014fcf7ec84291b9b39621e09772ea)
- [`setInEarMonitoringVolume`](https://docs.agora.io/cn/Video/API%20Reference/java/classio_1_1agora_1_1rtc_1_1_rtc_engine.html#af71afdf140660b10c4fb0c40029c432d)

## 开发注意事项

- 在 `enableInEarMonitoring` 后调用 `setInEarMonitoringVolume`。
- 以上方法都有返回值，返回值小于 0 表示方法调用失败。


