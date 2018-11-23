
---
title: 调整通话音量
description: How to adjust volume on Windows
platform: Windows
updatedAt: Fri Nov 23 2018 02:28:22 GMT+0000 (UTC)
---
# 调整通话音量
## 功能描述

 在使用我们 SDK 时，开发者可以对 SDK 采集到的声音及 SDK 播放到声卡的声音音量进行调整，以满足产品在声音上的个性化需求。比如进行双人通话时，想实现静音操作，可以通过调整播放音量的接口将 Volume 设置为 0。



## 实现方法
开始前请确保你已完成环境准备、安装包获取等步骤，详见[集成客户端 ](../../cn/Interactive%20Broadcast/windows_video.md)。

### 调整设备音量

在 Windows 系统上，Agora 推荐使用音频设备相关 API 调节录音和播放音量：

```c++
// 设置录音音量
int setRecordingDeviceVolume(int volume);
// 设置播放音量
int setPlaybackDeviceVolume(int volume);
```

其中 `volume` 的取值范围是 [0, 255]。 0 代表设备静音，255 代表设备的最大音量。
`volume` 的值与 Windows 系统中音频设备属性里的值是一一对应的，255 对应于下图中麦克风阵列的音量值调到最右：

![](https://web-cdn.agora.io/docs-files/1542783833763)

### 调整信号音量

如果调整设备音量到极限值仍无法满足需求，Agora 提供另一套接口直接调整录制和播放声音的信号幅度，由此实现调整录音和播放的音量。
调节音量的参数值范围是 0 - 400，默认值 100 表示原始音量，即不对信号做缩放，400 表示原始音量的 4 倍（把信号放大到原始信号的 4 倍）。

```c++
RtcEngineParameters rep(*lpAgoraEngine);
  
// 将录音音量设置为 200
int ret = rep.adjustRecordingSignalVolume(200);
  
// 将播放音量设置为 200
int ret = rep.adjustPlaybackSignalVolume(200);
```

### API 参考

- [setRecordingDeviceVolume](https://docs.agora.io/cn/Interactive%20Broadcast/API%20Reference/cpp/classagora_1_1rtc_1_1_i_audio_device_manager.html#ac24424e86ded2727a532df739ebf8086)
- [setPlaybackDeviceVolume](https://docs.agora.io/cn/Interactive%20Broadcast/API%20Reference/cpp/classagora_1_1rtc_1_1_i_audio_device_manager.html#ac14a1238e83303abed2f36e02fcc9366)
- [adjustRecordingSignalVolume](https://docs.agora.io/cn/Interactive%20Broadcast/API%20Reference/cpp/classagora_1_1rtc_1_1_rtc_engine_parameters.html#aa9e9b5ae052022fe2e81232b9e6e7290)
- [adjustPlaybackSignalVolume](https://docs.agora.io/cn/Interactive%20Broadcast/API%20Reference/cpp/classagora_1_1rtc_1_1_rtc_engine_parameters.html#a8bed09e12b8e2d9934aafad50b77d364)

## 开发注意事项

- 所有相关的方法都有返回值。返回值小于 0 表示方法调用失败
- 用调整信号设置音量的方法，如果音量设置太大，由于硬件设备的限制，在某些设备上可能会出现声音不自然的效果。
